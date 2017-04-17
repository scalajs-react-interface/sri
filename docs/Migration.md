# Migration

Upgrading from old sri(0.x.x) to new sri(2017.x.x) 


1) `ReactComponent` renamed to `Component` check out [this](DefiningComponents.md) guide.

2) all `setState` calls now accepts function only 

```scala
//old 
setState(state.copy(...))

//new
setState((state:State) => state.copy(...))
```

3) Old WebRouter is deprecated , for now copy old webrouter files to your project until some one work on https://github.com/scalajs-react-interface/incubator/issues/3


4) all primitives(dom tags and react-native basics) comes with limited set of props, More Info : https://github.com/chandu0101/sri/issues/56#issuecomment-281671656

5) removed packages core.all , mobile.all, universal.all, web.all

```scala

//old 
import sri.core.all,_

//new 

import sri.core._
```