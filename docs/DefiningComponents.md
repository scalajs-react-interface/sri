

#### Component with Props and No State


```scala

@ScalaJSDefined
class HelloMessage extends ReactComponentP[HelloMessage.Props] {
  def render() = {
   Text(s"Hello ${props.name}")
  }
}

object HelloMessage { 
  case class Props(name : String)
  def apply(props:Props) = CreateElement[HelloMessage](props)
}

//usage
HelloMessage(HelloMessage.Props("Scala.js"))
```


#### Component with State  and No Props


```scala
@ScalaJSDefined
class HelloMessage extends ReactComponentS[HelloMessage.State] {

  inititalState(State("initial"))
  
  def render() = {
   Text(s"Hello ${state.name}")
  }
  
}

object HelloMessage {
 case class State(name:String)
 def apply() = CreateElementNoProps[HelloMessage]()
}

//usage

HelloMessage()

```


#### Component with Props and State


```scala
@ScalaJSDefined
class Counter extends ReactComponent[Counter.Props,Counter.State] {
  initialState(State(count= 0))
  def render() = {
    View(
      Text(s"Hello ${props.name}"),
      Text(onPress = tick _)(s"Clicks: ${state.count}")
    )
  }
  def tick() = {
   setState((state:State) => state.copy(count = state.count + 1))
  }
}

object Counter {
  case class Props(name: String)
  case class State(count: Int)

  def apply(props:Props) = CreateElement[Counter](props)
}

//usage
HelloMessage(HelloMessage.Props("Scala.js"))
```

***Note:*** If you want to define initialState from Props you need to place `initialState(State(count= props.count))` call inside `componentWillMount` life cycle method because props are not available in primary constructor, that being said [Props in initialState is an AntiPattern](http://reactjs.cn/react/tips/props-in-getInitialState-as-anti-pattern.html),
if you're using data management libraries like [diode](https://github.com/suzaku-io/diode) or [redux](http://redux.js.org/docs/introduction/) or [relay](https://facebook.github.io/relay/) you don't come across this situation.

#### Component with No State  and No Props


```scala
@ScalaJSDefined
class HelloMessage extends ReactComponentNoPS {
  
  def render() = {
   Text(s"Hello Scala.js")
  }
  
}

object HelloMessage {
 def apply() = CreateElementNoProps[HelloMessage]()
}

//usage 
HelloMessage()

```

#### Component with Children

If any of your components have children just use `WithChildren` variants of `CreateElement`

```scala
@ScalaJSDefined
class HelloMessage extends ReactComponentP[HelloMessage.Props] {
  def render() = {
   Text(s"Hello ${props.name}")
  }
}

object HelloMessage { 
  case class Props(name : String)
  def apply(props:Props)(children:ReactNode*) = CreateElementWithChildren[HelloMessage](props,children= children.toJSArray)
}

//usage 

HelloMessage(HelloMessage.Props("Scala.js"))("child1","child2")
```


```scala
@ScalaJSDefined
class HelloMessage extends ReactComponentS[HelloMessage.State] {

  inititalState(State("initial"))
  
  def render() = {
   Text(s"Hello ${state.name}")
  }
  
}

object HelloMessage {
 case class State(name:String)
 def apply(key:String)(children:ReactNode*) = CreateElementNoPropsWithChildren[HelloMessage](key = key,children = children.toJSArray)
}

//usage

HelloMessage(key="1")("child1","child2")

```


***Note:*** all ReactComponent`..` are pure components,meaning they will be updated only when reference of `Props/State` changed,If your component has children make sure that all children also pure components. for some reason if you want to use `mutable` classes as props/state then use not pure counter parts.

`ReactComponentP` -> `ReactComponentNotPureP`

`ReactComponentS` -> `ReactComponentNotPureS`

`ReactComponent` -> `ReactComponentNotPure`

`Hint: @ScalaJSDefined annotation  is not needed in scala.js 1.0 :) `
