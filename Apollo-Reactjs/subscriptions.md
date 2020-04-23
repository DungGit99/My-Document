# Subscriptions

## Install

- npm install --save apollo-link-ws subscriptions-transport-ws

## Client setup

- **index.js**

    ```jsx
        import { WebSocketLink } from 'apollo-link-ws';
        import { split } from 'apollo-link';
        import { HttpLink } from 'apollo-link-http';
        import { getMainDefinition } from 'apollo-utilities';

        // Create an http link:
        const httpLink = new HttpLink({
            uri: 'http://localhost:3000/graphql'
        });


        // Create a WebSocket link:
        const wsLink = new WebSocketLink({
            uri: `ws://localhost:5000/`,
            options: {
                reconnect: true,
                connectionParams:{
                    authToken: user.authToken,
                }
            }
        });


        const link = split(
            ({query})=>{
                const definition = getMainDefinition(query);
                return ( definition.kind === 'OperationDefinition' &&  definition.operation === 'subscription' )
            },
            wsLink,
            httpLink
        )
    ```

- **subscriptions.js**
    ```jsx
        import  gql  from  '@apollo/react-hooks';

        export const COMMENTS_SUBSCRIPTION =gql`
            subscription onCommentAdded($repoFullName: String!){
                commentAdded(repoFullName: $repoFullName){
                    id
                    content
                }
            }
        `

        export const COMMENT_QUERY = gql`
            query Comment($repoName: String!) {
                entry(repoFullName: $repoName) {
                    comments {
                        id
                        content
                    }
                }
            }
        `;
    ```

- **DontReadTheComments.js**
    ```jsx
        import React from 'react';
        import { useSubscription } from '@apollo/react-hooks';
        import { COMMENTS_SUBSCRIPTION } from '../graphql/QMStrings';

        function DontReadTheComments({ repoFullName }) {

            const { data:{ commentAdded }, loading } = useSubscription(COMMENTS_SUBSCRIPTION,{
                variables : { repoFullName }
            }) ;

            return (
                <div>
                    <h1>New comment : {!loading && commentAdded.content}</h1>
                </div>
            );
        }

        export default DontReadTheComments;
    ```

- **CommentsPageWithData.js**
    ```jsx
        import React from 'react';
        import { useQuery } from '@apollo/react-hooks';
        import { COMMENT_QUERY } from '../graphql/QMStrings';
        import CommentsPage from './CommentsPage';

        function CommentsPageWithData({ params }) {

            const { subscribeToMore, ...result } = useQuery( COMMENT_QUERY,
                { variables: { repoName: `${params.org}/${params.repoName}` } }
            );

            const handleSubsribe = () => (

                subscribeToMore({

                    document: COMMENTS_SUBSCRIPTION,
                    variables: { repoName: params.repoName },
                    updateQuery: ( prev, { subscriptionData }) => {
                        if (!subscriptionData.data) return prev;

                        const newFeedItem = subscriptionData.data.commentAdded;

                        return Object.assign({},prev,{
                            entry: {
                                comments: [newFeedItem, ...prev.entry.comments]
                            }
                        });
                    }
                })
            )

            return <CommentsPage {...result} subscribeToNewComments={handleSubsribe}/>;
        }

        export default CommentsPageWithData;
    ```

- **CommentsPage.js**
    ```jsx
        import React, { Component } from 'react';

        class CommentsPage extends Component {

            componentDidMount(){
                this.props.subscribeToNewComments();
            }
        }

        export default CommentsPage;
    ```