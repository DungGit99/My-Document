# GraphQL là gì?

- **GraphQL**
    - là một ngôn ngữ truy vấn dành cho API
    - Là một cú pháp mô tả cách yêu cầu lấy dữ liệu, và thường được dùng để **load** **data** từ một **server** cho **client**.
- **GraphQL** bao gồm 3 điểm đặc trưng bao gồm :
    - Cho phép **client** xác định chính xác những gì dữ liệu họ cần
    - Làm cho việc tổng hợp dữ liệu từ nhiều nguồn dễ dàng hơn
    - Nó sử dụng một **type system** để mô tả dữ liệu.

    ![image-graphQl](https://topdev.vn/blog/wp-content/uploads/2017/04/graphql_diagram.jpg)

- **GraphQL API** được tạo ra từ 3 phần chính: **schema**, **queries**, và **resolvers**

## Schema
- Query
- Muatation
- Subscription
    ```javascript
        type Query {
            allPersons(last: Int): [Person!]!
        }

        type Mutation {
        createPerson(name: String!, age: Int!): Person!
        }

        type Subscription {
        newPerson: Person!
        }

        type Person {
        name: String!
        age: Int!
        posts: [Post!]!
        }

        type Post {
        title: String!
        author: Person!
        }
    ```
