# Higher-order functions

- khi ta định nghĩa một **Function**, ta thường nhận **param** ( tham số đầu vào ) là các **String**, **Number**, **Boolean**, **Array** hoặc **Object** rồi lại **return** ( trả về ) các **String**, **Number**, **Boolean**, **Array** hoặc **Object**.
- Đối với **Higher Order Function** (HOF), **param** có thể là **Function**, và lại **return** ra có thể cũng là **Function**.
    ```jsx
        Higher-order functions là các hàm lấy các hàm khác làm đối số hoặc trả về các hàm làm kết quả của chúng.
    ```
    ![image](https://images.viblo.asia/full/b88c3e38-0af3-4d94-8540-ff2fe3248e79.jpg)
## Gán Function đến các variable
- Chúng ta có thể gán **Function** đến các **variable**.
    ```javascript
        // gán constantA bằng functionA
        const constantA = function functionA(x){
        console.log('kết quả',x * x);
        }

        //gọi constantA và xem kết quả
        constantA(5);
    ```
- Chúng ta cũng có thể truyền chúng đến **variable** khác.
    ```javascript
        // gán constantB bằng constantA
        const constantB = constantA;

        // gọi constantB và xem kết quả
        constantB(6)
    ```
## Truyền các Function như params
- Ví dụ :
    ```javascript
        function inHoa(text) {
            return text.toUpperCase() + '!' // in hoa param truyền vào và return kết quả đó
        }
        function inThuong(text) {
            return text.toLowerCase() + '...'
        }

        function chao(func){
            // thực hiện việc gán const greeting bằng return của function là param truyền vào
            const greeting = func('I am javascript')
            return greeting;
        }
        console.log(chao(inHoa)) // >> "I AM JAVASCRIPT!"
        console.log(chao(inThuong))
    ```
    ```javascript
        Nó cho phép chúng ta thay đổi hành vi và kết quả của một Function mà không hề thay đổi lại code trong Function đó.
    ```

- Ví dụ :
    ```javascript
        const multiplication = function(a,b){
            return a * b ;
        }
        function calculate(a,b,operation){
            return operation(a,b)
        }
        console.log(calculate(2,3,multiplication)) // >>> 6

    ```
## Functions có thể chứa bên trong hàm khác
- **Function** trong **JavaScript** có thể được **define** ( định nghĩa ) trong 1 **Function** khác, Chúng còn có thể được gọi như là **nested** **functions** hay **inner** **functions**.
- Ví dụ :
    ```javascript
        function inText(text){
        // function inHoaText được lồng trong function inText
        // function inHoaText làm nhiệm vụ in hoa ký tự được truyền từ param vào
            function inHoaText(textInHoa){
                return textInHoa.toUpperCase();
            }

            // trả về function inHoaText có param là text
            return inHoaText(text);
        }

        // gọi function inText và in kết quả từ function inText
        console.log(inText('Nguyen Van Dũng'));
    ```
- Ví dụ :
    ```javascript
        // khởi tạo function totalAB có param là số A
        function totalAB(A){
            // function numberB được lồng trong function totalAB
            // function numberB này có param là số B và thực hiện việc cộng 2 số là A và B
            function numberB(B){
                return A + B;
            }
            // return function numberB
            return numberB;
        }

        // Dùng cách gán Function cho 1 Variable khác.
        const getTotal = totalAB(5);
        console.log(getTotal(7))

        // Gọi trực tiếp function
        console.log(totalAB(4)(5));
    ```