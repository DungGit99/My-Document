# ReactJS: Code-Splitting là gì?

- Code-Splitting là một trong những kỹ thuật giúp tăng tốc thời gian load Javascript của React App.
- Có nghĩa là chúng ta chia nhỏ code hiệu quả hơn và load những phần cần thiết cho trang web.
- Kỹ thuật này cũng được hiểu như là lazy load javascript.

## React.lazy
- React.lazy cho phép chúng ta load những component động như những cách thông thường.
    - Before:
    ```jsx
        import OtherComponent from './OtherComponent';

        function MyComponent() {
            return (
                <div>
                <OtherComponent />
                </div>
            );
        }
    ```
    - After:
    ```jsx
        const OtherComponent = React.lazy(() => import('./OtherComponent'));

        function MyComponent() {
            return (
                <div>
                <OtherComponent />
                </div>
            );
        }
    ```
- Lazy sẽ tự động load bundle chứa component OtherComponent khi mà MyComponent được render.
- React.lazy nhận vào một function sẽ gọi một dynamic import().

