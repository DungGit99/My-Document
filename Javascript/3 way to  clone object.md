# 3 cách để Clone Objects trong JavaScript

## Objects là một loại tham chiếu

- [https://viblo.asia/p/3-cach-de-clone-objects-trong-javascript-Az45baqNlxY]
- [https://viblo.asia/p/clone-mot-object-trong-javascript-nhu-the-nao-la-chuan-naQZRpg05vx]
- **Object** là 1 loại tham chiếu
- Vì vậy khi sử dụng dấu = , nó đã sao chép con trỏ vào không gian bộ nhớ mà nó chiếm
- Các kiểu tham chiếu không giữ các giá trị, chúng là 1 con trỏ tới giái trị trong bộ nhớ

    ```jsx
        const obj2.three = 3;

        console.log(obj2);
        // {one: 1, two: 2, three: 3}; <-- ✅

        console.log(obj);
        // {one: 1, two: 2, three: 3}; <-- 😱
    ```

## 1 Sử dụng Spread

```jsx
    const food = {
        corn: '🌽',
        bacon: '🥓'
    };

    const cloneFood = { ...food };

    console.log(cloneFood); // { corn: '🌽', bacon: '🥓' }
```
- Sử dụng **Spread** sẽ giúp ta **clone** **Obj**. Lưu ý khi sử dụng nó bạn có thể cần phải **compile** cùng với **Babel**

- **Spread** là một cách sao chép các thuộc tính của một đối tượng thành một đối tượng mới
    ```jsx
        const food = { corn: '🌽', bacon: '🥓' };

        food = {
            ...food,
            corn: '🌽🌽',
        }
        // TypeError: invalid assignment to const `food'
    ```

    ```jsx
        const food = {
            corn: '🌽',
            bacon: '🥓'
        };

        const newFood = {
            ...food,
            corn: '🌽🌽',
        }

        console.log(newFood); // { corn: '🌽🌽', bacon: '🥓' }
    ```


## 2. Sử dụng Object.assign

```jsx
    const food = {
        corn: '🌽',
        bacon: '🥓'
    };

    const cloneFood = Object.assign({}, food);

    console.log(cloneFood);
    // { corn: '🌽', bacon: '🥓' }
```
- **`Object.assign`** là một hàm sửa đổi và trả về đối tượng đích.
- **{}** được hiểu là *`đối tượng được sửa đổi`*
- đối tượng đích không được tham chiếu bởi bất kỳ biến nào tại thời điểm đó
- Nhưng nó trả về  *`đối tượng đích`*

    ```jsx
        const food = {
            corn: '🌽',
            bacon: '🥓'
        };

        Object.assign(food, { corn: '🌽🌽' });

        console.log(food); // { corn: '🌽🌽', bacon: '🥓' }
    ```


## 3. Sử dụng JSON

```jsx
    const food = {
        corn: '🌽',
        bacon: '🥓'
    };

    const cloneFood = JSON.parse(JSON.stringify(food))

    console.log(cloneFood); // { corn: '🌽', bacon: '🥓' }

```

## Shallow Clone vs Deep Clone

```jsx
    const nestedObject = {
        country: '🇨🇦',
        detail: {
            city: 'vancouver'
        }
    };

    const shallowClone = { ...nestedObject };

    // Changed our cloned object
    shallowClone.country = '🇹🇼'
    shallowClone.detail.city = 'taipei';

    console.log(shallowClone); // {country: '🇹🇼', detail: {city: 'taipei'}} <-- ✅

    console.log(nestedObject);// {country: '🇨🇦', detail: {city: 'taipei'}} <-- 😱

```
- **Clone** "nông" có nghĩa ta chỉ **clone** được **level** đầu, các **level** sâu hơn sẽ được hiểu là tham chiếu.
- Như vậy, để thấy rằng **spread** chỉ có tác dụng với **object** 1 cấp
- và khi thực hiện **clone** **object** thì nó chỉ bảo đảm tính **immutable** cho dữ liệu ở cấp đầu tiên mà thôi, đó gọi là **shallow** **copy**.

- Sử dụng cách 3 :
    ```jsx
    const deepClone = JSON.parse(JSON.stringify(nestedObject));

    console.log(deepClone); // {country: '🇹🇼', detail: {city: 'taipei'}} <-- ✅

    console.log(nestedObject); // {country: '🇨🇦', detail: {city: 'vancouver'}} <-- ✅
    ```