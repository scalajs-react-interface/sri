# Navigation

Sri navigation module is wrapper for [react-navigation](https://reactnavigation.org/) , make sure you go through the docs of react-navigation before using it.

#### Example : 

***Javascript***

```javascript

class MyHomeScreen extends React.Component {
  static navigationOptions = {
    title: 'Home',
  }

  render() {
    return (
      <Button
        onPress={() => this.props.navigation.navigate('Profile', {name: 'Lucy'})}
        title="Go to Lucy's profile"
      />
    );
  }
}

const ModalStack = StackNavigator({
  Home: {
    screen: MyHomeScreen,
  }
});


```

***Sri***


```scala

@ScalaJSDefined
class MyHomeScreen extends NavigationScreenComponentNoPS {
  def render() = {
    Button(onPress = () => navigation.navigate[ProfileScreen](ProfileParams(name = "Lucy")),
      title = "Go to home tab")
  }
}

object MyHomeScreen {
 
}

val ModalStack = StackNavigator(registerStackScreen[MyHomeScreen](navigationOptions = NavigationStackScreenOptions(title = "Home")))

```


#### Screen Components

`NavigationScreenComponentNoPS` -> Component with No Params and State

`NavigationScreenComponentP` -> Component with Params and No State

`NavigationScreenComponentS` -> Component with State and No Params

`NavigationScreenComponent` -> Component with State and  Params


#### NavigationAwareComponent 

In Screen components you have access to `navigation` object to navigate,.. etc,But if you want to access `navigation` object some where down the tree, you need to pass `navigation` explicitly to all the children. With `NavigationAwareComponent` you don't need to pass `navigation` object explicitly.


Example : 

```scala
@ScalaJSDefined
class MyDeepChildComp extends NavigationAwareComponentP[Props] {

  def render() = {
    ScrollView(style = GlobalStyles.navScreenContainer)(
      Button(onPress = () => navigation.navigate[HomeScreen],
             title = "Go to home tab"),
      Button(onPress = () => navigation.navigate[SettingsScreen],
             title = "Go to settings tab"),
      Button(onPress = () => navigation.goBack(null), title = "Go Back")
    )
  }

}

object MyDeepChildComp {

  case class Props()
  def apply(props: Props) = WithNavigation[MyDeepChildComp](props)
}
```

`NavigationAwareComponentNoPS` -> Component with No Props and No State

`NavigationAwareComponentP` -> Component with Props and No State

`NavigationAwareComponentS` -> Component with State and No Props

`NavigationAwareComponent` -> Component with State and  Props






