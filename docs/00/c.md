
### FoldLeft

> What we wanted was a function that generalized on `List`. ... So we want to generalize on `foldLeft` operation.

```scala
scala> object FoldLeftList {
         def foldLeft[A, B](xs: List[A], b: B, f: (B, A) => B) = xs.foldLeft(b)(f)
       }
defined module FoldLeftList

scala> def sum[A: Monoid](xs: List[A]): A = {
         val m = implicitly[Monoid[A]]
         FoldLeftList.foldLeft(xs, m.mzero, m.mappend)
       }
sum: [A](xs: List[A])(implicit evidence\$1: Monoid[A])A

scala> sum(List(1, 2, 3, 4))
res15: Int = 10

scala> sum(List("a", "b", "c"))
res16: String = abc

scala> sum(List(1, 2, 3, 4))(multiMonoid)
res17: Int = 24
```

> Now we can apply the same abstraction to pull out `FoldLeft` typeclass.

```scala
scala> :paste
// Entering paste mode (ctrl-D to finish)

trait FoldLeft[F[_]] {
  def foldLeft[A, B](xs: F[A], b: B, f: (B, A) => B): B
}
object FoldLeft {
  implicit val FoldLeftList: FoldLeft[List] = new FoldLeft[List] {
    def foldLeft[A, B](xs: List[A], b: B, f: (B, A) => B) = xs.foldLeft(b)(f)
  }
}

def sum[M[_]: FoldLeft, A: Monoid](xs: M[A]): A = {
  val m = implicitly[Monoid[A]]
  val fl = implicitly[FoldLeft[M]]
  fl.foldLeft(xs, m.mzero, m.mappend)
}

// Exiting paste mode, now interpreting.

warning: there were 2 feature warnings; re-run with -feature for details
defined trait FoldLeft
defined module FoldLeft
sum: [M[_], A](xs: M[A])(implicit evidence\$1: FoldLeft[M], implicit evidence\$2: Monoid[A])A

scala> sum(List(1, 2, 3, 4))
res20: Int = 10

scala> sum(List("a", "b", "c"))
res21: String = abc
```

Both `Int` and `List` are now pulled out of `sum`.

### Typeclasses in Scalaz

In the above example, the traits `Monoid` and `FoldLeft` correspond to Haskell's typeclass. Scalaz provides many typeclasses.

> All this is broken down into just the pieces you need. So, it's a bit like ultimate ducktyping because you define in your function definition that this is what you need and nothing more.
