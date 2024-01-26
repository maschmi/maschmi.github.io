---
layout: post
title:  "The semantics of Java Optional"
date:   2023-04-12 13:14:49 +0100
categories:
  - coding
tags:
  - java
  - semantics
  - coding
  - model
---

Java `Optional` is similar to a functional Maybe type. It either contains some value or not. However, there are some caveats associated with it. Lets's have a look at Optional and think about how and when to use it. And also think about it what it signals.

## The example
Let's say you have some sort of application where users can send in reviews of movies. A review must contain a numerical score in the range of 1 to 5, a movie ID and may include a free text comment. So the users can consciously decide if the want to enter a comment or not.

## The first MovieReview data structure

```
public record MovieReview(UUID movieId, int score, String comment) {}
```

While this type can held all the information needed it does not reflect the requirement written above very accurately. Let's change that.
 
## The semantics of optional

Above we have written, the user may or may not leave a comment. To make this more clear in the code we can change the MovieReview as follows

```
public record MovieReview(UUID movieId, int score, Optional<String> comment) {}
```

Now the data structure reflects there may or may not be a review comment. But there are still some pitfalls.

## Nullability

As Java goes it does not support NullSafety like e.g. Kotlin. Every object can be null. So what a developer can do when initializing the MoviewReview record is the following:

```
new MovieReview(null, 0, null) {}
```

Now our nice `Optional` is null and we will get a NullPointerException when we try to access it. So we won nothing right? That is not entirely true. Semantically the type still reflects there may or may not be comment present. However, the other semantics are still missing. Let's change that with a short detour.

## Adding bean validation

```
public record MovieReview(
@NotNull
UUID movieId, 

@Min(1)
@Max(5)
int score, 

@NotNull
Optional<String> comment) {}
```

With this added semantics we tell a developer what we expect to be put into the record. We also allow for JSR-380 bean validation to be performed. Yes, one can still set the record components to null or other invalid values. But now we can validate and also can hope the other person reads the signature before using the data structure.

## Java Optional at borders

Usually there are multiple borders in your code. Some may be very clear cut, e.g. the web interface which produces JSON, or the border to the persistence layer. Others may not be so clear pronounced, e.g. a method call to an another part of your code.

### Serialized nullables

When a data structure with an `Optional` is serialized you may loose information as the serialized data format may not have an `Optional` type. Instead it will be serialized to `null` . But this is not a big problem. As long as your deserialization interprets a serialized `null` as `Optional.empty()` your semantics are still correct. One could even say, the contract holds. If there is no comment present the user just left none.

### Internal borders - Method arguments

Let's leave the example from above. Imagine you have a query function which wants to receive all reviews for a given movie after day X and optional before day Y.
```
public List<MovieReviews> findAllByDates(UUID movieId, LocalDate start, Optional<LocalDate> end) {
    // your implementation
}
```
Easy, right? Your `Optional` signals there may or may not be and end date set. But again a  develop can put `null` into the end field. This is the reason some static code analyzers will give you a warning for this method. It could be a better way to refactor it like this:

```
public List<MovieReviews> findAllAfter(UUID movieId, LocalDate start) {
	return findAllByDatesImpl(movieId, start, Optional.empty();
}
public List<MovieReviews> findAllBetween(UUID movieId, LocalDate start, LocalDate end) {
	return findAllByDatesImpl(movieId, start, Optional.of(end));
}

private List<MovieReviews> findAllByDatesImpl(UUID movieId, LocalDate start, Optional<LocalDate> end) {
   // your implementation
}
```
Of course, this depends on your code base and your intended customers of your API.

### Internal borders - Method return value

A good use of `Optional` is to signal there may or may not be a return value of a method. 
```
Optional<Integer> findReviewScoreForMovie(UUID movieId)
```
This signals the users of this API there may not be a review score associated with this movie. And they have to take care to do something with this. This could be as easy as serializing it to `null` when sending it out over the API or something more complex. But again, semantically we say: Expect it to be not present. And if so, this is intentional and not by some error.

## Conclusion

### When not to use optional?

`Optional` does not make your code null safe. Nor should it be used to merely signal there may be nothing caused by some quirks of code evolution. If you come into the situation where you have to deserialize something into a new version of your data structure and there may be missing data you have to decide if the domain usually needs this information or not. If the domain needs this information from now on you can think about using a specific default - have a look at the NullObject-Pattern, it may come in handy. But do not use `Optional` as this would suggest it is totally normal this value may not be set.

### When to use optional?

For me, there is basically one rule when to use Optional. You may have guessed it from the word semantically written a lot of times.

You are free to use Optional encapsulated inside you implementation. But when it shows up at any kind of border use optional only when it conveys some domain information. When it has meaning.
