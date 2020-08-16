## REACT

> React là một thư viện UI hỗ trợ việc xây dựng những (components) UI có tính tương tác cao, có trạng thái và có thể sử dụng lại được, đặc biệt cho SPA

**Tính năng**

- React JS sử dụng Virtual DOM (DOM ảo) để cải thiện hiệu suất
- One Way Data Binding(Follow) - Unidirectional Data Flow (giúp kiểm soát tốt hơn trong toàn bộ ứng dụng)
- Hỗ trợ server-side rederingredering (lib 'react-dom/server')
- Declarative views make your code more readable and easier to debug.
- RUses reusable/composable UI components -> dễ truyền dữ liệu qua ứng dụng và giữ trạng thái ra khỏi DOM.

Nhuoc diem

- React is just a view library, not a full framework.
- There is a learning curve for beginners who are new to web development.
- Integrating React into a traditional MVC framework requires some additional configuration.
- The code complexity increases with inline templating and JSX.
- Too many smaller components leading to over engineering or boilerplate.

[Readmore](https://acowebs.com/react-js-features/)

### Vituarl DOM (V)

> The Virtual DOM (VDOM) is an in-memory 'objects representation' of a UI and synced with the "real" DOM. It's a step that happens between the render function being called and the displaying of elements on the screen. This entire process is called reconciliation (hay có thể hiểu là quá trình sử dụng thuât toán diffing để tìm ra những gì nên cập nhật trên DOM thực).

VDOM thường được hiểu là object representation of UI , tuy nhiên nó cũng có 1 internal object call fibers lưu trữ thông tin của component tree

**How it work : **Khi render() lần đầu tiên được gọi nó sẽ tạo ra 1 vDOM ,trong những lần tiếp theo render() được gọi nó sẽ trả về 1 vDOM khác và tiến hành so sánh 2 phiên bản (thuật toán diffing) để tìm ra chính xác những gì nó cập nhật trên DOM thực .

Chuẩn theo tài liệu của React .
https://reactjs.org/docs/reconciliation.html
https://reactjs.org/docs/faq-internals.html

----- LƯU Ý :

1. DOM không chậm tuy nhiên việc update DOM là đồng bộ mỗi khi DOM thay đổi browser sẽ thực hiện lại tính toán css, layout (reflow) và repaint , những công việc này cần nhiều thời gian hơn khi DOM càng lớn, thay vào đó React sẽ thực hiện các update ở vDOM sau đó đông bộ với DOM 1 lần để cải thiện hiệu suất 

2. vDOM chỉ chứa những gì đủ để construct DOM (lightweigth representation) chứ không chứa tất cả DOM API (heavyweight parts of real DOM) -> Chi phí cập nhật vDOM rẻ hơn DOM . Việc update vDOM trên memory cũng sẽ nhanh hơn update trên browser

3. Hiện nay các browser cũng đã phát triển nên việc tối ưu hóa quá trình update DOM cùng lúc, tuy nhiên không phải trình duyệt nào cũng giống nhau và có những sự kiện làm phá vỡ điều này và khiến DOM vẫn update đồng bộ. 

   Đọc các bài viết này để hiểu hơn về DOM , quá trình render, reflow, repaint và tối ưu (js, css)
  -> https://medium.com/jspoint/how-the-browser-renders-a-web-page-dom-cssom-and-rendering-df10531c9969 (đọc cả link trong bài viết)
  -> https://developers.google.com/web/fundamentals/performance/critical-rendering-path/render-tree-construction
  -> https://stackoverflow.com/questions/2549296/whats-the-difference-between-reflow-and-repaint (reflow, repaint).
  
  Nếu browser phát triển đủ mạnh để có thể không cần vDOM thì chúng ta vẫn có thể sẽ cần React, React không chỉ dừng lại ở mỗi vDOM mà còn là về vấn đề maintain, scale hay có thể tham khảo các Framework khác thay vì vanilla js

Example về update vDOM vs DOM
-> https://stackoverflow.com/a/52555662

-----
reconciliation :
1. Elements or Component Of Different Types: Nếu chuyển từ 1 root(parent) element or component -? children E or C sẽ luôn được thay thế bằng E or C mới dù cho bạn có sử dụng memo(), react sẽ không thực hiện thuật diffing tại đây. -> https://codesandbox.io/s/learn-react-ww0h9?file=/src/DiffAth/index.js
2. DOM Elements Of The Same Type: React chỉ update changed attributes
3. Component Elements Of The Same Type: Thuật toán diffing sẽ được thực thi
4. Recursing On Children: Khi thực hiện đệ quy children of a DOM node , react sẽ thay thế toàn bộ children nếu thấy sự khác biệt. (chèn ở cuối sẽ hiệu quả hơn chèn ở đầu vì có sự khác biệt ngay tại phần tử đầu tiên, và react khuyên dùng KEY react sẽ biết được phần tử được chèn vào thông qua key thay vì thay thế toàn bộ)

React fiber (React 16) là một reconciliation engine mới giúp enable incremental rendering of the virtual DOM ( khả năng chia công việc kết xuất thành nhiều phần và trải rộng trên nhiều khung hình) tận dụng lợi thế của việc lập trinh, trình tự ưu tiên của công việc , chẳng hạn cập nhật animation có mức ưu tiên cao hơn với cập nhật list các phần tử.

Đọc thêm :
-> https://stackoverflow.com/a/46357759
-> https://www.youtube.com/watch?v=d7pyEDqBDeE

### Component 
- React components are immutable, the data within them cannot be changed
- component lại có thể chứa thành phần khác
- Sử dụng Class Component nếu muốn truy cập vào các lifecycle methods các trường hợp còn lại sử dụng Function Component
- [Pure Component](https://reactjs.org/docs/react-api.html#reactpurecomponent) sẽ shallow comparison (so sánh nông) cả props và state trước đó và hiện tại để quyết định shouldComponentUpdate có được call hay không => cải thiện hiệu suất

```jsx
// Function Component
function FComponent({ message }) {
  return <h1>{`Hello, ${message}`}</h1>
}

const FComponent = ({message}) => {
  return <h1>{`Hello, ${message}`}</h1>
}

// Class Component
class CComponent extends React.Component {
  render() {
    return <h1>{`Hello, ${this.props.message}`}</h1>
  }
```


### Element 

Một element là một plain object miêu tả một component instances hoặc DOM node và những properties mong muốn của nó
 Nó chỉ chứa những thông tin về component type (ví dụ như 1 Button), những thuộc tính của nó (ví dụ như color), and những component con ở trong nó

 ```jsx
 const element = <h1>Hello, world</h1>;
 ```
 
 [Component vs Element](https://viblo.asia/p/tim-hieu-react-component-elements-va-instances-GrLZDwaBKk0)

### Data binding 
> Liên kết model với view để khi data ở model thay đổi thì view cũng thay đổi (one way data binding - read only dễ kiểm soát, dễ hiểu ) hoặc có cả chiều ngược lại (twotwo way data binding - có thể gây ra tác dụng phụ khó theo dõi và khó hiểu hơn.)

One way data binding does not permit developers to edit any properties of the React JS component directly.

Every time the state changes, the view needs to be updated

https://www.wintellect.com/data-binding-pure-javascript/

### JSX (Javascript + XML)

> Transform cú pháp dạng gần như XML về thành Javascript. Giúp người lập trình có thể code ReactJS bằng cú pháp của XML thay vì sử dụng Javascript.

- Viết code js trong jsx được bao bởi cặp dấu ngoặc nhọn
- JSX cũng là 1 biểu thức có thể được return
- JSX có thể chứa các children

### Why use :

- Ngắn hơn, dễ hiểu hơn, dễ quản lý hơn code JS.
- JSX không làm thay đổi ngữ nghĩa của Javascript
- Giống với code html

```js
//code  JSX

<DashboardUnit data-index="2">
  <h1>Scores</h1>
  <Scoreboard className="results" scores={gameScores} />
</DashboardUnit>
```

```js
//code js

React.createElement(
  DashboardUnit,
  { 'data-index': '2' },
  React.createElement('h1', null, 'Scores'),
  React.createElement(Scoreboard, { className: 'results', scores: gameScores }),
);
```
condition-rendering react 
if
most basic conditional rendering
use to opt-out early from a rendering (guard pattern)
cannot be used within return statement and JSX (except self invoking function)
if-else
use it rarely, because it's verbose
instead, use ternary operator or logical && operator
cannot be used inside return statement and JSX (except self invoking function)
ternary operator
use it instead of an if-else statement
it can be used within JSX and return statement
logical && operator
use it when one side of the ternary operation would return null
it can be used inside JSX and return statement
switch case
avoid using it, because it's too verbose
instead, use enums
cannot be used within JSX and return (except self invoking function)
enums
use it for conditional rendering based on multiple states
perfect to map more than one condition
nested conditional rendering
avoid them for the sake of readability
instead, split out components, use if statements, or use HOCs
HOCs
components can focus on their main purpose
use HOC to shield away conditional rendering
use multiple composable HOCs to shield away multiple conditional renderings
external templating components
avoid them and be comfortable with JSX and JS

### Conditional in jsx

```js
return isTrue ? <p>True!</p> : null;

return isTrue && <p>True!</p>;
```

[Readmore](https://github.com/vasanthk/react-bits/blob/master/patterns/18.conditionals-in-jsx.md),
[Readmore !](https://engineering.musefind.com/our-best-practices-for-writing-react-components-dec3eb5c3fc8)

### Spread Attributes

```js
function App1() {
  return <Greeting firstName="Ben" lastName="Hector" />;
}

function App2() {
  const props = { firstName: 'Ben', lastName: 'Hector' };
  return <Greeting {...props} />;
}
```

### Convert HTML to JSX

```js
//code html
<div class="awesome" style="border: 1px solid red"></div>

//jsx
<div className="awesome" style={{ border: "1px solid red" }}>
```

[Tool Convert](https://transform.tools/html-to-jsx)

## CRA
he create-react-app CLI tool allows you to quickly create & run React applications with no configuration step
```jsx
# Installation
$ npm install -g create-react-app

# Create new project
$ create-react-app todo-app
$ cd todo-app

# Build, test and run
$ npm run build
$ npm run test
$ npm start
```

React, JSX, ES6, and Flow syntax support.
Language extras beyond ES6 like the object spread operator.
Autoprefixed CSS, so you don’t need -webkit- or other prefixes.
A fast interactive unit test runner with built-in support for coverage reporting.
A live development server that warns about common mistakes.
A build script to bundle JS, CSS, and images for production, with hashes and sourcemaps.


## Ref in JSX

[READMORE](https://www.newline.co/fullstack-react/articles/using-refs-in-react/)

- Ref cho phép truy cập trực tiếp vào node DOM hoặc các thành phần REACT được tạo từ method render -> refs sẽ return về một node mà chúng ta tham chiếu đến.

**Usage :**

- Managing focus, text selection, or media playback.
- Kích hoạt ảnh động
- Tích hợp với các library ngoài

**Way :**

- React.createRef()
- Callback refs
- String refs (legacy)
- Forwarding refs

**Note :**

> Tránh sử dụng ref cho bất cứ điều gì có thể được thực hiện khai báo

> Giá trị của ref khác nhau tùy thuộc vào loại nút:

- Khi ref attribute được sử dụng trên element HTML, phần tử ref được tạo trong hàm tạo React.createRef()sẽ nhận phần tử DOM cơ bản làm thuộc tính của nó trong current.
- Khi ref attribute được sử dụng trên một thành phần lớp tùy chỉnh, ref sẽ nhận được thể hiện được gắn kết của thành phần đó trong current.
- Không sử dụng ref attribute trên các functional component vì chúng không có instances. (nhưng vẫn có thể nếu nó truy cập đến DOM element or class component)

```js
function MyFunctionComponent() {
  return <input />;
}

class Parent extends React.Component {
  constructor(props) {
    super(props);
    this.textInput = React.createRef();
  }
  render() {
    // This will *not* work!
    return <MyFunctionComponent ref={this.textInput} />;
  }
}
```

React sẽ assign current property với DOM element khi component mounts, và assign nó trở về null khi nó unmounts. ref xảy ra update trước componentDidMount hay componentDidUpdate

#### Case

- Adding a Ref to a DOM Element
- Adding a Ref to a Class Component
- Adding a Ref to a Function Component (when access to DOM element or class component)
- DOM Refs to Parent Components (forwarding ref)

#### Forwarding Ref ????

> Ref forwarding is a technique for automatically passing a ref through a component to one of its children.

```js
const FancyButton = React.forwardRef((props, ref) => (
  <button ref={ref} className="FancyButton">
    {props.children}
  </button>
));

// You can now get a ref directly to the DOM button:
const ref = React.createRef();
<FancyButton ref={ref}>Click me!</FancyButton>;
```

[[]](https://reactjs.org/docs/forwarding-refs.html)

[Example](https://codesandbox.io/s/inspiring-borg-x020y)

## Controlled Component vs UnControlled Component

[READMORE](https://itnext.io/controlled-and-uncontrolled-component-design-pattern-in-react-21e8d40e46e)

#### Controlled Component

> Controlled Component let you maintain the state of the input fields in the state. But the catch is, it makes sure that the state is the single source of truth for data as well as UI.

> A Controlled Component takes its current value through props and notifies changes through callbacks. A parent component “controls” it by handling the callback and managing its own state and passing the new values as props to the controlled component.

In HTML, form elements such as `<input>`, `<textarea>`, and `<select>` typically maintain their own state and update it based on user input. In React, mutable state is typically kept in the state property of components, and only updated with setState().

```js
class ControlledFormInput extends React.Component {
  constructor(props) {
    super(props);
    this.handleChange = this.handleChange.bind(this);
    this.state = { value: 'Glad' };
  }

  handleChange(evt) {
    this.setState({ value: evt.target.value });
  }

  render() {
    return (
      <div>
        <h1>Hello {this.state.value}!</h1>
        <input
          type="text"
          value={this.state.value}
          onChange={this.handleChange}
          placeholder="Enter your name"
        />
      </div>
    );
  }
}
```

Advantages of using controlled components

- The state becomes the single source of truth for both the data and the UI.
- Makes debugging easier for bug-detection.
- Makes it easier to “lift the state up”, which in turn, makes it easier to break your components into functional and presentational components(Pure Components!).
- Easier to implement on-the-fly(onChange) validations.
- Easier to ensure inputs are in the specific format, like telephone numbers.

#### Uncontrolled Component

> An Uncontrolled Component is one that stores and maintains its own state internally. It's useful when developers only care about the final state rather than the intermediate state of the component.

In the React rendering lifecycle, the value attribute on form elements will override the value in the DOM. With an uncontrolled component, you often want React to specify the initial value, but leave subsequent updates uncontrolled. To handle this case, you can specify a defaultValue attribute instead of value

Advantages:
An uncontrolled form doesn’t have state, so every time you type, your component doesn’t re-render, increasing overall performance.

[You dont migth uncontrolled component](https://hackernoon.com/you-might-not-need-controlled-components-fy1g360o)

```js
class UncontrolledFormInput extends React.Component {
  constructor(props) {
    super(props);
    this.inputField = React.createRef();
    this.handleChange = this.handleChange.bind(this);
    this.state = { value: 'Glad' };
  }

  handleChange(evt) {
    this.setState({ value: this.inputField.current.value });
  }

  render() {
    return (
      <div>
        <h1>Hello {this.state.value}!</h1>
        {/* Attach the created ref: this.inputField */}
        <input
          type="text"
          ref={this.inputField}
          defaultValue={this.sate.value}
          onChange={this.handleChange}
          placeholder="Enter your name"
        />
      </div>
    );
  }
}
```

Usage:

- Your form is very simple and doesn’t need any instant validation?
- Even if any validations are needed, they are done only when the form is submitted?
- Values need to be retrieved only on “submit”. No fields are dependent on any other field(s)?

[Controlled Input vs Controlled Input](https://goshakkk.name/controlled-vs-uncontrolled-inputs-react/)

[Example react champ](https://github.com/adarshsingh1407/react-champ)

#### Conclusion:

Uncontrolled components remember what you typed(or clicked or changed) on the form in DOM while the controlled components keep that in the component’s state(or any data store you use, such as Redux).

## Single Source of Truth

> which is one master state for most if not all of your application, then send that state down as props to your child components.

By default input fields are not controllable which means it will render data from DOM, not state.

However, if you make your input listen to state instead (therefore making it controllable) it will not be able to change its value unless you change state

First effect you will notice is that, once you added value property to it, when you type in, nothing will change. And if you add onChange method that changes state, it will be fully controllable component that only listens to one source of truth; state, instead of DOM events

[READMORE](https://stackoverflow.com/questions/47182888/what-does-the-single-source-of-truth-mean)

## Functional setState

## Component

Three tenets(nguyên lý) of component

- **Component Nesting**: A component can shown in another component
- **Component Resuability** (Tái sử dụng): We want to make components that can be easily reused through out application
- **Component Configuration**: We should be able to configure a component when it is created

Tips:

- Should use props.children to reusue component
component names start with capital letter because only HTML elements and SVG tags can begin with a lowercase letter.


## Function Component vs Class Component

Usage Function Component

- When **don't need state, lifecycle,** if u want to use state in FC , u use React Hook
- To **resue** component

Usage Class Component:

- When you **need lifecycle, state**
- Fetching async data in lifecyle


## HOC 
> A higher-order component (HOC) is a function that takes a component and returns a new component

**Usage :**
- Code reuse, logic and bootstrap abstraction.
- Render hijacking.
- State abstraction and manipulation.
- Props manipulation.

props proxy for HOC component? ?? ???

```jsx
function HOC(WrappedComponent) {
  return class Test extends Component {
    render() {
      const newProps = {
        title: 'New Header',
        footer: false,
        showFeatureX: false,
        showFeatureY: true
      }

      return <WrappedComponent {...this.props} {...newProps} />
    }
  }
}
```
### stateless components vs statefull components

If the behaviour is independent of its state then it can be a stateless component
If the behaviour of a component is dependent on the state of the component then it can be termed as stateful component
## Props

> Props are arguments passing data from parent component to child component

**Usage :** 

Goal is to customize or configure child component

**Note :**

- [Whether you declare a component as a function or a class, it must never modify its own props](https://reactjs.org/docs/lifting-state-up.html)
- Can change props via parent component ([use Callbacks](https://www.freecodecamp.org/news/how-to-update-a-components-prop-in-react-js-oh-yes-it-s-possible-f9d26f1c4c6d/) and  [lifting-state-up](https://reactjs.org/docs/lifting-state-up.html)) 
- [Should use super(props) in construtor](https://overreacted.io/why-do-we-write-super-props/)
- [not use props to generate state](
https://github.com/vasanthk/react-bits/blob/master/anti-patterns/01.props-in-initial-state.md)
- Props data is immutable (read-only)

### lifting-state-up
Khi một số thành phần cần chia sẻ cùng một dữ liệu thay đổi thì nên nâng trạng thái chia sẻ lên đến tổ tiên chung gần nhất của chúng. Điều đó có nghĩa là nếu hai thành phần con chia sẻ cùng một dữ liệu từ cha mẹ của nó, sau đó chuyển trạng thái sang cha mẹ thay vì duy trì trạng thái cục bộ trong cả hai thành phần con.

## State

> State is where you store property values that belongs to the component

**Usage :** 

When a component needs to keep track of information between renderings the component itself can create, update, and use state

**Note :**

- Component will re-render after sate change
- Only use in class component or function component if you use Hook
- State must be initiazed when component created
- Can only update sate with setState in class component
- [Nếu có sử dụng lại PrevState thì phải sử dụng callback để cập nhật state](https://reactjs.org/docs/faq-state.html#what-does-setstate-do)
- initialize your state like ```state = {p : 'state'}``` if your Babel setup has support for class fields
- Nên đơn giản và giảm số lượng state 
- Không thay đổi state trực tiếp (component sẽ không được render lại) => use setState
- không nên gọisetState() trong constructor()
- [not use prop to generate state](https://reactjs.org/blog/2018/06/07/you-probably-dont-need-derived-state.html)

- [không mutate state](https://kipalog.com/posts/Vi-sao-khong-nen-mutate-state-va-khong-nen-dung-cloneDeep-trong-react)

Using props to generate state in getInitialState often leads to duplication of “source of truth”, i.e. where the real data is. This is because getInitialState is only invoked when the component is first created.

```js
class MyComponent extends Component {
  state = {
    counter: this.props.initialCounterValue,
  };
}
```

https://upmostly.com/tutorials/how-to-use-the-setstate-callback-in-react
https://www.freecodecamp.org/news/get-pro-with-react-setstate-in-10-minutes-d38251d1c781/
https://blog.logrocket.com/a-guide-to-usestate-in-react-ecb9952e406c/

## Flux


## LifeCycle

<img src="https://cdn.ereka.vn/2018/12/27/e26366887af4aaaccaed31f3bea6a565.png?w=600" alt="Smiley face" height="100%" width="100%" style="text-align:center">

### **I. Mounting**

It is at this phase the component is created (your code, and react’s internals) then inserted into the DOM

**1. Constructor()** is the first method invoked — before the component is mounted to the DOM.

Note: It's NOT where to introduce any side-effects or subscriptions such as event handlers.

**constructor called only once :** React's reconciliation algorithm assumes that without any information to the contrary, if a custom component appears in the same place on subsequent renders, it's the same component as before, so reuses the previous instance rather than creating a new one.

```js
 constructor(props) {
    super(props);
    this.state = {favoritecolor: "red"};
  }
```

**2. Static getDerivedStateFromProps()** is called (or invoked) before the component is rendered to the DOM on initial mount.

Usage:

- When update internal state in response to a change in props.\*
- resetting a video or audio element when the source changes
- refreshing a UI element with updates from the server
- closing an accordion element when the contents change

```js
static getDerivedStateFromProps(props, state) {
  return { blocks: createBlocks(props.numberOfBlocks) };
}

this.sate -> {blocks: Array(...)}
```

**3. Render()** is the method to render elements to the DOM

```js
 render() {
    return <>
            <div>Hello</div>
            <div>World</div>
      </>
   }
```

Note:

- <> is short way of React.Fragment to return multiple elements
- you could return a Boolean or null within the render method:
- Portals ???

```js
 render() {
    return (2 + 2 === 5) && <div>Hello World</div>;
  }
```

**4. ComponentDidMount()** is invoked immediately after the component is mounted to the DOM

Use:

- to grab a DOM node from the component tree immediately after it’s mounted,
- Fetching data, ...
- Draw on a `<canvas>` element that you just rendered
- add event listeners

```js
 componentDidMount() {
   document.querySelector("body).appendChild(this.el);

   setTimeout(() => {
     this.setState({favoritecolor: "yellow"})
   }, 1000)


 }

```

### **II. Updated**

Whenever a **change is made to the state or props** of a react component,the component is re-rendered. In simple terms, the component is updated. This is the updating phase of the component lifecycle.

**1. static getDerivedStateFromProps()**

**2. shouldComponentUpdate()** control re-render behavior.

Usage:

- for performance optimisation measures.

```js
shouldComponentUpdate(nextProps, nextState) {
  console.log("should component update is called here!");
  return nextState.cars.length < this.state.cars.length;
}
```

**3. render()**

**4. getSnapshotBeforeUpdate()** have access to the props and state before the update, meaning that even after the update, you can check what the values were before the update

```js
constructor(props) {
    super(props);
    this.state = {favoritecolor: "red"};
  }
  componentDidMount() {
    setTimeout(() => {
      this.setState({favoritecolor: "yellow"})
    }, 1000)
  }
  getSnapshotBeforeUpdate(prevProps, prevState) {
    document.getElementById("div1").innerHTML =
    "Before the update, the favorite was " + prevState.favoritecolor;
    //PrevState is red
  }
  componentDidUpdate(prevProps, prevState, snapshot) {
    document.getElementById("div2").innerHTML =
    "The updated favorite is " + this.state.favoritecolor;
    //state is yellow
  }
```

The snapshot is the value returned from the getSnapshotBeforeUpdate method. You can set state here but it should always be inside a conditional statement.

### **III. Unmounting**

The following method is invoked during the component unmounting phase. (when a component is removed from the DOM)

**1. componentWillUnmount** is invoked immediately before a component is unmounted and destroyed

Usage:

- Clean up logic should go, clearing counters and caches, cancel API requests or removing things like event listeners.

```js
// e.g add event listener
componentDidMount() {
    el.addEventListener()
}

// e.g remove event listener
componentWillUnmount() {
    el.removeEventListener()
 }
```

**IV. Error Handling**

**1. getDerivedStateFromError()** used to gracefully handle errors called error boundaries. If a child component of a parent component has an error we can use this method to display an error screen.

```js
static getDerivedStateFromError(error) {
 // Update state so the next render will show the fallback UI.
 return { hasError: true };
}

render() {
 if (this.state.hasError) {
   // You can render any custom fallback UI
   return <h1>Something went wrong.</h1>;
 }

 return this.props.children;
}
```

**2. componentDidCatch()** send the error or info received to an external logging service. Unlike getDerivedStateFromError, the componentDidCatchallows for side-effects:

```js
componentDidCatch(error, info) {
    logToExternalService(error, info) // this is allowed.
        //Where logToExternalService may make an API call.
}
```

### **DOCS**

[React 16 lifecyle have most case](https://blog.bitsrc.io/react-16-lifecycle-methods-how-and-when-to-use-them-f4ad31fb2282)

[New lifecyle detail + example](https://blog.logrocket.com/the-new-react-lifecycle-methods-in-plain-approachable-language-61a2105859f3/)

[W3school best ex getSnapshotBeforeUpdate](https://www.w3schools.com/react/react_lifecycle.asp)

[Short theory pusher ](https://pusher.com/tutorials/lifecycle-methods-react-16#getderivedstatefromerror-)

## Portals

Portal is a recommended way to render children into a DOM node that exists outside the DOM hierarchy of the parent component.

```jsx 
ReactDOM.createPortal(child, container)
```

The first argument is any render-able React child, such as an element, string, or fragment. The second argument is a DOM element.


## Bind methods

```jsx
//Binding in Constructor

class Component extends React.Componenet {
  constructor(props) {
    super(props)
    this.handleClick = this.handleClick.bind(this)
  }

  handleClick() {
    // ...
  }
}
// Public class fields syntax

handleClick = () => {
  console.log('this is:', this)
}

// Arrow functions in callbacks
<button onClick={(event) => this.handleClick(event)}>
  {'Click me'}
</button>
```
**Note :**

[Nếu callback is passed như prop cho child components, các thành phần đó có thể được kết xuất lại ảnh hưởng đến hiệu suất](https://stackoverflow.com/a/52788669)

[=> Example](https://codesandbox.io/s/learn-react-ww0h9)

**pass a parameter to an event handler**

```jsx
//use arrow function in callback
handleClick = (id) => {
    console.log("Hello, your ticket number is", id)
};
<button onClick={() => this.handleClick(id)} >Click</button>

//the same method above

handleClick = (id) => () => {
    console.log("Hello, your ticket number is", id)
};

<button onClick={this.handleClick(id)} >Click</button>

//use bind
<button onClick={this.handleClick.bind(this, id)} />

```

**Orther way create child pure component and use callback => không render lại child component**

[=> Example](https://codesandbox.io/s/learn-react-ww0h9)


## Key 

Keys giúp React xác định mục nào đã thay đổi (thêm / xóa / sắp xếp lại).

[Why use keys in react and case](https://medium.com/@adhithiravi/why-do-i-need-keys-in-react-lists-dbb522188bbb)

- Mọi mục trong danh sách đều có một khóa duy nhất.
- Không nên sử dụng index làm key trừ khi danh sách đó là danh sách tĩnh (không bổ sung / sắp xếp lại / xóa vào danh sách).
- Không bao giờ sử dụng các khóa không ổn định như Math.random () để tạo khóa => [giảm hiệu suất](https://reactjs.org/docs/reconciliation.html#recursing-on-children) 

## Context

Context provides a way to pass data through the component tree without having to pass props down manually at every level. For example, authenticated user, locale preference, UI theme need to be accessed in the application by many components.

```jsx 
const {Provider, Consumer} = React.createContext(defaultValue)
```
## fragments

It's common pattern in React which is used for a component to return multiple elements. Fragments let you group a list of children without adding extra nodes to the DOM.

```jsx
render() {
  return (
    <React.Fragment>
      <ChildA />
      <ChildB />
      <ChildC />
    </React.Fragment>
  )
}

// Or
render() {
  return (
    <>
      <ChildA />
      <ChildB />
      <ChildC />
    </>
  )
}
```

**Why use :**
- Fragments are a bit faster and use less memory by not creating an extra DOM node. This only has a real benefit on very large and deep trees.
- Some CSS mechanisms like Flexbox and CSS Grid have a special parent-child relationships, and adding divs in the middle makes it hard to keep the desired layout.
- The DOM Inspector is less cluttered.


## React dom 

The react-dom package provides DOM-specific methods that can be used at the top level of your app. Most of the components are not required to use this module. Some of the methods of this package are:

render()
hydrate()
unmountComponentAtNode()
findDOMNode()
createPortal()

## styles in React?

The style attribute accepts a JavaScript object with camelCased properties rather than a CSS string. This is consistent with the DOM style JavaScript property, is more efficient, and prevents XSS security holes.

const divStyle = {
  color: 'blue',
  backgroundImage: 'url(' + imgUrl + ')'
};

function HelloWorldComponent() {
  return <div style={divStyle}>Hello World!</div>
}
Style keys are camelCased in order to be consistent with accessing the properties on DOM nodes in JavaScript (e.g. node.style.backgroundImage).


## How do you memoize a component?
There are memoize libraries available which can be used on function components. For example moize library can memoize the component in another component.

import moize from 'moize'
import Component from './components/Component' // this module exports a non-memoized component

const MemoizedFoo = moize.react(Component)

const Consumer = () => {
  <div>
    {'I will memoize the following entry:'}
    <MemoizedFoo/>
  </div>
}
Update: Since React v16.6.0, we have a React.memo. It provides a higher order component which memoizes component unless the props change. To use it, simply wrap the component using React.memo before you use it.

  const MemoComponent = React.memo(function MemoComponent(props) {
    /* render using props */
  });
  OR
  export default React.memo(MyFunctionComponent);

## PropType
The basic PropTypes for primitives and complex objects are:

PropTypes.array
PropTypes.bool
PropTypes.func
PropTypes.number
PropTypes.object
PropTypes.string
There are two more PropTypes that can define a renderable fragment (node), e.g. a string, and a React element:

PropTypes.node
PropTypes.element


# Other

synthetic events
Fiber

lazy function
error boundaries 
props type
dangerouslySetInnerHTML
conditionally render components 

React.StrictMode
React Mixins
Pointer Events

re-render the view when the browser is resized

The ReactDOMServer object enables you to render components to static markup (typically used on node server). This object is mainly used for server-side rendering (SSR). The following methods can be used in both the server and browser environments:

renderToString()
renderToStaticMarkup()
For example, you generally run a Node-based web server like Express, Hapi, or Koa, and you call renderToString to render your root component to a string, which you then send as response.

// using Express
import { renderToString } from 'react-dom/server'
import MyPage from './MyPage'

app.get('/', (req, res) => {
  res.write('<!DOCTYPE html><html><head><title>My Page</title></head><body>')
  res.write('<div id="content">')
  res.write(renderToString(<MyPage/>))
  res.write('</div></body></html>')
  res.end()
})

## enable production mode in React
You should use Webpack's DefinePlugin method to set NODE_ENV to production, by which it strip out things like propType validation and extra warnings. Apart from this, if you minify the code, for example, Uglify's dead-code elimination to strip out development only code and comments, it will drastically reduce the size of your bundle.
