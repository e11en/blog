---
title: "Angular Ngrx State Dependent Actions"
date: 2018-02-15T10:03:25+01:00
---

**C depends on B -> B depends on A.**

* First, notice that I am setting the value of the courses property to the result of the `switchMap()` operator.
* I use the `tap()` operator to tranparently perform an action (or side effect), in this case, to `dispatch()` the LoadCoursesForUserAction action. The `tap()` operator will receive the user that is emitted from the observable that is created using the `select()` method.
* Finally, using the `switchMap()` operator, the inner observable (obtaining the user) is complete, returning the observable of the array of Course objects.

```js
public administrator = false;
public group: Observable<Group>;
public users: Observable<Array<User>>;

ngOnInit() {
    this.group = this.activatedRoute.paramMap
    .takeWhile(() => this.alive)
    .tap(params => this.store.dispatch(new LoadGroupAction({ id: params.get('id') })))
    .switchMap(() => this.store.select('userGroup'));

    this.users = this.group
    .tap(group => this.store.dispatch(new LoadUsersInGroupAction({ group: group })))
    .switchMap(() => this.store.select('users'));

    this.administrator = this.group
    .switchMap(group => this.store.select('authUser')
        .switchMap(user => Observable.of(group.security.administrators.indexOf(user.id > -1))
    );
}

ngOnDestroy() {
    this.alive = false;
}
```

Source: [RxJS Antipatterns](http://brianflove.com/2017/11/01/ngrx-anti-patterns/)