
  [tt]: http://learnyouahaskell.com/types-and-typeclasses
  [moott]: http://learnyouahaskell.com/making-our-own-types-and-typeclasses
  [z7]: $scalazTree$
  [z7docs]: $docBaseUrl$

### sbt

以下が Scalaz $scalazVersion$ を試すための build.sbt だ:

```scala
scalaVersion := "$scalaVersion$"

val scalazVersion = "$scalazVersion$"

libraryDependencies ++= Seq(
  "org.scalaz" %% "scalaz-core" % scalazVersion,
  "org.scalaz" %% "scalaz-effect" % scalazVersion,
  "org.scalaz" %% "scalaz-typelevel" % scalazVersion,
  "org.scalaz" %% "scalaz-scalacheck-binding" % scalazVersion % "test"
)

scalacOptions += "-feature"

initialCommands in console := "import scalaz._, Scalaz._"
```

あとは sbt 0.12.3 から REPL を開くだけだ:

```scala
\$ sbt console
...
[info] downloading http://repo1.maven.org/maven2/org/scalaz/scalaz-core_2.10/7.0.5/scalaz-core_2.10-7.0.5.jar ...
import scalaz._
import Scalaz._
Welcome to Scala version 2.10.3 (Java HotSpot(TM) 64-Bit Server VM, Java 1.6.0_51).
Type in expressions to have them evaluated.
Type :help for more information.

scala>
```

Scalaz $scalazVersion$ から生成された [API ドキュメント][z7docs]もある。
