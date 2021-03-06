---
layout: reference11
title_md: Type declarations
tab: documentation
unique_id: docspage
author: Tom Bentley
---

# #{page.title_md}

This page is about *type declarations*, which define new *type constructors*. 
There's a separate page about the [types](../type/) that 
are produced by applying type arguments to such type constructors.

## Usage 

Example *type declarations*:

<!-- try: -->
    class MyClass(name) {
        shared String name;
    }
    class MyGenericClass<Arg>(Arg arg) {
    }
    interface MyInterface {
        shared formal String name;
    }
    interface MyInterface<Arg> {
        shared formal Arg arg;
    }
    object myObject {
        shared String name = "Trompon";
    }

## Description

### Types versus Type declarations

It is important the appreciate the difference between a 
*type declaration* (such as the declaration of a class) 
which introduces a *type constructor*, and an
*applied type* (also called a *produced type*).

There's a separate reference page about [types](../type/)

### Type declarations

In Ceylon, a *type declaration* is one of:

* A [`class` declaration](../class)
* An [`interface` declaration](../interface)
* An [`object` declaration](../object)

All these declarations can have [members](#member_declarations).

[Function](../function/) and [value](../value/) declarations to not 
introduce new types or type constructors and do not have members.

### Declarative subtyping

The [`extends`](../class/#extending_classes) and 
`satisfies` clauses of a type declaration
list the types that are treated as supertypes of the 
types produced from the type declaration.

Put another way, these clauses have the effect that 
every type produced from the type constructor
of the class or interface being defined will be a subtype of the type
in `extends` or `satisfies` clause:

    class Sub() extends Generic<String>() {
    }
    class GenericSub<Parameter>() extends Generic<Parameter>() {
    }
    
Thus the type `Sub` is a subtype of `Generic<String>`, and the
type `GenericSub<Boolean>` is a subtype of `Generic<Boolean>`.

### Declarative cases

The `of` clause of a type declaration enumerates the 
disjoint subtypes (the *cases*) of the 
types produced from the type declaration.

### Different kinds of declaration

#### Member declarations

A declaration that occurs directly in a type declaration is called a 
*member declaration*. Member [values](../value/) are called 
*attributes*. Member [functions](../function/) are called *methods*.
Classes and interfaces that are members do not have an 
alternative name they're just called member classes and interfaces.

#### Local declarations

A *local* (or *nested*) declaration is a declaration that is 
contained within another declaration or a statement.

#### Top-level declarations

A *top level* declaration is contained directly in a
[compilation unit](../compilation-unit) and not contained within any other
declaration. In other words a top level declaration is neither
a [member](#member_declarations) nor a [local](#local_declarations) declaration.

### Enumerated types

Classes can [enumerate](../class#enumerated_classes) 
a list of their permitted subclasses. 

Interfaces can [enumerate](../interface#enumerated_subtypes) 
a list of their permitted subtypes. 

The subtypes are called the *cases* of the class or interface type.

### Type aliases

To avoid having to repeat long type expressions, you can declare a 
[type alias](../alias#type_alises) for a type using the `alias` 
keyword:

<!-- try: -->
    alias BasicType = String|Character|Integer|Float|Boolean;

### Selected important type declarations

#### `Anything`

[`Anything`](#{site.urls.apidoc_1_1}/Anything.type.html) 
the ultimate supertype of all types. That means that every 
type has `Anything` as a supertype, which in turn means that 
every reference is assignable to `Anything`.

You can also think of `Anything` as the union of *all* types.

`Anything` corresponds to the notion of universe set in mathematics.

You can't do anything with an instance of `Anything`, except 
narrow it to some more specific type.

`Anything`'s cases are `Null` and `Object`.

`Anything` is the default upper bound for type parameters 
lacking an upper bound constraint.

#### `Nothing`

[`Nothing`](#{site.urls.apidoc_1_1}/Nothing.type.html) 
is a subtype of all types. That means that every type has 
`Nothing` as a subtype (even if the type is produced 
from a `final` class declaration).

You can also thing of `Nothing` as the intersection of *all* types. 

`Nothing` corresponds to the notion of the empty set in mathematics.

Because `Nothing` is the intersection of all types it is assignable to 
all types. Similarly because it is the intersection of all types it can 
have no instances. 

There is a value called `nothing` in the language module, which 
has the type `Nothing`. At runtime trying to evaluate 
`nothing` (that is, get an instance of `Nothing`) will 
throw an exception.

Any function or value which claims to return `Nothing` cannot return normally, it
must either:

* throw an exception or
* not return (for example, by looping forever, or stopping the virtual machine)

#### `Null`

[`Null`](#{site.urls.apidoc_1_1}/Null.type.html) is the type of 
[`null`](#{site.urls.apidoc_1_1}/index.html#null). 

Conceptually `null` is the *absence* of a value. 

If an expression permits `null` then it
needs `Null` as a supertype. This is usually expressed as using a 
[union type](#union_types) such as `T|Null`, which can be [abbreviated](../type-abbreviation)
as `T?`, and we may refer to it as an *optional type*.

Because `null` represents the absence of a value (something that is not a thing), 
it is meaningless to ask some reference is equal to `null`. Thus Ceylon does not 
permit `obj == null` or `null == null`. In practice there are some situations 
where you want the answer to be true and other situations where you want the 
answer to be false.

`Null` is one of the cases of `Anything`, the other being `Object`.

#### `Object`

[`Object`](#{site.urls.apidoc_1_1}/Object.type.html) 
is the class that declares `equals()`, `hash` and `string`. 

`Object` is one of the cases of `Anything`, the other being `Null`.

#### `Basic`

[`Basic`](#{site.urls.apidoc_1_1}/Basic.type.html) 
mixes `Identifiable` into `Object`.

`Basic` is the default superclass of classes whose declaration lacks 
an `extends` clause.

#### `Iterable`

[`Iterable`](#{site.urls.apidoc_1_1}/Iterable.type.html) 
is a type that produces instances of another type when iterated. 

There are two flavours of `Iterable`:

* the type `Iterable<T>`, usually [abbreviated](../type-abbreviation)  to `{T*}`,
  may contain zero or more elements (it is *possibly empty*),
* the type `Iterable<T,Nothing>`, usually [abbreviated](../type-abbreviation)  to `{T+}`,
  contains at least one element (it is *non-empty*)

#### `Sequential`

[`Sequential`](#{site.urls.apidoc_1_1}/Sequential.type.html) 
is an enumerated type with subtypes 
[`Sequence`](#{site.urls.apidoc_1_1}/Sequence.type.html) and 
[`Empty`](#{site.urls.apidoc_1_1}/Empty.type.html). 
`Sequential<T>` is usually [abbreviated](../type-abbreviation) 
to `T[]` or `[T*]`.

#### `Empty`

[`Empty`](#{site.urls.apidoc_1_1}/Empty.type.html) is the type 
of a [`Sequential`](#{site.urls.apidoc_1_1}/Sequential.type.html) 
which contains no elements. 

The expression `[]` (or alternatively the value `empty`) 
in the language module has the type `Empty`.

#### `Sequence`

[`Sequence`](#{site.urls.apidoc_1_1}/Sequence.type.html) is the 
type of non-empty sequences.
`Sequence<T>` is usually [abbreviated](../type-abbreviation) to `[T+]`.

#### `Tuple`

[`Tuple`](#{site.urls.apidoc_1_1}/Tuple.type.html) is a subclass 
of `Sequence` (and thus cannot be empty). It differs from `Sequence` 
in that it encodes the types of each of its elements individually.

<!-- try: -->
    [Integer, Boolean, String] t = [1, true, ""];
    Integer first = t[0];
    Boolean second = t[1];
    String last = t[2];

Tuples also have a notion of *variadicity*:

<!-- try: -->
    // A tuple of at least two elements
    // the first is an Integer and 
    // the rest are Boolean
    [Integer, Boolean+] t = [1, true, false];
    // A tuple of at least element
    // the first is an Integer and 
    // the rest are Boolean
    [Integer, Boolean*] t2 = t;

`Tuple` types may thus be used to represent the type of an argument or
parameter list, and are therefore used to encode function types.

Unabbreviated tuple types are extremely verbose, and therefore the 
[abbreviated](../type-abbreviation) form is always used. 

### Metamodel

There are two ways of modelling declarations at runtime mirroring 
the difference between a declaration and its type.

The package `ceylon.language.meta.declaration` contains interfaces
which model *declarations*. As such, those declarations have
type parameter lists. Supplying 
a type argument list for a declaration you can obtain an 
instance of a *model* from `ceylon.language.meta.model`.

## See also

* [types](../type) are produced from type declarations by applying
  type arguments
* Top level types are declared in [compilation units](../compilation-unit)
* [`class` declaration](../class)
* [`interface` declaration](../interface)
* [`object` declaration](../object)
* [method declaration](../function)
* [attribute declaration](../value)
* [type abbreviations](../type-abbreviation)
* [type parameters](../type-parameters)
