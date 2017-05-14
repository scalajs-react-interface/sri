# Web Router

An extensive routing solution for web.

## History 

#### browserHistory : 

is for use in modern web browsers that support the [HTML5 history API](http://diveintohtml5.info/history.html) (see [cross-browser compatibility](http://caniuse.com/#feat=history))

***Options*** : 

`basename` (default = "") : The base URL of the app

`forceRefresh` (default = false) : Set true to force full page refreshes

`keyLength` (default = 6) : The length of location.key


Example : 

```scala
val history = HistoryFactory.browserHistory(options = BrowserHistoryOptions(..))
```


#### hashHistory : 

is for use in legacy web browsers

***Options*** : 

`basename` (default = "") : The base URL of the app

`hashType` (default = "slash") : The hash type to use 

Example : 

```scala
val history = HistoryFactory.memoryHistory(options = HashHistoryOptions(..))
```


## Routes

A route can be static(constant for entire lifecycle of the app) or dynamic(varies over the lifecycle of the app),route can optionally store a state(works only in browserHistory).In react we usually define ScreenComponent for each route in our app.

### Static Routes/URLS :

Examples : `about` , `sponsors`,..


1) Route Component with No LocationState and ComponentState 

```scala
@ScalaJSDefined
class AboutScreen extends RouterScreenComponentNoPSLS {
  
  def render() = {
    ...  
  }
}

//while registering 

registerScreen[AboutScreen]("about")

// while navigating 
navigation.navigate[AboutScreen]()
```

2) Route Component with  LocationState and No ComponentState 


```scala
@ScalaJSDefined
class AboutScreen extends RouterScreenComponentLS[AboutScreen.LocationState] {
  
  def render() = {
    val id = locationState.id
    ...  
  }
}

object AboutScreen {
   case class LocationState(id:String)
}

//while registering 

registerScreen[AboutScreen]("about")

// while navigating 
navigation.navigateLS[AboutScreen](state = AboutScreen.LocationState("123"))
```

***Note: It only works for browserHistory*** 

3) Route Component with ComponentState  and No LocationState 


```scala
@ScalaJSDefined
class AboutScreen extends RouterScreenComponentS[AboutScreen.State] {
  
  initialState(State(true))
  def render() = {
   if(state.loading) "spinner"
   else  ...  
  }
}

object AboutScreen {
   case class State(loading:Boolean)
}

//while registering 

registerScreen[AboutScreen]("about")

// while navigating 
navigation.navigate[AboutScreen]()
```


### Dynamic Routes/URLS :

Examples : `users/:id/info`, `problems/:id`

1) Route Component with No LocationState and ComponentState 

```scala
@ScalaJSDefined
class UserInfoScreen extends RouterScreenComponentP[UserInfoScreen.Params] {
  
  def render() = {
  val user  = UsersService.getUserById(params.id)
    ...  
  }
}

object UserInfoScreen {
 
 @ScalaJSDefined
 trait Params extends js.Object {
   val id:String
 }
}

//while registering 

registerDynamicScreen[UserInfoScreen]("users/:id/info")

// while navigating 
navigation.navigateP[UserInfoScreen](params = new UserInfoScreen.Params {overrid val id = "userid1234"})
```

2) Route Component with  LocationState and No ComponentState 


```scala
@ScalaJSDefined
class UserInfoScreen extends RouterScreenComponentPLS[UserInfoScreen.Params,UserInfoScreen.LocationState] {
  
  def render() = {
  val user  = UsersService.getUserById(params.id)
  val 
    ...  
  }
}

object UserInfoScreen {
 
    case class LocationState(sid:String)

 @ScalaJSDefined
 trait Params extends js.Object {
   val id:String
 }
}

//while registering 

registerDynamicScreen[UserInfoScreen]("users/:id/info")

// while navigating 
navigation.navigatePLS[UserInfoScreen](params = new UserInfoScreen.Params {overrid val id = "userid1234"},
state = UserInfoScreen.LocationState("sid"))
```

***Note: It only works for browserHistory*** 

3) Route Component with ComponentState  and No LocationState 

```scala
@ScalaJSDefined
class UserInfoScreen extends RouterScreenComponentPS[UserInfoScreen.Params,UserInfoScreen.State] {
   initialState(State(true))
  def render() = {
  val user  = UsersService.getUserById(params.id)
  if(state.loading) ..  
   else   ...  
  }
}

object UserInfoScreen {
 
 case class State(loading:Boolean)
 @ScalaJSDefined
 trait Params extends js.Object {
   val id:String
 }
}

//while registering 

registerDynamicScreen[UserInfoScreen]("users/:id/info")

// while navigating 
navigation.navigateP[UserInfoScreen](params = new UserInfoScreen.Params {overrid val id = "userid1234"})
```

## Example 

```scala

object AppRoutes extends RouterConfig {

  override val history: History = HistoryFactory.browserHistory()
 
     override val initialRoute = registerInitialScreen[HomeScreen]
     registerScreen[AboutScreen]("about")
     registerDynamicScreen[ThirdScreenWithParams]("users/:id/info")
 
     registerModule(ProblemsModuleRoutes)
 
     override val notFound: RouteNotFound = RouteNotFound(initialRoute._1)
}


object ProblemsModuleRoutes extends  RouterModuleConfig("problems") {

  registerScreen[ProblemsListScreen]("/")
  registerDynamicScreen[ProblemInfoScreen](":id/info")
}

val root = Router(AppRoutes)

ReactDOM.render(root,dom.document.getElementByID("app"))

```