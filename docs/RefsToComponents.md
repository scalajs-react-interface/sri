
## Refs To Components

react 0.14 supports two types of refs( string based and callback). In future string ref's are going to be obsolete

>The next big (and probably most invasive change for a while) will be replacing string refs. Most can be automatically converted. #reactjs
 Use callback refs e.g.
 this.node = n} /> to avoid having to do any upgrade work. Allows you to move work from life-cycles too

so Sri only supports callback refs.

Let's say we have a SideMenu component which has public method `hideMe()`, and we want close menu from another component.

```scala

@ScalaJSDefined
class SideMenu extends ReactComponentNoPS {
  def render() = SideMenu..
  def hideMe() = {
    println("Ok done!.")
  }
}

object SideMenu {

  def apply(ref: js.Function1[SideMenu, Unit] = null) =
    CreateElementNoProps[SideMenu](ref = ref)
}

@ScalaJSDefined
class RandomComponent extends ReactComponentNoPS {
  def render() = SideMenu(ref = storeChildRef _)
  var menuRef: SideMenu = _
  def storeChildRef(ref: SideMenu) = {
    menuRef = ref // store reference to use later
  }
  def onButtonClick() = {
    menuRef.hideMe() // invoke actions
  }
}

object RandomComponent {
  def apply() = CreateElementNoProps[Component]()
}

```