---
id: 34
title: "Can't construct a query for the property "formHost" of "ParentDynamicFormComponent""
date: 2018-06-23T08:00:00+00:00
author: admin
layout: post
guid: http://www.darkedges.com/blog/?p=34
permalink: /2018/06/23/cant-construct-a-query/
categories:
  - angular

---
Last night I started to get the following error in my project

```
Can't construct a query for the property "formHost" of "ParentDynamicFormComponent"
```

<!-- more -->

All I had changed was in another module was adding my own Cutom HTTP Client, so I went back to it and put it back and it started working again. That is when I started going down the rabbithole to determine what I had done.

Turns out it was a Circular dependency in another class that 

The original code was 

```
import { Injectable } from '@angular/core';
import { BehaviorSubject } from 'rxjs';
import { Observable } from 'rxjs';
import { Store } from '@ngrx/store';
import { ActionAuthLogin, AuthTokenTypes, ActionAuthAccessToken } from './auth.reducer';
import { Authenticated } from 'models/authentication';
import { OpenAMHTTPService } from '../http.service';

@Injectable()
export class AuthService {
  private loggedIn = new BehaviorSubject<boolean>(false);

  constructor(
    private store: Store<any>,
    private httpService: OpenAMHTTPService
  ) {

  }

  get isLoggedIn() {
    return this.loggedIn.asObservable();
  }

  login(event: Authenticated) {
    this.store.dispatch(new ActionAuthLogin(event));
  }

  loggedin() {
    this.loggedIn.next(true);
    this.store.dispatch(new ActionAuthAccessToken());
  }

  logout() {
    this.httpService.logout().subscribe(
      action => {
        this.loggedIn.next(false);
      }
    );
  }
}
```

The circular dependency was the following line

```
 private httpService: OpenAMHTTPService
```

So off I went on a hunt to fix this issue and I found this discussion [https://github.com/angular/angular/issues/18224](https://github.com/angular/angular/issues/18224) and reading through it I found I should try

```
constructor(inj: Injector) {
    this.httpService = inj.get(OpenAMHTTPService)
  }
```

Aha It should be fixed, but no I then got the following

```
ERROR Error: Uncaught (in promise): RangeError: Maximum call stack size exceeded
RangeError: Maximum call stack size exceeded
    at _createProviderInstance$1 (core.es5.js:9480)
    ...  
```

Turns out I should have read some more as the solution was to not assign in the constructor, but in the call where I wanted to use it.

```
 logout() {
    this.httpService = this.injector.get(OpenAMHTTPService);
    this.httpService.logout().subscribe(
      action => {
        this.loggedIn.next(false);
      }
    );
  }
```

This solved my problem and I can continue on with my project.
