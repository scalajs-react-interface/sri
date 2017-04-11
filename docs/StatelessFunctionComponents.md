# Functions as Stateless Components

React 0.14 introduced [stateless functional components](https://facebook.github.io/react/blog/2015/10/07/react-v0.14.html#stateless-functional-components) .

>In idiomatic React code, most of the components you write will be stateless, simply composing other components. We’re introducing a new, simpler syntax for these components where you can take props as an argument and return the element you want to render


**Note : even though we're just creating/passing functions, react internally creates instances and manges them.Currently we don't gain any performance from these infact these are slower than components with `shouldComponentUpdate` implemented.**


#### Stateless Component with props

```scala

 object StatelessComponent {
  
  val component = (props: String) => Text(s"Hello Stateless ${props}")
 
  def apply(props : String) = CreateElementSF(component,props)
  
 }

```

#### Stateless Component No Props

```scala

 object StatelessComponent {
  
  val component = () => Text(s"Hello Stateless No Props")
 
  def apply() = CreateElementSFNoProps(component)
  
 }

```
