---
layout: layouts/post.njk
title: 5 terrific TypeScript tips
metaDesc: Whether you are new to TypeScript and trying to learn some new tricks,
  or are an experienced TypeScript user - I do hope you will find these tips
  useful.
date: 2022-03-16T20:50:01.978Z
tags:
  - typescript
  - tips
---
Maybe you have noticed that TypeScript have had quite a rise in attention over the last couple of years. In the latest [State of JS survey from 2021](https://2021.stateofjs.com/en-US/other-tools/#javascript_flavors), 69% of the respondents say they are using TypeScript - that’s a lot! I’m not surprised why so many people like TypeScript, because I certainly do too.

Whether you are new to TypeScript and trying to learn some new tricks, or are an experienced TypeScript user - I do hope you will find these tips useful.

# Return Type Annotations

One thing that I really like about TypeScript is that you can describe what a function will return, although in many cases it’s actually not needed since TypeScript will infer the type from what is returned in the code. But I think there can be value in having it set explicitly, and in my experience this is often underrated. If you provide a return type, TypeScript will help you/yell at you if you’re returning something wrong. It’s also nice to see the return type when navigating the code in the IDE. Your future self and your team mates (or whoever is going to consume your code) will love you for it. TypeScript will even check if the value returned from the function is used correctly.

Here’s an example:

```typescript
function getFavoriteNumber(): number {
  return 26;
}
```

[Read more here](https://www.typescriptlang.org/docs/handbook/2/everyday-types.html#return-type-annotations)

# Interfaces

I love interfaces! They allow me to describe a “thing” in detail, both simple and complex “things”. You can think of an interface like a JavaScript object with key/value pairs, where the key is the property name and the value is the type.

Simple example:

```typescript
interface Dog {
    name: string;
    breed: string;
    age: number;
    color: string;
}
```

Here we have an interface describing a Dog. It has a name, a breed and a color which are all strings. It also has an age, which is a number.

The great thing about this, is that we can now use this whole interface as a type-variable. We can make a new variable and give it our Dog type. Now we have ensured that this variable can only be of this type, and TypeScript will yell at us if we mess it up - which is actually a good thing.

```typescript
const labrador: Dog = {
    age: 3,
    breed: "Labrador",
    color: "brown",
    name: "Bella"
}
```

[Try it out here](https://www.typescriptlang.org/play?#code/JYOwLgpgTgZghgYwgAgCIHsDmyDeAoZQ5EOAWwgC5kBnMKUTAbgKICMoIIATK2+kJi0JxMlYgFdSraMyLIE6ADboovOg2YBfPHgUhayRXHZwuKqhmwBeXEOQixAZgA0d9px7IARABljUUxUvVzkFZVVvdnQAdxBguxJyKi8AIQhFIy88bV10fSUIADplTAAKIxMzKABKRiA)

# Intersection Types (&)

So, interfaces are great! But what if you suddenly need all properties from two or more interfaces? Is it possible to “merge” interfaces? Yes, it is! I’m not sure if a merge is the correct term here, but that’s how I like to think about it at least.

Here’s an example:

```typescript
interface Colorful {
  color: string;
}
interface Circle {
  radius: number;
}

function draw(circle: Colorful & Circle) {
  console.log(`Color was ${circle.color}`);
  console.log(`Radius was ${circle.radius}`);
}

draw({ color: "blue", radius: 42 });
```

Example inspired from [typescriptlang.org](http://typescriptlang.org/), [read more here](https://www.typescriptlang.org/docs/handbook/2/objects.html#intersection-types).

# Union Types (|)

Union Types are great. Why? Well, let me explain. Let’s say you have a function called `notify` that takes a `Message` (your custom type) object. But suddenly you need it to also handle a simple string. This could be solved using a Union Type, which I like to think of as an OR operator, for types. So it should take `Message` OR `string`. Here’s the code:

```typescript
function notify(message: Message | string) {
	...
}
```

Now the `message` variable can be two different things, so it would be a good idea to make code-branches within the function, one for each case, so TypeScript (and you) can be sure what the actual type is in the given situation. You can do this with a `if else` structure, or using a `switch case`.

[Read more here](https://www.typescriptlang.org/docs/handbook/2/everyday-types.html#union-types).

# Partial Utility Type

I’ve been in a few situations where I needed only a subset of properties from an interface, and for this I’ve used the builtin Utility Type `Partial<Type>`. What this actually does behind the scenes is that it loops through all the properties of the provided type and makes a copy, but also adds the [optional operator](https://www.typescriptlang.org/docs/handbook/2/objects.html#optional-properties). So it basically returns a copy of the type, but with all properties optional. This can be really useful. There are many other useful [Utility Types](https://www.typescriptlang.org/docs/handbook/utility-types.html) as well, so make sure to check them out.

[Read more here](https://www.typescriptlang.org/docs/handbook/utility-types.html#partialtype).