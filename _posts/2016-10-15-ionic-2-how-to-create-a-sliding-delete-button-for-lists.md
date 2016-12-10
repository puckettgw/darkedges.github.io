---
id: 206
title: 'Ionic 2: How to Create a Sliding Delete Button for Lists'
date: 2016-10-15T13:50:36+00:00
author: admin
layout: post
guid: http://www.darkedges.com/blog/?p=206
permalink: /2016/10/15/ionic-2-how-to-create-a-sliding-delete-button-for-lists/
categories:
  - Ionic 2
---
This post is inspired by <a href="http://www.joshmorony.com/ionic-2-how-to-create-a-sliding-delete-button-for-lists/">http://www.joshmorony.com/ionic-2-how-to-create-a-sliding-delete-button-for-lists/</a> and updates the code for the latest version of Ionic.

# Pre-Requisite
Ensure that you have the latest version of Ionic by issuing the following command
```
npm install -g ionic
```
I had some issues getting the `blank` project to serve as it was complaining about
```
WARN: No gulpfile found! 
```
After the reinstall this issue disappeared.
# Changes
The following outline the changes I made to get this tutorial to work.

## 2. Set up the List Data
There appears to be a number of file location changes.
`app/home/home.js` is now `src/pages/home/home.ts`

The original code was
```
import {Page} from 'ionic/ionic'
 
 
@Page({
  templateUrl: 'app/home/home.html',
})
export class HomePage {
  constructor() {
 
    this.items = [
        {title: 'item1'},
        {title: 'item2'},
        {title: 'item3'},
        {title: 'item4'},
        {title: 'item5'},
        {title: 'item6'}
    ];
 
  }
}
```

I had to change it to

```
import { Component } from '@angular/core';

import { NavController } from 'ionic-angular';

@Component({
  selector: 'page-home',
  templateUrl: 'home.html'
})
export class HomePage {

  public items: any;

  constructor(public navCtrl: NavController) {
    this.items = [
      { title: 'item1' },
      { title: 'item2' },
      { title: 'item3' },
      { title: 'item4' },
      { title: 'item5' },
      { title: 'item6' }
    ];
  }
}
```

## 3. Modify the Home Template
The original code called for
```html
<ion-navbar *navbar>
  <ion-title>
    Home
  </ion-title>
</ion-navbar>
 
<ion-content>
  <ion-list>
 
  <ion-item-sliding *ng-for="#item of items">
 
    <ion-item>
    {{item.title}}
    </ion-item>
 
    <ion-item-options>
      <button danger (click)="removeItem(item)"><icon trash></icon> Delete</button>
    </ion-item-options>
  </ion-item-sliding>
 
  </ion-list>
</ion-content>
```

this gave the following error
```html
Parser Error: Unexpected token # at column 8 in [ng-for #item of items] in HomePage@10:20 ("
<ion-content padding>
	<ion-list>
		<ion-item-sliding [ERROR ->]*ng-for="#item of items" #slidingItem>
			<ion-item>{{item.title}}</ion-item>
			<ion-item-options>
"): HomePage@10:20
```
and
```html
Can't bind to 'ng-forOf' since it isn't a known property of 'ion-item-sliding'.
```

I had to change it to
```html
<ion-header>
	<ion-navbar>
		<ion-title>
			Ionic Blank
		</ion-title>
	</ion-navbar>
</ion-header>

<ion-content padding>
	<ion-list>
		<ion-item-sliding *ngFor="let item of items" #slidingItem>
			<ion-item>{{item.title}}</ion-item>
			<ion-item-options>
				<button danger (click)="removeItem(item)"><ion-icon name="trash"></ion-icon> Delete</button>
			</ion-item-options>
		</ion-item-sliding>
	</ion-list>
</ion-content>
```

## 4. Create a Function to Remove the Data
The original code of
```
  removeItem(item){
    for(i = 0; i < this.items.length; i++) {
      if(this.items[i] == item){
        this.items.splice(i, 1);
      }
    }
  }
```
gave transpile errors of
```bash
Cannot find name 'i'.
```

The code was changed to 
```
  removeItem(item) {
    for (let i = 0; i < this.items.length; i++) {
      if (this.items[i] == item) {
        this.items.splice(i, 1);
      }
    }
  }
```
and the issue disappeared.