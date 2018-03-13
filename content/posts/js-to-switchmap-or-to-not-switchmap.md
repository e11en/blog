---
title: "To Switchmap Or To Not Switchmap"
date: 2018-03-13T11:54:59+01:00
---

Note: This article will be extended over time.

# What the map
When we want to use a flattening operator you usually come across `switchMap` but this is not the only map operator we have. We have a total of 4 maps to choose from:

- `mergeMap` (or `flatMap` which is an alias)
- `concatMap`
- `switchMap`
- `exhaustMap`

### mergeMap/flatMap
This will concurrently handle each dispacthed request, this means it will not wait for any other request to complete and also doesn't abort the previous one if a new request is fired. This might be a good choice if you _don't mind the order in which the requests complete_.

### concatMap
This is the same as using the `mergeMap` but then feeding it one request at a time. _Process all requests but one request at a time_. If you are unsure about which map to choose this is always a save pick.

### switchMap
This will make sure _only one request is going through and will cancel the previous observable and created a new one_. If you are not concerned about the return value or completion this is the map for you!

### exhaustMap
This is the opposite of `switchMap`, it will ignore the new request if there is already one pending. So it _will wait for the first request to finish and will ignore any new ones_.


### Sources
- [Switchmap bugs](https://blog.angularindepth.com/switchmap-bugs-b6de69155524)
- [Learn RxJS](https://www.learnrxjs.io)