# Introduction to category theory

In this talk, I will be giving an introduction to the subject of category theory. In the first place, I want to point out that
I will be assuming not only programming, but also mathematical background. I will also be showing some
code examples in Haskell, but it's not a problem if you haven't seen any Haskell code before, because we will be
aiming at understanding the concepts and not the syntax.

At first, a disclaimer: you're not going to learn category theory after a ten minute talk. This is a theory
that's been developed for quite some time, but we'll be looking at the fundamentals.

Category theory is one of the most abstract, and if not, the most abstract things in mathematics, so the question is
what does that have to do with computer science and programming after all? The answer is: abstraction. 

## Abstraction in programming

Typically, when you start writing code, you start with the Assembly language, which is the lowest level of abstraction a computer
can understand. You're telling the computer to move data from register to register, jump from address to address, etc.
This is a very imperative style of programming. An analogy to that style of programming is the Turing machine. Although
it's possible to write programs in that style, they don't really scale. 

Another approach to programming, which people moved to, is procedural programming, where you split your program into 
procedures, so each problem is solved in a separate procedure, and then the result is combined.

Next, people came with another approach called object-oriented programming, which is even more abstract: now you have
stuff that you hide inside objects. Once you've programmed an object, you don't have to look inside it anymore, you
only access it through the surface, which is the interface.

