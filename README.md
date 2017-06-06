# scalajs-vuetags
Type safe Scala templates for Vue.js

This project is still in the design phase. The API is still subject to constant changes, and is extremely depedendant on how the design for [scalajs-vue-facade](https://github.com/daniel-shuy/scalajs-vue-facade) turns out.

# Design
A Scala.js Facade for Vue.js alone is not enough to provide typesafety to Vue.js, since its HTML templates are still not type safe, and we will not be able to use [JSX for Render Functions](https://vuejs.org/v2/guide/render-function.html#JSX) (requires Babel).

Inspired by [ScalaTags](https://github.com/lihaoyi/scalatags), this framework aims to provide type safe Scala templates for Vue.js, for both HTML and Render Functions.

The current API looks like:

## HTML
### Vue.js example 
- from https://vuejs.org/v2/guide/index.html#Composing-with-Components
```html
<html>
  <body>
    <div id="vue">
      <todo-item
        v-for="item in groceryList"
        :key="item.id"
        :todo="item.text">
      </todo-item>
    </div>
  </body>
</html>
```

### ScalaTags
```scala
html(
  body(
    div(id:="vue")(
      for ((item <- groceryList) yield TodoItem(key = item.id, todo = item.text))
    )
  )
)
```
- We will not be able to use the := operator like ScalaTags, because we need to restrict the props that can be set on each component, which requires them to be method arguments instead.
- Like ScalaTags, we want to keep the syntax as Scala-like as possible.

### Play Framework (Twirl)
```scala
@(groceryList: List[Grocery])

<html>
  <body>
    <div id="vue">
      @for(item <- groceryList) {
        @TodoItem(key = item.id, todo = item.text)
      }
    </div>
  </body>
</html>
```


## Render Functions (Scala.js)
### Vue.js examples
- from https://vuejs.org/v2/guide/render-function.html#JSX

#### Without JSX
```javascript
new Vue({
  el: '#vue',
  render: function (createElement) {
    return createElement('anchored-heading', {
      props: {
        level: 1
      }
    }, [
      createElement('span', 'Hello'),
      ' world!'
    ]
  )}
})
```

#### With JSX
```javascript
new Vue({
  el: '#vue',
  render: function (h) {
    <AnchoredHeading level={1}>
      <span>Hello</span> world!
    </AnchoredHeading>
  }
})
```

### Scala.js
```scala
new Vue(
  ComponentOptions
    .el("#vue")
    .render(
      implicit createElement =>
        AnchoredHeading(level = 1)(
          span("Hello"), " world!"
        )
    )
)
```

Notice how similar both templates look. One of the aims for this project is to have the same syntax for both HTML and Render Function templates (so that you don't have to learn 2 different templates to use both).
