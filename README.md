React Stateful Functional Comopnents
---------------------
[![Build Status](https://img.shields.io/travis/mpeyper/react-stateful-functional-components/master.svg?style=flat-square)](https://travis-ci.org/mpeyper/react-stateful-functional-components) 
[![npm version](https://img.shields.io/npm/v/react-stateful-functional-components.svg?style=flat-square)](https://www.npmjs.com/package/react-stateful-functional-components) 
[![npm downloads](https://img.shields.io/npm/dm/react-stateful-functional-components.svg?style=flat-square)](https://www.npmjs.com/package/react-stateful-functional-components)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg?style=flat-square)](LICENSE)

If you prefer to write functional components, but have a need to manage a small amount of state, but don't want to go through all the boilerplate of Redux (or your external statment management library of choice), then the `stateful` higher-order component is just what you're after.

Inspired by the [React Redux bindings](https://github.com/reactjs/react-redux), `stateful` allows you to seperate the state and view into container and presentational components so you can enjoy all the simplicity of functional components, with the addition of some stateful behaviour.

This is ideal for those small pieces of state that don't belong with your application state, such as temporary text in an `input`, the open state of a menu, the currently selected tab, or many other things that only the single component cares about.  Redux (or your external statment management library of choice) is still recommended for any application state you want to store.

## Installation

### NPM

```
npm install --save react-stateful-functional-components
```

This assumes that you’re using [npm](http://npmjs.com/) package manager with a module bundler like [Webpack](https://webpack.js.org/) or [Browserify](http://browserify.org/) to consume [CommonJS modules](http://webpack.github.io/docs/commonjs.html).

## Usage

### Basic

```
import stateful from 'react-stateful-functional-components'

const MyComponent = ({text, setState}) => {
    return <input value={text} onChange={(e) => setState({ text: e.target.value })} />
}

export default stateful()(TestComponent)
```

##### With initial state

```
import stateful from 'react-stateful-functional-components'

const MyComponent = ({text, setState}) => {
    return <input value={text} onChange={(e) => setState({ text: e.target.value })} />
}

const initialState = { text: "initial value" }

export default stateful(initialState)(TestComponent)
```

### Presentation of State

```
import stateful from 'react-stateful-functional-components'

const MyComponent = ({text, setText}) => {
    return <input value={text} onChange={(e) => setText(e.target.value)} />
}

const mapStateToProps = (state) => {
    return {
        text: state.myText
    }
}

const mapSetStateToProps = (setState) => {
    return {
        setText: (myText) => setState({ myText })
    }
}

export default stateful(mapStateToProps, mapSetStateToProps)(TestComponent)
```

##### With initial state

```
import stateful from 'react-stateful-functional-components'

const MyComponent = ({text, setText}) => {
    return <input value={text} onChange={(e) => setText(e.target.value)} />
}

const initialState = { myText: "initial value" }

const mapStateToProps = (state = initialState) => {
    return {
        text: state.myText
    }
}

const mapSetStateToProps = (setState) => {
    return {
        setText: (myText) => setState({ myText })
    }
}

export default stateful(mapStateToProps, mapSetStateToProps)(TestComponent)
```

### Props

```
import stateful from 'react-stateful-functional-components'

const MyComponent = ({text, setText}) => {
    return <input value={text} onChange={(e) => setText(e.target.value)} />
}

const mapStateToProps = (state, ownProps) => {
    return {
        text: ownProps.prefix + state.text
    }
}

const mapSetStateToProps = (setState, ownProps) => {
    return {
        setText: (text) => setState({ text: text.replace(ownProps.prefix, '') })
    }
}

export default stateful(mapStateToProps, mapSetStateToProps)(TestComponent)
```

### Merging state, setState and props

```
import stateful from 'react-stateful-functional-components'

const MyComponent = ({text, setText, submit, submitted}) => {
    return (
        <div>
            <input value={text} onChange={(e) => setText(e.target.value)} />
            <button onClick={submit}>Submit</button>
            <p>Submitted: {submitted}</p>
        <div>
    )
}

const mapStateToProps = (state) => {
    return {
        text: state.myText
        submitted: state.submitted
    }
}

const mapSetStateToProps = (setState) => {
    return {
        setText: (myText) => setState({ myText }),
        submit: (text) => setState({ submitted: text })
    }
}

const mergeProps = (mappedState, mappedSetState, ownProps) => {
    return {
        ...mappedState,
        ...mappedSetState,
        submit: () => mappedSetState.submit(ownProps.prefix + mappedState.myText)
    }
}

export default stateful(mapStateToProps, mapSetStateToProps, mergeProps)(TestComponent)
```