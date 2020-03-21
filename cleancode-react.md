## Use destructring

When we spread props we run into the risk of adding unknown HTML attributes, which is a bad practice. Instead we can use prop destructuring with ...rest operator, so it will add only required props. For example,

```jsx

const ComponentA = () =>
  <ComponentB isDisplay={true} className={'componentStyle'} />

const ComponentB = ({ isDisplay, ...domProps }) =>
  <div {...domProps}>{'ComponentB'}</div>

```


approach of removing an array element in React state

```jsx
removeItem(index) {
  this.setState({
    data: this.state.data.filter((item, i) => i !== index)
  })
}
```

1. Use in render if state, props has more properties

```js
render() {
  const {name, age, selectedGender} = this.state;
  const {name, age, selectedGender} = this.props;
  ...
  <FormControl type="text" value={name} placeholder="Name"
  onChange={(e) => this.handleChange(e.target.value, 'name')}
  />
}
```

possible ways of updating objects in state
```jsx
const user = Object.assign({}, this.state.user, { age: 42 })
this.setState({ user })

//spread operator:
const user = { ...this.state.user, age: 42 }
this.setState({ user })

this.setState(prevState => ({
  user: {
    ...prevState.user,
    age: 42
  }
}))

```

How to update a component every second?
You need to use setInterval() to trigger the change, but you also need to clear the timer when the component unmounts to prevent errors and memory leaks.

```jsx
componentDidMount() {
  this.interval = setInterval(() => this.setState({ time: Date.now() }), 1000)
}

componentWillUnmount() {
  clearInterval(this.interval)
}
```


trigger click event in React

You could use the ref prop to acquire a reference to the underlying HTMLInputElement object through a callback, store the reference as a class property, then use that reference to later trigger a click from your event handlers using the HTMLElement.click method. This can be done in two steps:

Create ref in render method:
```jsx
<input ref={input => this.inputElement = input} />
Apply click event in your event handler:

this.inputElement.click()

```
benefit of styles modules


Render Props is a simple technique for sharing code between components using a prop whose value is a function. The below component uses render prop which returns a React element.

```jsx
<DataProvider render={data => (
  <h1>{`Hello ${data.target}`}</h1>
)}/>
```
Libraries such as React Router and DownShift are using this pattern.

## Use spread and rest operator

1. Speard

```js
constructor(props) {
  super(props);
  this.state = {
    socialMedia: [...SOCIAL_MEDIA_LIST] // An Array
  };
}
```

HOC loading

const withLoading = (Component) => (props) =>
  props.isLoading 
    ? <Loading /> 
    : <Component { ...props } />

## Computed Property

1. using in arg

```js
handleChange = (value, key) => {
  this.setState({
    [key]: value
  }
}
```

2. using from name attribute HTML e

## ESLINT RULES

- Chỉ chứa 1 component trong 1 file , ngoại trừ các element sử dụng lại
- Use classcomponent khi sử dụng state, ref, lifecycle. Use function component ( arrow funtion chỉ dùng cho cục bộ ) khi chỉ tạo các element
- Phần mở rộng jsx, tên file = tên component, or folder
  Dùng camelCase để đặt tên props và gán component, bỏ qua props có giá trị true
- Đặt tên Higher-order Component
- Dấu nháy kép cho thuộc tính jsx còn lại dùng nháy đơn
- Không dùng các từ "image", "photo", hoặc "picture" trong <img> alt props

```jsx
// tệ
<img src="hello.jpg" alt="Picture of me waving hello" />

// tốt
<img src="hello.jpg" alt="Me waving hello" />
```

- Chỉ sử dụng [ARIA roles](https://www.w3.org/TR/wai-aria/#role_definitions)
  Tránh dùng chỉ số của mảng(index) cho thuộc tính key

```jsx
// tệ
{
  todos.map((todo, index) => <Todo {...todo} key={index} />);
}

// tốt
{
  todos.map(todo => <Todo {...todo} key={todo.id} />);
}
```

- Luôn xác định rõ ràng các defaultProp(thuộc tính mặc định)

- Sử dụng toán tử spread đối với prop được khai báo rõ ràng.

````jsx
export default function Foo {
const props = {
  text: '',
  isPublished: false
}

return (<div {...props} />);
}```
````

- Nên lọc các props không cần thiết khi có thể. Ngoài ra, sử dụng prop-types-exact để giúp ngăn ngừa lỗ

```jsx
// tốt
render() {
  const { irrelevantProp, ...relevantProps  } = this.props;
  return <WrappedComponent {...relevantProps} />
}
```

- Các hàm binding được gọi trong lúc render nên đặt ở trong hàm khởi tạo(constructor)
- Không nên dùng dấu "\_" đặt trước tên các hàm của Component
- Không nên sử dụng isMounted


Passing parameter in arrow function react
```
twoParameter = (a, b) => e => {
    alert(a + b);
  };
 <button onClick={this.twoParameter(10, 54)}> two parameter</button>
```
