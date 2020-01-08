# Category Theory (and Functional Programming) by Example

## A Bag of Patterns

As code writers we have learned that there are patterns to solve common problems. We call them with kind of fancy names: _Design Patterns_, _Enterprise Patterns_, _Architecture Patterns_ and so on. We know a bunch of them, starting on the simple ones as the _Singleton_ and passing through the _Model-View-Controller_ or _Dependency Injection_; and we collect them in hardcover books.

Regardless of what patterns we choose, the idea is pretty simple: many problems have similar solutions and some solutions have been proven better than others. Actually, common algorithms as _BFS_ for graphs and trees, _binary search_ for ordered collections and so on are also patterns, although they are probably less glamorous as buzzwords.

There is another bag of patterns: Maths. Particularly, that area of Maths that some people consider the Math of Maths: Category Theory. But before we continue, let’s talk a bit more about patterns in general.

Patterns are context dependent, that is, some of them are pretty clever and useful in one language or problem domain while trivial or irrelevant in others, for example, the _Command_ pattern from the famous Design Patterns book only makes sense if you are in a language were anonymous functions don’t exist, using a specialized object to represent a closure in Scala, Haskell or JS is not even pedantic, it’s silly.

Some patterns can also be overwhelmed by the reality of problems, the _Facade_ pattern, for example, is not telling us too much in the world of proxies and APIs where everything is a _Facade_.

In order to use a pattern we need: the bag of patterns at hand, a problem, and a way to recognize the patterns that match our problem. The last part is the most difficult and the only way I know to learn it is getting experience. For the first part things are easier, these bas come in the shape of books and are easy to acquire. 

Category theory patterns are like any other bag of patterns, some of them cannot be applicable in our problems: We don’t deal with infinite categories in our programs; or they become almost trivial: _Terminal_ and _Initial_ objects become `Empty` and `unit` in Haskell, for example. But many others are actually quite interesting, as _Monoids_ and _Functors_.

This text attempts to provide a short overview of examples of categories and categorical patterns hoping that having this bag at hand could be useful.

Sets and Function Like Arrows
Set-like examples are very similar, objects are pretty much the Types in a programming language and arrows from A to B are a process that produces values of type B when given values of type A.
Types (or Hask)
Objects: Types
Arrows: Functions
Composition: Function composition (`.` in Haskell, `|>` in PS, `.after` in Scala)
Initial Object: Empty (in the languages that support it like Haskell)
Terminal Object: unit or `()` (or any other distinguished singleton)
Hom(A, B): The type: a -> b (Function[A, B])
Functor Hom(_, B): Given a known type `b` the parameterized type: h a = a -> b
Product: Given types A and B, the tuple type (A, B); or any other isomorphic representation as {a: A, b: B}.
Coproduct: Given types A and B, the Either type Either A B; or any other isomorphic representation.
Composite Types
Objects: Types
Arrows: Relations (There is an arrow from A to B if the type A has a property of type B or a collection of B’s)
Composition: A type A has a property of type B, and type B has a property of type C
Initial Object: Empty type (i.e. every object of type Empty has a property of type A for any A, of course, if you can ever find an Empty value; be aware that undefined in JS is actually like void or unit). It’s not very useful.
Terminal Object:
Hom(A, B): No explicit notation (probably `a.p` for an object `a` of type A having a property `p` of type B)
Functor Hom(_, B): No explicit notation (in languages with structured typing as TS, the type: `{p: b}` gets close to the idea, but forces the property to be named `p`)
Product: Same as in Types.
Coproduct: Same as in Types.
DataBases
Objects: Table schemas (Table type)
Arrows: Relations (There is an arrow between a row “a” in table A and a row “b” in table B if there is a “join” like relation where “a” and “b” are joined together).
Composition: Joining three tables: A with B and then B with C
Initial Object: This may be a Table with no columns, which as in the other cases, it’s pretty useless.
Terminal Object:
Hom(A, B): No explicit notation
Functor Hom(_, B): No explicit notation
Product: JOIN of two tables.
Coproduct: UNION of two tables (with null or default values for columns other than the schema itself)
Kleisli Categories (over a monad M)
Objects: Types
Arrows: Kleisli Arrows (a -> M b)
Composition: Monadic composition (>>=, flatMap or bind)
Initial Object:
Terminal Object:
Hom(A, B): The type: a -> M b
Functor Hom(_, B): Given a known type `b` the parameterized type: h a = a -> M b
Product: Same as in Types.
Coproduct: Same as in Types.
General Programs (profunctors)
Objects: Types
Arrows: Programs that consume values of type A and generate values of type B with possible side effects. It is important to note that programs may not need to actually produce any B at all or that they may produce many regardless of how many or when they are given the values of type A.
Composition: Piping the results of a program as inputs of another program
Initial Object:
Terminal Object:
Hom(A, B): No explicit notation, the collection of all programs consuming A’s and producing B’s
Functor Hom(_, B): No explicit notation
Orders and Lattices
Most of these examples are similar, their main characteristic is that arrows represent some kind of dependency between objects and such dependency is unique. That is, between two objects A and B, if there is an arrow, there is only one.

The fact that there is at most one arrow between two objects makes it common for this categories to not have an explicit notation to denote the set of morphisms between two objects. Instead, the relation is usually taken as a predicate, if aRb (A < B) then the Hom(A, B) is not empty, if !aRb then the Hom set is empty. Considering this, you can think on the Hom functors Hom(_, B), Hom(A, _) and Hom(_, _) as predicates: \x -> x < b; \x -> a < x; and \x y -> x < y.
Types again (Lattice)
Objects: Types
Arrows: Subtyping (There is an arrow from A to B if a value of type A can be seen as a value of type B, that is, A is a subtype of B, A extends B or B is a superclass of A. We are assuming unique subtype relationships as it is the case on a given scope)
Composition: Subtyping chain
Initial Object: any or Object
Terminal Object: null
Product: least common type ancestor, at most it should be any. If the type system allows multiple inheritance/implementations, the least common common ancestor may be a product type: A1 & A2 &...
Build Systems (Semilattice)
Objects: Artifacts
Arrows: Dependency (There is an arrow from A to B if building B depends on first building A)
Composition: Dependency chain
Initial Object: None
Terminal Object: final artifact (if there is only one, which we can safely assume as a restriction)
Product: Given two artifacts A and B, the least common ancestor in the chain. It may not always exist, if there are two common ancestors C and D that don’t share ancestors themselves.
Spreadsheets (Poset)
Objects: Cells
Arrows: Dependency (There is an arrow from A to B if you need A to calculate B)
Composition: Dependency chain
Initial Object: None
Terminal Object: None
Product: Given two cells A and B, the least common dependency cell. It may not always exist.
Dependency Injection (Poset)
Objects: Instances
Arrows: Dependency (There is an arrow from A to B if the value of A is needed to build the value of B)
Composition: Dependency chain
Initial Object: None
Terminal Object: Run time module (or none, if you don’t want to consider the whole system as a node)
Product: Given two instances A and B, the least common dependency. It may not always exist.

Deduction Systems (Semilattice)
Objects: Formulas
Arrows: Implication (There is an arrow from A to B if A implies B, actually, there are many forms of implications and an arrow here may be represented by a family of processes)
Composition: Implication chain
Initial Object: None
Terminal Object: Final proof (if there where many, the conjunction of all of them will be the terminal object)
Product: Given two formulas A and B, the least common dependency. It may not always exist.

