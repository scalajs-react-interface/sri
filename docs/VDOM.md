#VDOM


### Tags 


```scala
import sri.vdom.tags._
import sri.vdom.tags.tagsPrefix_<._ // to use tags <.tag


//usage 
div(id ="app")(
  a(href = "")("link"),
  spanC("hello")
 )
 
// tag without props 
div()("NoProps") 
or 
divC("NoProps")
```

***Note:*** Sri core vdom  tags comes with  only few default props defined ,if you want to pass extra prop then use `extraProps` or send a PR with that prop to that tag. to be honest this is opinionated but its intentional!.This way we can reduce compile times,add more type safety, and building an app only using low level vdom tags is not recommended(for most of the aps)!,use frameworks like `react-native` ,`material-ui`,`bootstrap` or build your own high level components. How ever if you want vdom like scala-tags sri has that variant via vdom-styled.
 
 

#### VDOM-Styled 
 
 #### Dependency:
 
     "scalajs-react-interface" %%% "vdom" % "version"
     "scalajs-react-interface" %%% "vdom-styled" % "version"
 
 #### How to use:
 ```scala
 import sri.web.styledtagsPrefix_<^._ //recommended for avioding name conflicts.
   //tags begin with `<`, attributes begin with `^`
   //or
 import sri.web.styledtags._
   //you still have type safety.
 ```
 
 #### example:
 ```scala
 import sri.core._
 import sri.web.styledtagsPrefix_<^._
 
 <.divC("Only contents.")
 <.div(^.value := "some value")("Contents with value")
 <.divC(
     "Has child:",
     <.p(^.value := "v1")("I'm a child <p>")
   )
 <.a(
     ^.onClick := ((e: ReactEventI) => println("this is a callback"))
   )("Click me will log something.")
   //note := is a method, when pass complicated code in, you need give them parentheses.
 ```    
 
### Inline Styles 


```scala
import sri.web.vdom.styles.InlineStyleSheetWeb

 object styles extends InlineStyleSheetWeb {
    import dsl._
    import sri.universal.styles.units._

    val container = style(display.flex,
                          fontSize := 15.px,
                          alignItems.center,
                          background := "red")
  }

//usage 

div(style = styles.container)("Styled Div")
```