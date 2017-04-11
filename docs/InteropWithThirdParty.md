# Interop With Third Party Components

If you want to use a reactjs component in your project then you must define a wrapper for js component.

## Example
 Let say we have a JS component , Name : AwesomeJSComp , props ..
 ```js
   propTypes: {
     numberOfLines: React.PropTypes.number.isRequired,
     onPress: React.PropTypes.func, // function with zero args
     suppressHighlighting: React.PropTypes.bool,
     testID: React.PropTypes.string,
   }

   ```
   
   
### Using FunctionObjectMacro   


```scala

import sri.macros.{
  exclude,
  FunctionObjectMacro,
  OptDefault => NoValue,
  OptionalParam => U
}
import sri.core.{CreateElementJSNoInline, ReactElement, ReactNode}
import scala.scalajs.js.JSConverters.genTravConvertible2JSRichGenTrav
import scala.scalajs.js
import scala.scalajs.js.annotation.{JSImport}
import sri.core.{JSComponent}

//first import the js component lib , let say if jslib available as npm package `awesome-js-comp`

@js.native
@JSImport("awesome-js-comp", JSImport.Default)
object AwesomeJSComp extends JSComponent[js.Object]


object AwesomeJS {
  @inline
  def apply(numberOfLines: Int,
            onPress: U[() => Unit] = NoValue,
            suppressHighlighting: U[Boolean] = NoValue,
            @exclude key: String | Int = null,
            @exclude ref: js.Function1[AwesomeJSComp.type, Unit] = null,
            testID: U[String] = NoValue)
    : ReactElement { type Instance = AwesomeJSComp.type } = {

    val props = FunctionObjectMacro()
    CreateElementJSNoInline[AwesomeJSComp.type](AwesomeJSComp, props, key, ref)
  }

}

//if component accepts children

object AwesomeJS {
  @inline
  def apply(numberOfLines: Int,
            onPress: U[() => Unit] = NoValue,
            suppressHighlighting: U[Boolean] = NoValue,
            @exclude key: String | Int = null,
            @exclude ref: js.Function1[AwesomeJSComp.type, Unit] = null,
            testID: U[String] = NoValue)(children: ReactNode*)
    : ReactElement { type Instance = AwesomeJSComp.type } = {

    val props = FunctionObjectMacro()
    CreateElementJSNoInline[AwesomeJSComp.type](AwesomeJSComp,
                                                props,
                                                key,
                                                ref,
                                                children.toJSArray)
  }

}
  
  //usage 
  
  AwesomeJS(numberOfLines = 4,testID = "hello");

```

***Note:*** If you're wondering why we're using `OptionalParam` instead of `js.UndefOr` check this : https://github.com/scala-js/scala-js/issues/2714

### Using ScalaJSDefined traits

```scala

import sri.core.{CreateElementJSNoInline, ReactElement, ReactNode}
import sri.macros.exclude
import scala.scalajs.js.JSConverters.genTravConvertible2JSRichGenTrav
import scala.scalajs.js
import scala.scalajs.js.{UndefOr => U,undefined}
import sri.core.{JSComponent}
import scala.scalajs.js.annotation.{JSImport, ScalaJSDefined}
import scala.scalajs.js.|

  //first import the js component lib , let say if jslib available as npm package `awesome-js-comp`

  @js.native
  @JSImport("awesome-js-comp", JSImport.Default)
  object AwesomeJSComp extends JSComponent[AwesomeJSCompProps]


  @ScalaJSDefined
  trait AwesomeJSCompProps extends js.Object {
    val numberOfLines: Int
    val onPress: U[() => Unit] = undefined
    val suppressHighlighting: U[Boolean] = undefined
    val testID: U[String] = undefined

  }

  object AwesomeJS {
    @inline
    def apply(props: AwesomeJSCompProps,
              @exclude key: String | Int = null,
              @exclude ref: js.Function1[AwesomeJSComp.type, Unit] = null)
      : ReactElement { type Instance = AwesomeJSComp.type } = {
      CreateElementJSNoInline[AwesomeJSComp.type](AwesomeJSComp,
                                                  props,
                                                  key,
                                                  ref)
    }

  }

  //if component accepts children
  object AwesomeJS {
    @inline
    def apply(props: AwesomeJSCompProps,
              @exclude key: String | Int = null,
              @exclude ref: js.Function1[AwesomeJSComp.type, Unit] = null)(
        children: ReactNode*)
      : ReactElement { type Instance = AwesomeJSComp.type } = {
      CreateElementJSNoInline[AwesomeJSComp.type](AwesomeJSComp,
                                                  props,
                                                  key,
                                                  ref,
                                                  children.toJSArray)
    }

  }
  
  
  //usage 
  
 AwesomeJS(props = new AwesomeJSCompProps {
                       override val numberOfLines: Int = 2
                       override val testID: U[String] = "hello"
                     })


```


## Real World Examples

https://github.com/scalajs-react-interface/universal/blob/master/src/main/scala/sri/universal/components/UniversalComponents.scala