For quite some time, people thought that object-oriented programming is the solution to everything, it's the beast in data
hiding. However, problems began to emerge as people started writing multi-threaded, concurrent, parallel code. And the problem
is that since objects hide implementation, they hide exactly the wrong thing, and that makes them not composable. They hide two
things that are very important:
- mutation (they mutate some side inside, and we don't know that)
- sharing of data (they share data between themselves)

and mixing and sharing data has a name, it's called a data race. In other words, in object-oriented programming, objects
are abstracting over data races, and when you abstract over data races, you get data races for free when you start composing
objects.

So the next level of abstraction that people reached out to is functional programming, where you abstract things into
functions, because with functions you don't have mutations, so you don't have the problem of data races, but you
also have the ability to compose solutions of subproblems into solutions of a big problem.

Finally, category theory is that final level of abstraction, that language on top of Haskell, ML and other functional
languages. It's not a practical programming language, but it's a language.

Example: people taking ideas from Haskell, translating them to template metaprogramming in C++.

## Towards a definition of a category

Generally, we conclude that the pattern that emerges is the following: we're taking a problem,
and we're splitting it into subproblems, solving them separately, and combining the solutions. This is called composition.
We're also trying to abstract stuff, meaning that we're trying to get away from the details of how stuff is implemented.
This is called abstraction. Add composition, abstraction and identity, and you get the main ideas behind a category.
Note that, once we abstract stuff, things that used to be different become identical.

Example: billiard balls of the same color are different under the microscope (e.g. they have scratches), but
you can replace them when playing and it's fine

All in all, a category is defined by two of these things: composition and identity.

## Definition of a category

You can think of a category as a bunch of objects. Note that we're not saying a set of objects, because that limits
the scope of the definition. When talking about category theory, we're losing generality, because set theory has
paradoxes, e.g. Russell's paradox. A category also consists of a bunch of arrows between these objects, which
are called morphisms. Both objects and morphisms are primitives for this theory, they have no properties, no internal
structure. We define a category by defining the objects for the category, and for each pair of objects, we define the
arrows between them. A good analogy about a category is a graph, but you have to be very open-minded about what
a graph is. It can have infinitely many nodes, infinitely many arrows between nodes, or it could have zero, etc.

### Composition

If you have an arrow from a to b called f, and an arrow from b to c called g, then there must exist an arrow from a to c,
which we call g after f (g . f). It's important to understand that there might be multiple arrows from a to c, but
exactly one is a composition. As said, when we're defining a category, we're giving the objects and arrows, and then
defining composition: for every two arrows, you have to define the composition. It's sort of multiplication table
for arrows, the whole information about the category is in this multiplication table. This table encodes the structure
of the category

### Identity

For every object a, there's an identity arrow. Identity plays the role of a "one" when defining composition.

### Associativity

The associativity law works for compositions as well, meaning that h . (g . f) is the same thing as (h . g) . f

### An example from programming

We'll be looking at a category which is very
close to us programmers. This is the category in which objects are types and arrows are functions. Any single argument function takes an argument of type A and returns a result of a type B, so in essence, a function is an arrow between two types. For instance, in Haskell and ML this is almost the category we're using day to day.

You might ask what are types exactly, and a good answer would be that types are actually sets of values (think numeric type ranges, for instance), so we can model programming as a category of sets, so instead of types we'll be saying sets of values, and morphisms would be functions between sets, just like in mathematics where functions are defined between sets. 

The difference though is that before building a category on top of sets, we know the structure of the sets, we know they consist of elements. After we define a category with objects that are sets, we have to forget about the structure of the sets, so we have no idea what's inside the set: it's an atom, a primitive.

To sum up, that gives you a new way of looking at things, an abstract way of looking at things. Category theory gives you this language that you don't have to (for example) look inside the structure of these sets, but you can reason about them only using the morphisms. If you think about it, this is the ultimate in data hiding, the end of the road of abstraction: you have an object, it's a set, but you cannot look inside of it. All you have is it's interface, which basically is all these arrows that connect to it.

## Functions

Typically, the topic of functions is interesting not only in programming context, but also in mathematical. However, we're going to look at it from a categorical point of view.

### A note on total pure functions

By the way, what we're really interested in is total pure functions. Most of the problems that arise in programming happen because people use partial functions, e.g. a function is not defined for some of its arguments, and this typically leads to some code that throws exceptions and so on. Just as a side note, a funny definition for
a pure function is a function that can be memoized, meaning you can save it into a lookup table of some sort.

### Directionality

As you know from the discrete structures course, in set theory, functions are defined as relations, they're just a set of pairs. You already know what a domain and an image of a function is mathematically, so there's no point in explaining that. However, there's one fundamental concern about functions in category theory, and it's about function directionality. Directionality is a vital intuition to have about functions, and this intuition comes in handy afterwards when dealing with functors, natural transformations and so on, which are really fundamental concepts from category theory.

### Isomorphisms
In general, the simplest way to understand whether there's directionality is to ask whether the function is invertible or not.

f :: a -> b
g :: b -> a

f is invertible <=> g . f = id and f . g = id

A function that's invertible is called isomorphism. Note that this is basically using category theory notation, so we're not talking about elements of sets. Therefore, this is a definition of isomorphism not only in the set category, but in any category, since it's expressed by only composition and identity.

Isomorphisms are great, because they identify sets. In other words, they provide for set identification => for finite sets, an isomorphism is an identification of elements (one-to-one correspondence).

### Monomorphisms and epimorphisms

In set theory, we have the concepts of injective and surjective functions. These ideas can also be translated into categorical terms, and it turns out we can express these ideas in terms of morphisms as well. Nevertheless, we're not going to do that, as we don't have the time. The point here is that abstracting these two concepts lets you use them without the knowledge of the internal structure. Fianlly, in category theory, the terms for injections and surjections are monomorphisms and epimorphisms.

## Functors

The functions paragraph was more of a revision that was intended as a preparation for the introduction of a concept that is strictly categorical, which is called a functor.

Now, the ones of you who have had exposure to
functional programming outside of the university
might be familiar with the concept, but it's vital to understand that functors are a mathematical concept and not a design pattern of some sort.

One specific use case of functors is to formalize pattern matching. Imagine a category. You want to recognize a pattern inside that category. A pattern must be some kind of structure, which means that we're going to model the pattern with a category, because as said already, categories define structure. In order to map that model into the actual category, we'll be making use of functors.

### Definition

To take that intuition one step ahead, mathematically speaking, a functor is a map from one category to another category.

As a side note, it's crucial to realise that in mathematics, functions are somewhat primitive and trivial in the sense that they don't preserve structure (since they work on sets and sets have no structure). What we're really interested in mathematics and in the real world in general is mappings that preserve structure. And functors are exactly an example of such mappings.

The main takeaway is that functors are not just maps, but structure-preserving maps.

### Discrete categories

An interesting question that emerges is whether we have a notion for representing something unstructured, e.g. a set? And the answer is: yes, we do. It's called a discrete category.

### Mapping the arrows

Now that we have the definition of a functor, we conclude that it maps the arrows. So if you have any two objects a and b, and a functor called F, all the functor does is to map all of the arrows between a and b
into arrows from Fa to Fb. These arrows from a to b are obviously a set, and this set has a special name, called a hom-set. Same goes for arrows between Fa and Fb. If we call the first set C(a, b) and the second one D(Fa, Fb), the functor is actually the function (mathematical function, between sets) that maps from C(a, b) to D(Fa, Fb), so C(a, b) -> D(Fa, Fb).

In other words, a functor has one big function that maps objects, and then a function we define for every home-set.

### Mapping the arrows while preserving structure

But we still haven't mentioned how exactly preserving structure is implemented. Well, so far we said that a category defines structure, but a category is defined by composition and identity. Therefore, all we need to do is to ensure that the functor is mapping those compositions, as well as identities.

### Connection of functors to programming

Most of the functors in programming deal with a single category: the category of types and functions. In principle, objects in one category can be mapped into objects in the same category, and morphisms in one category can be mapped into morphisms in the same category. This type of functor is called an endofunctor. In Haskell, for instance, endofunctors are simply called functors.

Just applying a literal translation from the mathematical definition to our single category of types and functions, we conclude that a functor has to be a mapping of types (a total mapping!), which means it's a type constructor in Haskell (or generic type, parametric type etc. in other languages) and a mapping of functions. Of course, simply writing a functor doesn't mean that it's a functor, because we have to prove that it satisfies composability and identity. Often times, we achieve this with equational reasoning (reasoning about your code as if it was an algebraic expression).

### A useful intuition about functors

On the one hand, we can say that a functor is a type constructor which supports fmap. On the other hand, functors typically have some value they are holding, as with the Maybe example, where we're potentially holding some value of type a. Therefore, a useful intuition about functors is to think of them as containers of some value, where this very container could be empty.

A better example than Maybe, though, is defining a list as a coproduct. Applying the function to the contained value means applying the function to the head, and recursively applying it to the tail (code on the presentation). In general, applying a function can be thought of as opening the container, and applying the function as long as there's something in the container.

## Kleisli category

In order to introduce another big concept in category theory and functional programming, which is the monad, we'll be looking at a special type of category: the Kleisli category.

This is a category which is very close to us programmers: it's based on that category of types and functions which we mentioned, but it's not the same. And we will get to this category by solving a real world problem.

### Kleisli category problem statement

Imagine that we have a library of functions that we wrote. And one day, our manager comes and says there's a new requirement, and this requirement says that every function should leave an audit trail: whatever function you call should create a little log that has to be appended to some global log. Finally, we should rewrite the library so that every function leaves a trail.

### First possible solution

The first solution that comes to the mind of an imperative programmer is to have a global log (code on the presentation). But what's wrong with such a solution? In the first place, we have a hidden dependency on that log variable, that's sort of a long-distance dependency. Secondly, there's no mention that this function writes to a global log: what happens if someone accidentally deletes this log variable? And at last, what happens if we have to use our library in a multithreaded environment?

### A little improvement on the first solution

Another possible solution is to have all of the library functions return pairs of their actual type and a string, which is the log, but they can also take a log (code on the presentation). That way, the functions themselves will append to the log that's passed to them, and return that new string to the caller.

But this solution is still not perfect, it breaks the separation of concerns principle: why do these function know about appending strings?

### Reaching the final solution

Generally, we're not satisfied with passing logs as parameters and doing the appending in the function body, but we're fine with returning the result paired with what the function should log. But then, one may ask, how do we concatenate the logs? Who does the concatenation? And the answer is, since we're going to compose these functions either way, we're going to modify composition so that it does the normal composition, but also takes care of the log appending. In essence, we're defining our own way of composing functions (code on the slides). Keep that in mind, because it's a very powerful idea which has a specific name. And all of this makes a lot of sense, because if you think about it, appending strings is really part of the composition process.

Also, we have just defined a new way of composing things. Remember that a category is all about composition and identity. Do we have a category? The regular composition we're doing is associative, that's for sure. And fortunately, string concatenation is also associative, therefore concatenation of logs is also associative. We conclude that we have associativity satisfied for the new composition we defined. What about identity? Well, we can simply define a function that returns a pair, without doing any changes where the log is an empty string, and we're done! We've built a category! And this type of category has a name, it's called a Kleisli category.

The objects we defined are the types, the arrows however, are not abstractions for the regular functions between the types, but are abstractions for a sort of special functions, e.g. instead of a -> b, you have a -> pair<b, string>. The arrows that abstract these special functions, are called Kleisli arrows.

Redefining the way to compose these special functions is the key to understanding what a monad is.

### A note about string concatenation

Note that we said string concatenation is associative and it has identity defined (or also called a unit). That's a good example for another concept in category theory, which is the monoid. In other words, our custom composition could still define a category, as long as we're supplying a monoid for the logs. In our concrete example, we're using strings because they satisfy the monoid properties, but it's not a strict requirement.

## Monads

After we've built a good intuition about Kleisli categories, we'll be reaching to the final destination: introducing one of the most misleading concepts in programming – monads.

### The two categories construction

We'll be examining a construction which is kind of weird, in which we'll be holding two categories in our head. The first one is the original category, say some random category C in which we have objects and arrows, the second one we're trying to build, based on the first one, which will be the Kleisli category.

We're viewing it the following way: the objects are directly "copied", meaning that the objects in our original category are the same as the objects in the new category we're building. However, the arrows in the new category are different.

If we look at the first category, imagine that we have something that for every object in that left category, gives us another object in the same category, for instance the mapping a -> (a, string). Let's call this mapping m a. Now, m is a mapping that maps objects to objects (types to types), so this is basically a functor, or more precisely, an endofunctor.

Now let's assume that in the left category, you have two objects, a and b. You take the mapping of b, which we said we're going to call mb. If you take the arrow from a to mb, this is basically the "implementation" of the a to b arrow in the right category which we're building.

### Caveats concerning the "categority" of the right category

I incorrectly used the term category for the Kleisli category, while explaining the above construction. We're not certain that the thing we're building on the right is actually a category, because we have to check whether the arrows inside satisfy composition and identity. In the example we did beforehand, we were lucky that such composition was possible.

And here's the big takeaway: if the Kleisli category on the right is definitely a category, then we say that the mapping m, described as a -> (a, string), is a monad.
