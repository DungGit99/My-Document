# ReactJS: Code-Splitting là gì?

- **Code-Splitting** là một trong những kỹ thuật giúp tăng tốc thời gian load Javascript của **React App**.
- Có nghĩa là chúng ta chia nhỏ code hiệu quả hơn và load những phần cần thiết cho trang web.
- Kỹ thuật này cũng được hiểu như là lazy load javascript.

## React.lazy
- **React.lazy** cho phép chúng ta load những component động như những cách thông thường.
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
- **Lazy** sẽ tự động load bundle chứa component `OtherComponent` khi mà `MyComponent` được render.
- **React.lazy** nhận vào một function sẽ gọi một dynamic **import()**.

## Suspense
- Trong một số trường hợp, khi load dynamic component với lazy, chúng ta cần chờ đợi một khoảng thời gian.
- Khi ấy, `OtherComponent` chưa hiển thị và cần một **loading** để làm cho user có cảm giác đang tương tác với trang web.
- Trong những tình huống này chúng ta cần đến **Suspense** để truyền một loading component.
    ```jsx
        const OtherComponent = React.lazy(() => import('./OtherComponent'));

        function MyComponent() {
            return (
                <div>
                    <Suspense fallback={<div>Loading...</div>}>
                        <OtherComponent />
                    </Suspense>
                </div>
            );
        }
    ```
## Code-splitting theo route
- Giả sử chúng ta có một số trang như là: **Home**, **About**, **Contact**, …
- Thì mỗi trang như vậy là một **route** và tương ứng với **url** của từng **route** chúng ta chia nhỏ **bundle** **file** thành nhiều **file**.
    ```jsx
        import { BrowserRouter as Router, Route, Switch } from 'react-router-dom';
        import React, { Suspense, lazy } from 'react';

        const Home = lazy(() => import('./routes/Home'));
        const About = lazy(() => import('./routes/About'));
        const Contact = lazy(() => import('./routes/Contact'));

        const App = () => (
            <Router>
                <Suspense fallback={<div>Loading...</div>}>
                <Switch>
                    <Route exact path="/" component={Home}/>
                    <Route path="/about" component={About}/>
                    <Route path="/contact" component={Contact}/>
                </Switch>
                </Suspense>
            </Router>
        );
    ```
