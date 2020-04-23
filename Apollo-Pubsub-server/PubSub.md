# Subscriptions sever

## Install
- npm install --save graphql-subscriptions
- npm install --save subscriptions-transport-ws

## Adding Subscriptions To Schema

- **Graphql**

    ```javascript
        type Comment {
            id: String
            content: String
        }

        type Subscription {
            commentAdded(repoFullName: String!): Comment
        }

        schema {
            query: Query
            mutation: Mutation
            subscription: Subscription
        }
    ```

- **subscriptions** **resolver** phải trả về **`AsyncIterator`**

    ```javascript
        const rootResolver = {
            Query: ()=> {...},
            Mutation: () => {...},
            Subscription:{
                commentAdded:{
                    resolve: (paload) =>{
                        return{
                            customData: paload
                        }
                    },
                    subscribe: () => pubsub.asyncIterator('commentAdded')
                }
            }
        }

    ```
- Add **resolve** method change the payload as you wish.

    ```jsx
        pubsub.publish('commentAdded', { commentAdded: { id: 1, content: 'Hello!' }})
    ```

##  PubSub

- **`PubSub`** là 1 class **exposes** a simple **`publish`** và **`subscrible`** **API**.
- Nó nằm giữa **app** và **GraphQl subscriptions**
- Nó nhận **publish command** từ **app** và đẩy nó vào **Graphql** để thực thi
- **`graphql-subscriptions`** hiển thị class PubSub default, bạn có thể sử dụng đơn giản cho việc xuất dữ liệu

    ```jsx
        import { PubSub } from 'graphql-subscriptions';

        export const pubsub = new PubSub();

        // ... Later in your code, when you want to publish data over subscription, run:

        const payload ={
            commentAdded : {
                id: '1',
                content: 'Hello!',
            }
        };

        pubsub.publish('commentAdded',payload);
    ```

## SubscriptionServer
- **`SubscriptionServer`** sẽ quán lý kết nối **WebSocket** giữa **GraphQl** và **client**

    ```jsx
        import express from 'express';
        import { graphqlExpress, graphiqlExpress,} from 'apollo-server-express';
        import bodyParser from 'body-parser';
        import cors from 'cors';
        import { execute, subscribe } from 'graphql';
        import { createServer } from 'http';
        import { SubscriptionServer } from 'subscriptions-transport-ws';

        import { schema } from './src/schema';

        const PORT = 3000;
        const server = express();

        server.use('*', cors({ origin: `http://localhost:${PORT}` }));

        server.use('/graphql', bodyParser.json(), graphqlExpress({
            schema
        }));

        server.use('/graphiql', graphiqlExpress({
            endpointURL: '/graphql',
            subscriptionsEndpoint: `ws://localhost:${PORT}/subscriptions`
        }));

        // Wrap the Express server
        const ws = createServer(server);
        ws.listen(PORT, () => {
        console.log(`Apollo Server is now running on http://localhost:${PORT}`);

        // Set up the WebSocket for handling GraphQL subscriptions
        new SubscriptionServer({
            execute,
            subscribe,
            schema
        },
        {
            server: ws,
            path: '/subscriptions',
        });
        });

    ```

## Filter Subscriptions

- Đôi khi **client** sẽ muốn lọc ra các **event** cụ thể  dựa vào **context** và **arguments**

    ```jsx
        subscription($repoName: String!){
            commentAdded(repoFullName: $repoName) {
                id
                content
            }
        }
    ```

- khi bạn sử dụng **`withFilter`** -> nó cúng cấp 1 **filter** **function**
- Nó thực thi với payload  (the published value), variables, context and operation info.
- Nó phải return boolean or Promise
- with withFilter that will filter out all of the commentAdded events that are not the requested repository:

    ```jsx
        import { withFilter } from 'graphql-subscriptions';

        const rootResolver = {
            Query: () => { ... },
            Mutation: () => { ... },
            Subscription: {
                commentAdded: {
                    subscribe: withFilter(() => pubsub.asyncIterator('commentAdded'), (payload, variables) => {
                        return payload.commentAdded.repository_name === variables.repoFullName;
                    }),
                }
            },
        };
    ```

## Lifecycle Events

- **`onConnect`** :
    - call kết nối **client**, với **connectionParams** được passed to **SubscriptionsClient**
    - bạn có thể trả về a **Promise** và **reject** kết nối bằng cách **throwing an exception**
    - resolved return value sẽ được thêm vào context GraphQl của subscriptions

- **`onDisconnect`** :
    - called when the client disconnects.

- **`onOperation`** :
    - called when the client executes a GraphQL operation
    - use this method to create custom params khi resolving
    - you can this method để ghi đè ( override ) GraphQL Schema

- **`onOperationComplete`** :
    - được gọi khi hoạt động client đã được thực thi

    ```jsx
        const subscriptionsServer = new SubscriptionServer(
            {
                onConnect: (connectionParams, webSocket, context) => {
                    // ...
                },

                onOperation: (message, params, webSocket) => {
                    // Manipulate and return the params, e.g.
                    params.context.randomId = uuid.v4();

                    // Or specify a schema override
                    if (shouldOverrideSchema()) {
                        params.schema = newSchema;
                    }

                    return params;
                },

                onOperationComplete: webSocket => {
                    // ...
                },

                onDisconnect: (webSocket, context) => {
                    // ...
                },
            },
            {
                server: websocketServer,
            },
            );
    ```

## Authentication Over WebSocket

```jsx
        import { execute, subscribe } from 'graphql';
        import { SubscriptionServer } from 'subscriptions-transport-ws';
        import { schema } from './schema';

        const validateToken = (authToken) => {
            // ... validate token and return a Promise, rejects in case of an error
        }

        const findUser = (authToken) => {
            return (tokenValidationResult) => {
                // ... finds user by auth token and return a Promise, rejects in case of an error
            }
        }

        const subscriptionsServer = new SubscriptionServer(
        {
            execute,
            subscribe,
            schema,
            onConnect: (connectionParams, webSocket) => {
            if (connectionParams.authToken) {
                    return validateToken(connectionParams.authToken)
                        .then(findUser(connectionParams.authToken))
                        .then((user) => {
                            return {
                                currentUser: user,
                            };
                        });
            }

            throw new Error('Missing auth token!');
            }
        },
        {
            server: websocketServer
        }
        );
```

## Express

```jsx
        import express from 'express';
        import bodyParser from 'body-parser';
        import { graphqlExpress } from 'apollo-server-express';
        import { createServer } from 'http';
        import { execute, subscribe } from 'graphql';
        import { PubSub } from 'graphql-subscriptions';
        import { SubscriptionServer } from 'subscriptions-transport-ws';
        import { myGraphQLSchema } from './my-schema';

        const PORT = 3000;
        const app = express();

        app.use('/graphql', bodyParser.json(), graphqlExpress({ schema: myGraphQLSchema }));

        const pubsub = new PubSub();
        const server = createServer(app);

        server.listen(PORT, () => {
            new SubscriptionServer({
                execute,
                subscribe,
                schema: myGraphQLSchema,
            }, {
                server: server,
                path: '/subscriptions',
            });
        });
```