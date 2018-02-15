---
title: "Angular Combine Multiple Subscription Requests"
date: 2018-02-15T10:23:48+01:00
---

When using the async pipe with multiple subscriptions on the store there will be multiple options requests, the actual requests are combined to one request.
To combine the different request we can use `share()`.

HTML template example:

        <ama-module-menu>
            [course]="course | async"
        ></ama-module-menu>
        <ama-module-content>
            [course]="course | async
        ></ama-module-content>

Your filling the course observable use the `share()` operator.

        ngOnInit() {
            this.course = this.activatedRoute.paramMap
                .takeWhile(() => this.alive)
                .do(params => this.store.dispatch(new LoadCourseAction({ id: params.get('courseId') })))
                .switchMap(() => this.store.select(getCourse))
                .share()
        }

        ngOnDestroy() {
            this.alive = false;
        }


Side note:
If you use `*ngIf` result binding you avoid the duplicate requests as well.

        <ama-module-menu *ngIf="course | async as c">
            [course]="c"
        </ama-module-menu>


Source: [RxJS Antipatterns](http://brianflove.com/2017/11/01/ngrx-anti-patterns/)