# Fragment trong JSX

## JSX là gì ?
- JSX là một phần mở rộng cú pháp cho JavaScript.
- JSX cho ra React "elements".

## Fragments ?
- Là 1 **parttern** chung trong **React** dùng cho **component** để trả về nhiều **elements** mà không cần tạo thêm các **nodes** vào **DOM**.
- **React develops** tạo ra **Fragments** chính xác là việc trả về **list** các thành phần con mà không bao gồm bất kỳ **elements** cha nào.
    ```jsx
        class Columns extends React.Component {
            render() {
                return (
                    <React.Fragment>
                        <td>Hello</td>
                        <td>World</td>
                    </React.Fragment>
                );
            }
        }
    ```
- Cú pháp ngắn cho **Fragments** là <>
    ```jsx
        class Columns extends React.Component {
            render() {
                return (
                    <>
                        <td>Hello</td>
                        <td>World</td>
                    </>
                );
            }
        }
    ```
- **Fragments** có thể có **key** **attribute**
    - Một trường hợp sử dụng cho việc này là **mapping** một **collection** tới một mảng các **fragments**
    ```jsx
        function Glossary(props) {
            return (
                <dl>
                    {props.items.map(item => (
                    // Nếu không có `key`, React sẽ báo warning về key
                    <React.Fragment key={item.id}>
                        <dt>{item.term}</dt>
                        <dd>{item.description}</dd>
                    </React.Fragment>
                    ))}
                </dl>
            );
        }
    ```
- Nó có thể giúp bạn tránh được nhiều vấn đề về **CSS** liên quan đến **CSS** **selectors**.