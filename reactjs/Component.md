# Các loại component trong React

- **`Functional stateless component`**
- **`Component`**
- **`Pure component`**

## Functional stateless component

- Đây là **component** được khai báo dưới dạng 1 **function**, do đó nó là một **stateless** **component**, và **output** (được **rendered**) chỉ phụ thuộc vào **props** truyền vào cho **function**
- Đặc điểm:
  - Đây là cách đơn giản nhất để định nghĩa 1 **component** trong *React*
  - Nếu ta không cần quản lý bất kỳ **state** nào, thì đây là cách làm hợp lý
  - Bởi điểm khác biệt lớn nhất của việc sử dụng **function** thay vì **class** là bạn có thể hoàn toàn đảm bảo rằng **output** của nó chỉ phụ thuộc vào **input** được cấp cho hàm
  - *App* của bạn nên hướng tới mục tiêu có nhiều **component** **stateless** nhất có thể,
  - Bởi điều đó khiến bạn có thể quản lý *logic* của *app* dễ dàng hơn, cũng như dễ dàng *test* và tái sử dụng

- Hạn chế:
  - Không có các phương thức trong **life cycle**
  - Không có cách nào để kiểm soát việc **render**

- Ví dụ
    ```jsx
    // Thông thường
    function StatelessComponent(props) {
        return <div>{props.children}</div>
    }

    // es6
    const StatelessComponent = props => <div>{props.children}</div>

    // hoặc
    const StatelessComponent = ({ children }) => <div>{children}</div>
    ```

## Component
- Dạng thông thường, được khai bao dựa trên **class** của **es6**
- Có đầy đủ các **lifecycle**, **local state**
- Ví dụ:
    ```jsx
    class Message extends React.Component {
        constructor(props) {
            super(props)
            this.state = { isReaded: false }
            this.onRead = this.onRead.bind(this)
        }

        onRead() {
            this.setState({ isReaded: true })
        }

        componentDidMount() {
            this.props.sendNotification()
        }

        render() {
            return (
            <>
                <button onClick={this.onRead}>Show message</button>
                {isReaded && <div className="message">{this.props.message}</div>}
            </>
            )
        }
    }
    ```

## Pure component
- Giống **`Component`** ngoại trừ việc đó là nó xử lý **`shouldComponentUpdate`**
    - Đối với **`Component`** luôn return **true** trong hàm **`shouldComponentUpdate`** có nghĩa là mỗi lần **props** hay **state** gọi nó sẽ đi thẳng xuống **render**
    - Đối với **`PureComponent`** mỗi lần props và state nó nhảy vào **`shouldComponentUpdate`** so sánh giá trị trước và sau
        - Nếu nó === thì nó không **render** còn khác sẽ **render**
- Khi **props** và **state** thay đổi. **`PureComponent`** sẽ làm một **shallow comparison** trên cả **props** và **state**
- Muốn **`PureComponent`** hoạt động chính xác thì **props** và **state** phải là **immutability**
- Ví dụ:
    ```jsx
    // With Component
    class Message extends React.Component {
        shouldComponentUpdate(nextProps) {
            if (this.props.message === nextProps.message) return false
            return true
        }

        render() {
            return <li>{this.props.message}</li>
        }
    }

    // With PureComponent
    class Message extends React.PureComponent {
        render() {
            return <li>{this.props.message}</li>
        }
    }
    ```
