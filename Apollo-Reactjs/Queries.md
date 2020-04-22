# Set up Apollo Client in your React app

## Installation

- npm install apollo-boost @apollo/react-hooks graphql
    - **apollo-boost**: Package containing everything you need to set up Apollo Client
    - **@apollo/react-hooks**: React hooks based view layer integration
    -  **graphql**: Also parses your GraphQL queries

## Apollo Boost
- **apollo-client**: Where all the magic happens
- **apollo-cache-inmemory**: Our recommended cache
- **apollo-link-http**: An Apollo Link for remote data fetching
- **apollo-link-error**: An Apollo Link for error handling

## Queries ( useQuery )

- **useQuery** trả về 1 **object** từ **from** **Apollo** **Client** chứa các **properties** :
    - `loading`
    - `error`
    - `data`

    ```jsx
        const { loading, error, data } = useQuery(GetExchangeRates)
    ```
- **Updating cached query results**
    - Đôi khi bạn muốn đảm bảo rằng dữ liệu được lưu trong bộ nhớ cache được cập nhật tới **server**
    - **Apollo Client** hổ trợ 2 strategies : **polling** and **refetching**

    - **Polling** :
        - Nó cung cấp đồng bộ gần như thời gian thực với máy chủ của bạn bằng cách khiến truy vấn thực hiện định kỳ tại 1 khoảng thời gian xác định
        - **`pollInterval`** : `pollInterval : 500`
            - Điều này có nghĩa là bạn sẽ lấy **data** từ **server** sau *0.5s*
        - Bạn cũng có thể start and stop polling dynamically với:
            - **`startPolling`**
            - **`stopPolling`**

    - **Refetching**
        - Cho phép bạn **refresh** kết quả query để **response** với một hành động cụ thể của người dùng

- **Inspecting loading states**
    - **`networkStatus`** : khi bạn muốn thông báo cho **user** rằng **data** đang **loading**
    - set **notifyOnNetworkStatusChange**: **true**
    - [https://github.com/apollographql/apollo-client/blob/master/src/core/networkStatus.ts]

- **Inspecting error states**
    - **`errorPolicy`** : **none** (default value)

- **`Executing queries manually`**
    - Nếu bạn muốn thực hiện 1 query để **response** lại 1 **event** khác, như người dùng **click** vào **button**
    - **`useLazyQuery`** thực hiện truy vấn khi các **component** đang **redering**
    - khi **`useLazyQuery`** được call thì nó không ngay lập tức thực hiện truy vấn
    - Thay vào đó nó trả về  1 **function**

- **Result**
    - **`useQuery`** trả về  object với cái thuộc tính sau đây :
        - **data** : 1 **object** chứa kết quả của **GraphQL query** (default to **undefined**)
        - ....
        - [https://www.apollographql.com/docs/react/data/queries/]

    - **index.js**
        ```jsx
            import React from 'react';
            import ReactDOM from 'react-dom';
            import App from './App';

            import ApolloClient from "apollo-boost";
            import { ApolloProvider } from "@apollo/react-hooks";

            const client = new ApolloClient({
                uri : "https://32ypr38l61.sse.codesandbox.io" // dog
            });

            ReactDOM.render(
                <ApolloProvider client={client}>
                    <App />
                </ApolloProvider>
            ,
            document.getElementById('root')
            );
        ```

    - **GraphQl**
        ```jsx
            export const GET_DOGS = gql`
                {
                    dogs{
                        id
                        breed
                    }
                }
            `
            export const GET_DOG_PHOTO = gql`
                query dog($breed: String!) {
                    dog(breed: $breed) {
                    id
                    displayImage
                    }
                }
            `;
        ```
    - **Dog.js**
        ```jsx
            import React from 'react';
            import { useQuery } from '@apollo/react-hooks';
            import { GET_DOGS } from '../graphql';

            const Dogs = ({onDogSelected}) => {

                const { loading, error, data} = useQuery(GET_DOGS);

                if(loading) return 'Loading...';
                if(error) return `Error!${error.message}`;

                return (
                    <select name="dog" onChange={onDogSelected}>
                        {data.dogs.map(dog => (
                            <option key={dog.id} value={dog.breed}>
                                {dog.breed}
                            </option>
                        ))}
                    </select>
                );
            }

            export default Dogs;
        ```
    - **DogPhoto.js**
    ```jsx
        import React from 'react';
        import {useQuery} from '@apollo/react-hooks';
        import { GET_DOG_PHOTO } from '../graphql/queries';
        function DogPhoto({breed}) {

            const { loading, error, data, refetch, networkStatus } = useQuery(
                GET_DOG_PHOTO,
                {
                variables: { breed },
                notifyOnNetworkStatusChange: true
                // pollInterval: 500
                }
            );

            if(networkStatus === 4) return 'Refetching !!!'
            if(loading) return 'Loading...!!!';
            if(error) return `Error ${error.message}`;

            return (
                <div>
                    <img alt="#" src={data.dog.displayImage} style={{ height: 100, width: 100 }} />
                    <button onClick={()=>refetch()} >Refetch!!!</button>
                </div>
            );
        }

        export default DogPhoto;
    ```

    - **App.js**
        ```jsx
            import React, { useState } from 'react';
            import Dogs from './components/Dogs'
            import DogPhoto from './components/DogPhoto';
            function App() {

                const [ selectDog,setSelectDog] = useState(null)

                const onDogSelected = (e) =>{
                    setSelectDog(e.target.value);
                }

                return (
                    <div>
                        <h2>Building Query components 🚀</h2>
                        {selectDog && <DogPhoto breed={selectDog}/>}
                        <Dogs onDogSelected={onDogSelected}/>
                    </div>
                );
            }

            export default App;
        ```