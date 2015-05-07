# React Style Guide

A set of guidelines for writing consistent, readable, and effective React code.

Feedback & debate is welcome. Let's make this a resource that is useful to every React developer.

## Follow your JavaScript style guide

This guide is meant to address React-specific concerns. React is just JavaScript and your React code will be surrounded by other JavaScript. Follow your JS style guide.

Examples:

* [idiomatic.js](https://github.com/rwaldron/idiomatic.js)
*  [Google](https://google-styleguide.googlecode.com/svn/trunk/javascriptguide.xml)
* [airbnb](https://github.com/airbnb/javascript)

## Application Organization

* One public component per file.
* Mirror file organization and class organization. For example, if you component is `App.Components.Foo.Bar`, it should be defined in a file called `app/components/foo/bar`.
* Use sparingly: public components may have private components which only they call.

## state, props, and methods

It is important to properly decide whether each datum in your component is `props`, `state`, or a method.

|-|-|
| Always passed down from parent component | props |
| Sometimes passed down from parent        | props (list default value in getDefaultProps) |
| Can be calculated from props and state | make it a method |
| this component _owns_ it, it isn't passed in, and it can't be calculated from props or state | state |

`state` should be rare. Have no more than 1-2 state variables per component. Many components should have 0 state variables.

## Component Organization

Follow this order for component members (**bold** methods are commonly used):

  1.  **displayName**
  2.  statics
  3.  **propTypes**
  4.  **getDefaultProps**
  5.  getInitialState
  6.  mixins
  7.  componentWillMount
  8.  **componentDidMount**
  9.  componentWillReceiveProps
  10. shouldComponentUpdate
  11. componentWillUpdate
  12. componentDidUpdate
  13. **componentWillUnmount**
  14. componentDidUnmount
  15. _other methods_
  16. render "helpers"
  17. **render**

## propTypes

* propTypes are mandatory for all components.
* All props must be listed in propTypes.

## reserved props

* `key` and `ref` are special, reserved props. A component should not try to access the value of its own `key` or `ref` props (i.e. don't try `this.props.key`). It won't work anyway.
* Use `key` on child components for its intended purpose.
* You should not use `ref`.
* You should not use `dangerouslySetInnerHTML`.

## getDefaultProps

* No complex/reference types in `getDefaultProps`. This is because the method runs once per class (not instance), and so complex type objects are shared. This leads to bugs.

## lifecycle methods

* `componentDidMount` is for things which should happen once the component is in the DOM, for example AJAX operations, setting timers and integrating with other, more primitive frameworks.
* `componentWillUnmount` must clean up everything that `componentDidMount` sets up. Event listeners and timers should be cleared. Ties to external frameworks should be torn down.
* The others should be used rarely.
* Do not set state in `componentWillUpdate` or `componentDidUpdate`.

## JSX and `render` style

* One opening or closing component tag per line.
    * Bad: `<ul><li>Ouch!</li></ul>`
    * Better:
      ```
        <ul>
          <li>
            A little better.
          </li>
        </ul>
      ```
* Indent nested elements per your styleguide for HTML/JS. Two space tab is recommended.
* No more than three levels of nesting in block of JSX.
* Conditionals should fall outside of JSX. You may extract the conditional logic into a separate method.
  * Bad:
  ```
  return <div>
    {list.length ? <ListDisplay list={list} /> : 'This is empty!'}
  </div>;
  ```

  * Better:
  ```
  if(list.length) {
    return <ListDisplay list={list} />
  } else {
    return <div>
      This is empty
    </div>;
  }
  ```
  
* Do not make assignments of any kind in `render` or render "helpers".
* Assignments in `render` are often used to handle condition rendering. They make `render` read less declaratively (less like a template).
  * Bad:
    ```
    render: function() {
      if(age >= 21) {
        message = <span>Would you like a drink?</span>;
      } else {
        message = <span>Get out of here!</span>;
      }

      return <div>
        <h1>Welcome to my bar</h1>
        {message}
      </div>;
    }
    ```

  * Better:
    ```
      renderMessage: function() {
        if(age >= 21) {
          return <span>Would you like a drink?</span>;
        } else {
          return <span>Get out of here!</span>;
        }
      },
      render: function() {
        return <div>
          <h1>Welcome to my bar</h1>
          {@renderMessage()}
        </div>;
      }
    ```

  `renderMessage` is an example of a render helper.


### JSX Attributes/Props

* No more than 3 attributes on a single line. Use one attribute per line form for long props lists.
  * Bad: `<Foo first={...} second={...} third={...} forth={...} fifth={...} />`
  * Better:
  ```
    <Foo
      first={...}
      second={...}
      third={...}
      forth={...}
      fifth={...}
    />
  ```

* Generally, no spaces around `{` and `}`. If your JS style guide calls for spaces around those symbols, be consistent with the style guide.
