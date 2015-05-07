# React Style Guide

A set of guidelines for writing consistent, readable, and effective React code.

Feedback & debate is welcome. Let's make this a resource that is useful to every React developer.

## Follow your JavaScript style guide

This guide is meant to address React-specific concerns. React is just JavaScript and your React code will be surrounded by other JavaScript. Follow your JS style guide.

**Always** and **never** have obvious meaning. **Rarely** means that there may be legitimate cause to break a rule, but this case should occur only a couple of times per application.

Examples:

* [idiomatic.js](https://github.com/rwaldron/idiomatic.js)
* [Google](https://google-styleguide.googlecode.com/svn/trunk/javascriptguide.xml)
* [airbnb](https://github.com/airbnb/javascript)

## Application Organization

* **Always** have only one public component per file.
* **Always** mirror class organization with file organization. For example, if you component is `App.Components.Foo.Bar`, it should be defined in a file called `app/components/foo/bar`.
* **Rarely**, public components may have private components (components which are only used from the public component, which are not accessible to the rest of the application).

## state, props, and methods

It is important to properly decide whether each datum in your component is `props`, `state`, or a method.

| Situation                                | props, state or method |
|------------------------------------------|-------|
| Always passed down from parent component | props |
| Sometimes passed down from parent        | props (list default value in getDefaultProps) |
| Can be calculated from props and state   | method |
| this component _owns_ it, it isn't passed in, and it can't be calculated from props or state | state |

* `state` should be used sparingly. 
* Have no more than 1-2 state variables per component. 
* Many components should have 0 state variables.

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

* **Always** define propTypes for all components.
* **Always** list all props in propTypes.

## reserved props

* **Never** try to access the special props `key` and `ref` from inside the component which receives them (i.e. this.props.key). It won't work anyway.
* **Always** use `key` on _dynamic_ child components where order or identity matter.
* **Rarely** use `ref`. Overuse of `ref` indicates that you need to reconsider data flow in your components (use `props`).
* **Never** use `dangerouslySetInnerHTML`, unless you are writing a live HTML code previewer/wysiwyg.

## getDefaultProps

* **Never** use complex/reference types in `getDefaultProps`. This is because the method runs once per class (not instance), and so complex type objects are shared. This leads to bugs.

## lifecycle methods

* `componentDidMount` is for things which should happen once the component is in the DOM, for example AJAX operations, setting timers and integrating with other, more primitive frameworks.
* **Always** use`componentWillUnmount` to clean up everything that `componentDidMount` sets up. Event listeners and timers should be cleared. Ties to external frameworks should be torn down.
* **Rarely** use the other lifecycle methods.
* **Never** set state in `componentWillUpdate` or `componentDidUpdate`.

## JSX and `render` style

* **Never** have more than one opening or closing component tag per line.
    * Bad: `<ul><li>Ouch!</li></ul>`
    * Better:
      ```
        <ul>
          <li>
            A little better.
          </li>
        </ul>
      ```
* **Always** indent nested elements per your styleguide for HTML/JS. Two space tab is recommended.
* **Never** use more than three levels of nesting in block of JSX.
* **Always**: Conditionals should fall outside of JSX. You may extract the conditional logic into a separate method.
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
  
* **Never** make assignments of any kind in `render` or render "helpers".
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

* **Never** put more than 3 attributes on a single line. Use one attribute per line form for long props lists.
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

* **Always** follow your JS style guide's conventions for spaces around `{` and `}`. If there is not rule, use no spaces consistently.
