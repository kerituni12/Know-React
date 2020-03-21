What is React Router?
React Router is a powerful routing library built on top of React that helps you add new screens and flows to your application incredibly quickly, all while keeping the URL in sync with what's being displayed on the page.

⬆ Back to Top

How React Router is different from history library?
React Router is a wrapper around the history library which handles interaction with the browser's window.history with its browser and hash histories. It also provides memory history which is useful for environments that don't have global history, such as mobile app development (React Native) and unit testing with Node.


What is the purpose of push() and replace() methods of history?
A history instance has two methods for navigation purpose.

push()
replace()
If you think of the history as an array of visited locations, push() will add a new location to the array and replace() will replace the current location in the array with the new one.


How do you programmatically navigate using React Router v4?
There are three different ways to achieve programmatic routing/navigation within components.

Using the withRouter() higher-order function:

The withRouter() higher-order function will inject the history object as a prop of the component. This object provides push() and replace() methods to avoid the usage of context.

import { withRouter } from 'react-router-dom' // this also works with 'react-router-native'

const Button = withRouter(({ history }) => (
  <button
    type='button'
    onClick={() => { history.push('/new-location') }}
  >
    {'Click Me!'}
  </button>
))
Using <Route> component and render props pattern:

The <Route> component passes the same props as withRouter(), so you will be able to access the history methods through the history prop.

import { Route } from 'react-router-dom'

const Button = () => (
  <Route render={({ history }) => (
    <button
      type='button'
      onClick={() => { history.push('/new-location') }}
    >
      {'Click Me!'}
    </button>
  )} />
)
Using context:

This option is not recommended and treated as unstable API.

const Button = (props, context) => (
  <button
    type='button'
    onClick={() => {
      context.history.push('/new-location')
    }}
  >
    {'Click Me!'}
  </button>
)

Button.contextTypes = {
  history: React.PropTypes.shape({
    push: React.PropTypes.func.isRequired
  })
}

Why you get "Router may have only one child element" warning?
You have to wrap your Route's in a <Switch> block because <Switch> is unique in that it renders a route exclusively.

At first you need to add Switch to your imports:

import { Switch, Router, Route } from 'react-router'
Then define the routes within <Switch> block:

<Router>
  <Switch>
    <Route {/* ... */} />
    <Route {/* ... */} />
  </Switch>
</Router>
⬆ Back to Top

How to pass params to history.push method in React Router v4?
While navigating you can pass props to the history object:

this.props.history.push({
  pathname: '/template',
  search: '?name=sudheer',
  state: { detail: response.data }
})
The search property is used to pass query params in push() method.

⬆ Back to Top

How to implement default or NotFound page?
A <Switch> renders the first child <Route> that matches. A <Route> with no path always matches. So you just need to simply drop path attribute as below

<Switch>
  <Route exact path="/" component={Home}/>
  <Route path="/user" component={User}/>
  <Route component={NotFound} />
</Switch>
⬆ Back to Top

How to get history on React Router v4?
Create a module that exports a history object and import this module across the project.

For example, create history.js file:

import { createBrowserHistory } from 'history'

export default createBrowserHistory({
  /* pass a configuration object here if needed */
})
You should use the <Router> component instead of built-in routers. Imported the above history.js inside index.js file:

import { Router } from 'react-router-dom'
import history from './history'
import App from './App'

ReactDOM.render((
  <Router history={history}>
    <App />
  </Router>
), holder)
You can also use push method of history object similar to built-in history object:

// some-other-file.js
import history from './history'

history.push('/go-here')
⬆ Back to Top

How to perform automatic redirect after login?
The react-router package provides <Redirect> component in React Router. Rendering a <Redirect> will navigate to a new location. Like server-side redirects, the new location will override the current location in the history stack.

import React, { Component } from 'react'
import { Redirect } from 'react-router'

export default class LoginComponent extends Component {
  render() {
    if (this.state.isLoggedIn === true) {
      return <Redirect to="/your/redirect/page" />
    } else {
      return <div>{'Login Please'}</div>
    }
  }
}

How to add Google Analytics for React Router?
Add a listener on the history object to record each page view:

history.listen(function (location) {
  window.ga('set', 'page', location.pathname + location.search)
  window.ga('send', 'pageview', location.pathname + location.search)
})

Switch (same swtichjs)thực hiện chức năng match router nếu k có nó sẽ chọn cái cuối cùng


:slug -> params.slug
? -> location.query 