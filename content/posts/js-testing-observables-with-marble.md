---
title: "Testing Observables With Marble"
date: 2018-02-22T11:34:47+01:00
---

## Marble diagrams
Marble diagrams are a way to visually represent Observables. The marbles represent a value being emitted, the passage of time is represented from left to right, a vertical line represents the completion of an Observable, and an X represents an error.

- `-` represents the passage of 10 frames of time.
- `|` represents the completion of an Observable.
- `^` represents the subscription point of an Observable (only valid for hot Observables).
- `#` represents an error. The value of the error can be provided to errors argument.
- `"()"` sync groupings: When multiple events need to single in the same frame synchronously, parenthesis are used to group those events. You can group nexted values, a completion or an error in this manner. The position of the initial ( determines the time at which its values are emitted.
- `"^"` subscription point: (hot observables only) shows the point at which the tested observables will be subscribed to the hot observable. This is the "zero frame" for that observable, every frame before the `^` will be negative. You can only use this character **once** in a diagram.
- `"!"` unsubscription point: shows the point in time at which a subscription is unsubscribed. You can only use this character **once** in a diagram.
- any other character represents a value emitted. The actual value can be represented in the  values argument, where the character is the key.

## Methods
- `hot(marbles: string, values?: object, error?: any)` - creates a "hot" observable (a subject) that will behave as though it's already "running" when the test begins. An interesting difference is that hot marbles allow a `^` character to signal where the "zero frame" is. That is the point at which the subscription to observables being tested begins.
- `cold(marbles: string, values?: object, error?: any)` - creates a "cold" observable whose subscription starts when the test begins.
- `expectObservable(actual: Observable<T>).toBe(marbles: string, values?: object, error?: any)` - schedules an assertion for when the TestScheduler flushes. The TestScheduler will automatically flush at the end of your jasmine `it` block.
- `expectSubscriptions(actualSubscriptionLogs: SubscriptionLog[]).toBe(subscriptionMarbles: string)` - like `expectObservable` schedules an assertion for when the testScheduler flushes. Both `cold()` and `hot()` return an observable with a property subscriptions of type `SubscriptionLog[]`. Give `subscriptions` as parameter to `expectSubscriptions` to assert whether it matches the subscriptionsMarbles marble diagram given in `toBe()`.

## Examples

 - `hot('--a--b')` will emit `"a"` and `"b"`
 - `hot('--a--b', { a: 1, b: 2 })` will emit `1` and `2`
 - `hot('---#')` will emit error `"error"`
 - `hot('---#', null, new SpecialError('test'))` will emit `new SpecialError('test')`
 - `hot('-')` equivalent to Observable.never(), or an observable that never emits or completes
 - `hot('-a-^-b--|')` on frame -20 emit a, then on frame 20 emit b, and on frame 50, complete.
 - `hot('-----(a|)')` on frame 50, emit a and complete.

## Basic test
```js
var e1 = hot('----a--^--b-------c--|');
var e2 = hot(  '---d-^--e---------f-----|');
var expected =      '---(be)----c-f-----|';

expectObservable(e1.merge(e2)).toBe(expected);
```

- The `^` characters of hot observables should **always** be aligned.
- The **first character** of cold observables or expected observables should **always** be aligned with each other, and with the ^ of hot observables.
- Use default emission values when you can. Specify `values` when you have to.



### Sources
- [Marble testing](https://alligator.io/rxjs/marble-testing/)
- [Marble tests](https://github.com/ReactiveX/rxjs/blob/master/doc/writing-marble-tests.md)
