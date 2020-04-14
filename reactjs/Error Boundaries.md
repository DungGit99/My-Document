# Error Handling in React 16

## Error Boundaries ?
- Là một **React components** để bắt tất cả các lỗi **Javascript** ở bất cứ vị trí nào trong cây **component** con.
- **Log** lỗi ra **console** cũng như là hiển thị lên **UI** thay vì khiến cho toàn bộ cây **component** bị **crash**.
    ```jsx
    class ErrorBoundary extends React.Component {
        constructor(props) {
            super(props);
            this.state = { hasError: false };
        }
        static getDerivedStateFromError(error) {
        //  Update state để render tiếp theo sẽ hiển thị fallback UI.
            return { hasError: true };
        }

        componentDidCatch(error, info) {
            // Display fallback UI
            this.setState({ hasError: true });
            // Bạn cũng có thể log lỗi vào một service báo cáo lỗi
            logErrorToMyService(error, info);
        }

        render() {
            if (this.state.hasError) {
            // Bạn có thể render bất kỳ custom fallback UI nào
            return <h1>Something went wrong.</h1>;
            }
            return this.props.children;
        }
    }
    ```
    - `componentDidCatch(error, info)` có cơ chế hoạt động giống khối **catch** trong **javascript** thuần nhưng chỉ áp dụng cho **component**.

- **Error boundaries** chỉ áp dụng với **class component**.
- **Error boundaries** chỉ có tác dụng với các **component** con trong cây **component** của nó mà không thể bắt lỗi với chính **component** đang khai báo **componentDidCatch** .

## Tại sao React không sử dụng try/catch
- **try** / **catch** sử dụng rất tốt nhưng nó lại chỉ làm việc với mã **imperative** :
    ```javascript
        try {
            showButton();
        } catch (error) {
            // ...
        }
    ```
    - Tuy nhiên, **React components** lại sử dụng việc khai báo các thẻ cần được **render**. Ví dụ : `<Button />`
- Với **error boundaries** thì khi xảy ra lỗi trong hàm `componentDidMount()` gây ra bởi việc **setState** ở một chỗ nào đó trong cây **component** thì nó vẫn báo đúng vị trí phát sinh lỗi.

## Project MD Conference
    ```jsx
        import React from 'react'

        export class ErrorBoundary extends React.Component {
            state = { hasError: false }

            static getDerivedStateFromError() {
                // Update state so the next render will show the fallback UI.
                return { hasError: true }
            }

            componentDidCatch(error, errorInfo) {
                // You can also log the error to an error reporting service
                if (process.env.NODE_ENV === 'development') {
                    console.log(error, errorInfo)
                }
            }

            render() {
                return this.state.hasError ? this.props.fallback : this.props.children
            }
        }
    ```

    ```jsx
        class RouterComponent extends React.PureComponent {
        render() {
            return (
            <Router basename='/cfr'>
                <React.Suspense fallback={<BounceLoader />}>
                <ErrorBoundary fallback={<NotFound />}>
                    <Switch>
                        <Route
                        key={index.toString()}
                        {...routeProps}
                        render={() => {
                            const Component = React.lazy(() => import(`../pages/${component}`))
                            return <Component />
                        }}
                        />
                    ))}
                    <Redirect to={routesPath.notfound} />
                    </Switch>
                </ErrorBoundary>
                </React.Suspense>
            </Router>
            )
        }
        }

        export default RouterComponent

    ```