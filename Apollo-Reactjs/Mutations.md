# Mutations

- **index.js**
    ```jsx

        import { ApolloClient } from "apollo-client";
        import { ApolloProvider } from "@apollo/react-hooks";
        import { HttpLink } from "apollo-link-http";
        import { InMemoryCache } from "apollo-cache-inmemory";

        const client = new ApolloClient({
        link: new HttpLink({
            uri: "https://plp0mopxq.sse.codesandbox.io/"
        }),
        cache: new InMemoryCache()
        });


        ReactDOM.render(
        <ApolloProvider client={client}>
                    <App />
                </ApolloProvider>,
        document.getElementById('root')
        );

    ```
- **mutation.js**
    ```jsx
        import gql from 'graphql-tag';

        export const ADD_TODO = gql`
            mutation AddTodo($type: String!) {
                addTodo(type: $type) {
                    id
                    type
                }
            }
        `;

        export const UPDATE_TODO = gql`
            mutation UpdateTodo($id : String!, $type: String!){
                updateTodo(id: $id, type: $type){
                    id
                    type
                }
            }
        `
    ```
- **queries.js**
    ```jsx
        import gql from "graphql-tag";

        export const GET_TODOS = gql`
            {
                todos {
                    id
                    type
                }
            }
        `;
    ```
- **AddToDos.js**
    ```jsx
        import React from 'react';
        import { useMutation } from '@apollo/react-hooks'
        import { ADD_TODO } from '../graphql';
        function AddTodo() {

            const [ value, setValue ] = useState('');
            const [ addTodo,{data} ] = useMutation(ADD_TODO);

            const handleChange = (e) =>{
                setValue(e.target.value)
            }
            const handleSubmit = (e) =>{
                e.preventDefault();
                addTodo({ variables: { type: value }});
                setValue('');
            }

            return (
                <div>
                    <form onSubmit={handleSubmit}>
                        <input type='text' value={value} onChange={handleChange}/>
                        <button type="submit">Add Todo</button>
                    </form>
                </div>
            );
        }

        export default AddTodo;
    ```
- **Todo.js**
    ```jsx
        import React from 'react';
        import { useQuery } from '@apollo/react-hooks';
        import { GET_TODOS } from '../graphql';
        import UpdateTodo from './UpdateTodo';
        function Todos() {

            const { loading, error, data } = useQuery(GET_TODOS);


            if (loading) return <p>Loading...</p>;
            if (error) return <p>Error</p>;

            const dataTodo = data && data.todos;

            return (
                <div>
                    {dataTodo.map(({ id, type })=>(
                        <div key={id}>

                            <p>{type}</p>
                            <UpdateTodo id={id}/>
                        </div>
                    ))}
                </div>
            );
        }

        export default Todos;
    ```
- **UpdateTodo.js**
    ```jsx
        import React, { useState } from 'react';
        import { useMutation } from '@apollo/react-hooks'
        import { UPDATE_TODO } from '../graphql';
        function UpdateTodo({id}) {

            const [ value, setValue ] = useState('');
            const [ updateTodo,{ loading, error } ] =useMutation(UPDATE_TODO);

            const handleChange = (e) =>{
                setValue(e.target.value)
            }
            const handleSubmit = (e) =>{
                e.preventDefault();
                updateTodo({ variables: {id, type: value }});
                setValue('');
            }

            return (
                <div>
                    {loading && <p>Loading...</p>}
                    {error && <p>Error :( Please try again</p>}
                    <form onSubmit={handleSubmit}>
                        <input type='text' value={value} onChange={handleChange}/>
                        <button type="submit">Update Todo</button>
                    </form>
                </div>
            );
        }

        export default UpdateTodo;
    ```
