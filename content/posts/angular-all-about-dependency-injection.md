---
title: "Angular All About Dependency Injection"
date: 2018-02-27T17:52:59+01:00
draft: true
---

## Dependency with parameters
```js
// Service
@Injectable()
export class LogDebugger {
    constructor(private enabled: boolean) { }

    //...
}

// In your component
//...
providers: [
    {
        provide: LogDebugger,
        useFactory: () => {
            return new LogDebugger(true);
        }
    }
]
```

## Dependency with depenencies
```js
// Service
@Injectable()
export class LogDebugger {
    constructor(private service: SomeService) { }

    //...
}

// In your component
//...
providers: [
    SomeService,
    {
        provide: LogDebugger,
        deps: [SomeService],
        useFactory: (someService) => {
            return new LogDebugger(someService);
        }
    }
]
```