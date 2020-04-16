# Quản lý component state với MobX

- Nó đống gói **state** của bạn thành những **observable**.
- Vì thế bạn có mọi khả năng của một **Observable** trong **state** của bạn.
- Dữ liệu có thể chỉ cần có **setter** và **getter** thôi, nhưng **observable** làm cho chúng ta có khả năng nhận **update** khi dữ liệu thay đổi.
- Trong **MobX**, **state** là **mutable**. Nghĩa là bạn thay đổi **state** trực tiếp:
    ```javascript
        function addAuthor(author) {
            this.authors.push(author);
        }
    ```

## Store
- MobX lại dùng nhiều store

## Implementation nhìn như thế nào?
- Một **store** sẽ chỉ quản lý một **substate** (như một **reducer** trong **Redux** quản lý một **substate**)
- Có thể  thay đổi **state** 1 cách trực tiếp
-  **Annotation @observable** giúp cho chúng ta có khả năng nhận **update** khi **state** thay đổi.
    ```jsx
        class UserStore {
            @observable users = [
                {
                    name: 'Dan'
                },
                {
                    name: 'Michel'
                }
            ];
            @action addUser = (user) => {
                this.users.push(user);
            }
        }
    ```
    - Giờ thì chúng ta có thể gọi `userStore.users.push(user)`;
    - Tuy vậy **best practice** đó là xử lý việc thay đổi **state** rõ ràng hơn với các **action**.
