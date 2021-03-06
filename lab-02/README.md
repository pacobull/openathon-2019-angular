<p align="center">
    <img src="../boring-theory-1/resources/header.png">
</p>

# Lab 02 - Angular Basics
## Objectives and Outcomes
This lab offers a quick introduction to Angular directives. Beside we'll look at how to display data from our components and allow the user to interact with the application through an event.

* Work with structural directives
* Use different data binding methods - data flow from component to template
* User interaction through events

# Directives

Directives give instructions to	Angular	on how to render the templates to the	DOM. In the previous lab-01 we have seen the Component directive but there are two other kinds of directives	in Angular:	

* **Components** directives with a template.
* **Structural** directives change the DOM layout by adding and removing DOM elements.
* **Attribute** directives change the appearance or behaviour of an element, component, or another directive.


## Structural Directives
Structural Directives allow you to change the view structure. In other words, it alters the layout by adding, replacing or removing elements in DOM. 
* Examples of Structural Directives:

  ngIf 
  ```html
    <div *ngIf=“dataExist”> The data exist </div>
  ```

  ngFor
  ```html
    <li *ngFor="let color of colors">{{color.name}}</li>
  ```

# Data Binding
Data Binding could be described as the data flow between the component and the template and the link between the events generated by the user to the component. 

There are four ways of data binding: 

* **Interpolation** is a one-way data binding from the data source to the view. We can use the string interpolation with the double curly braces '{{ }}'. 
```html
  {{color.name}}
```

* **Property binding**  is also a one way binding from the data source to the view, allowing you to change value anytime at the component level. It defines and updates a variable's value in component and displays it in the view. Interpolation is a special syntax that Angular converts into property binding. You can make use of it using the square brackets '[ ]'.
```html
  [color] = "selectedColor"
```

* **Event binding** is a one-way binding from the view to the data source. For example, it could be a button click, that calls a function. The event action is set between parentheses '( )'
```html
 (buttonClick) = "changeColor()"
```

* **Two-way binding** is a two-way synchronized data binding between the component and the view. It could be said that is a combination of the event and property binding. Its syntax is between square brackets and parentheses inside of it, popular now it as banana in a box '[()]'.
```html
  [(ngModel)] = "color.name"
```


| Bindin Type                         | Data Direction                             | Syntax                    | Example              |
|-------------------------------------|--------------------------------------------|---------------------------|---------------------------
| Interpolation              |  One-way; from	data source to	view	target     | {{ Expression }}          |  {{color.name}}    |
| Property binding           |  One-way; from	data source to	view	target     | [target] = "statement"    |  [color] = "selectedColor" | 
| Event binding              |  One-way; binding from  the view targe to the data source. | (target)="statement"   | (buttonClick) = "changeColor()"   |
| Two-way binding            |  Two-way     | [(target)]="expression"        |  [(ngModel)] = "color.name"    |
 
 **_Side Note:_** As we can see properties targets are defined on the left side of the expression. The target property has to be specify as Input or Output property using a decorator in the controller. 
  * @Input() 
  * @Output()


# Pipes
Pipes were used to be called filters in earlier Angular versions. A pipe takes data (integers, strings, arrays, and dates) as input and transforms it with the desired format as the output to be displayed in the browser. Angular has some built-in pipes for example: CurrencyPipe, DatePipe, UpperCasePipe, etc. In addition we can also create and  <a href="https://angular.io/guide/pipes#custom-pipes">custom our own pipes</a>.

We can use more than one pipe at the same time as the following example that would display FRIDAY, APRIL 15, 1988

```html
{{ birthday | date | uppercase}}

```

# Coding Time

Now we are going to try to make use of some of these concepts in our application. We are going to display an event list.

Add a new component named event-list 

```sh
ng g component event-list
```
[[Commit 14]](https://github.com/pacobull/open-events-front/commit/4d75568acae01972c48d2b4c53e957be8e0d271e)


Now we are going to create the event model. In this file we are going to define the event's properties, it is basically a blueprint that will help us when we create new variables of type event.

Create a folder under src/app named ‘models’ then inside of it create a file named ‘event.ts’
<p align="center">
    <img src="./resources/eventModelFolder.png" border="1">
</p>

Copy and paste the following code in event.ts file.

```javascript
export class Event {
  id: string;
  title: string;
  location: string;
  date: Date;
  description: string;
  addedBy: string;
}
```

[[Commit 15]](https://github.com/pacobull/open-events-front/commit/4cd07baa2ae533fa0febe5703224dea1daff5696)

Copy and paste the following code in event-list.component.ts. As you can see, we are importing our even model. And we make use of it in the 'EVENTS' constant, which is an array of events. Besides we create the 'selectedEvent' variable and set it to be by default the first value from the 'EVENTS' array.


```javascript
import { Component, OnInit } from '@angular/core';
import { Event } from '../models/event';

const EVENTS: Event[] = [
  {
    id: '0',
    title: 'Introduction to JS - basic',
    location: 'Barcelona',
    date: new Date("2020-03-16"),
    description: 'Nulla aliqua duis adipisicing do amet et ullamco commodo id laborum nulla ipsum culpa. Lorem ipsum commodo quis amet consequat nostrud esse est deserunt. Laboris incididunt esse amet sunt tempor pariatur nisi irure nulla veniam id quis elit. Velit officia quis veniam aliqua. Cupidatat velit enim officia dolor ea veniam proident culpa ea duis labore nostrud. Occaecat in velit esse et. Duis anim ad elit ipsum occaecat Lorem veniam labore consequat laboris non.',
    addedBy: 'user01'

  },
  {
    id: '1',
    title: 'Introduction to Angular',
    location: 'London',
    date: new Date("2019-10-28"),
    description: 'Nulla aliqua duis adipisicing do amet et ullamco commodo id laborum nulla ipsum culpa. Lorem ipsum commodo quis amet consequat nostrud esse est deserunt. Laboris incididunt esse amet sunt tempor pariatur nisi irure nulla veniam id quis elit. Velit officia quis veniam aliqua. Cupidatat velit enim officia dolor ea veniam proident culpa ea duis labore nostrud. Occaecat in velit esse et. Duis anim ad elit ipsum occaecat Lorem veniam labore consequat laboris non.',
    addedBy: 'user01'
  },
  {
    id: '2',
    title: 'Introduction to RXJS',
    location: 'London',
    date: new Date("2019-10-02"),
    description: 'Nulla aliqua duis adipisicing do amet et ullamco commodo id laborum nulla ipsum culpa. Lorem ipsum commodo quis amet consequat nostrud esse est deserunt. Laboris incididunt esse amet sunt tempor pariatur nisi irure nulla veniam id quis elit. Velit officia quis veniam aliqua. Cupidatat velit enim officia dolor ea veniam proident culpa ea duis labore nostrud. Occaecat in velit esse et. Duis anim ad elit ipsum occaecat Lorem veniam labore consequat laboris non.',
    addedBy: 'user01'
  },
  {
    id: '3',
    title: 'AWS',
    location: 'Berlin',
    date: new Date("2019-11-21"),
    description: 'Nulla aliqua duis adipisicing do amet et ullamco commodo id laborum nulla ipsum culpa. Lorem ipsum commodo quis amet consequat nostrud esse est deserunt. Laboris incididunt esse amet sunt tempor pariatur nisi irure nulla veniam id quis elit. Velit officia quis veniam aliqua. Cupidatat velit enim officia dolor ea veniam proident culpa ea duis labore nostrud. Occaecat in velit esse et. Duis anim ad elit ipsum occaecat Lorem veniam labore consequat laboris non.',
    addedBy: 'user01'
  },
  {
    id: '4',
    title: 'Angular NgRx - introduction',
    location: 'Madrid',
    date: new Date("2019-12-05"),
    description: 'Nulla aliqua duis adipisicing do amet et ullamco commodo id laborum nulla ipsum culpa. Lorem ipsum commodo quis amet consequat nostrud esse est deserunt. Laboris incididunt esse amet sunt tempor pariatur nisi irure nulla veniam id quis elit. Velit officia quis veniam aliqua. Cupidatat velit enim officia dolor ea veniam proident culpa ea duis labore nostrud. Occaecat in velit esse et. Duis anim ad elit ipsum occaecat Lorem veniam labore consequat laboris non.',
    addedBy: 'user01'
  }
]

@Component({
  selector: 'oevents-event-list',
  templateUrl: './event-list.component.html',
  styleUrls: ['./event-list.component.scss']
})

export class EventListComponent implements OnInit {

  events = EVENTS;

  selectedEvent = EVENTS[0];

  constructor() { }

  ngOnInit() {
  }

}
```

[[Commit 16]](https://github.com/pacobull/open-events-front/commit/0d86b35601aae7677dd93aa5e090698e455e4e52)

In order to display the event list, we will use the <a href="https://material.angular.io/components/list/overview">mat-list</a> component from Angular Material. 
Delete the content from event-list.component.html and copy and paste the following code.

```html
<div class="container">
  <mat-list>
    <mat-list-item *ngFor="let event of events">
      <button mat-button>{{event.title}}</button>
    </mat-list-item>
  </mat-list>
</div>
```

[[Commit 17]](https://github.com/pacobull/open-events-front/commit/36ae80cd810474122ecffb2b96d631c3c03cc574)

Add the following in event-list.component.scss

```css
.container {
    display: flex;
    justify-content: center;
}
```

[[Commit 18]](https://github.com/pacobull/open-events-front/commit/f763ddc018de16dc5b1e8e74151971d49afa2ae0)

Edit app.component.html and add:

```html
<oevents-event-list></oevents-event-list>
```
[[Cpmmit 19]](https://github.com/pacobull/open-events-front/commit/5dda889e794a6c4bf9861a5845ec0a5315b2c78c)

<p align="center">
    <img src="./resources/eventList.png" border="1">
</p>


Now we will use <a href="https://material.angular.io/components/card/overview">'mat-card'</a> component to show the event details.
Import the mat-card component in app.module.ts

```javascript
import { MatCardModule } from '@angular/material/card';
. . .
  imports: [
  . . . 
    MatCardModule,
  . . . 

```

[[Commit 20]](https://github.com/pacobull/open-events-front/commit/b93977ae230b8caefecbe5285519f8e58729cef6)

Edit event-list.component.html to use mat-card and display the event details as following:

```html
<div class="container">

  <mat-list>
    <mat-list-item *ngFor="let event of events">
      <button mat-button>{{event.title}}</button>
    </mat-list-item>
  </mat-list>

  <mat-card *ngIf="selectedEvent" id="eventDetails">
    <mat-card-header>
      <mat-card-title>
        <h3>{{selectedEvent.title | uppercase}}</h3>
      </mat-card-title>
      <mat-card-subtitle>
        <p>{{selectedEvent.location}} - {{selectedEvent.date | date: 'dd/MM/yyyy'}} </p>
      </mat-card-subtitle>
    </mat-card-header>

    <mat-card-content>
      <p>{{selectedEvent.description}}
      </p>
    </mat-card-content>

  </mat-card>

</div>
```

[[Commit 21]](https://github.com/pacobull/open-events-front/commit/9fd3a6d177d863714fe306d4593697964c02881e)

Edit event-list.component.scss as follow.

```css
.container {
    display: flex;
    justify-content: center;
}
#eventDetails{
  max-width: 600px;
}
```

[[Commit 22]](https://github.com/pacobull/open-events-front/commit/93b881cbe0e7669bc12124fbf80ce6e25c22a653)

After saving all your changes you should see the event list and the first event’s details in your browser.
<p align="center">
    <img src="./resources/eventListDetails01.png" border="1">
</p>

Now let’s make some changes to load the details from the selected event from our list. 

First things first. Let’s refactor the code to separate our event details from the even list component.

Create a new component named event-details.
```sh
ng g component event-details
```
[[Commit 23]](https://github.com/pacobull/open-events-front/commit/374e98794d4cf87bfcc1bc4b82e91212cedcf43b)

Remove the following from event-list-component.html
```html
<mat-card *ngIf="selectedEvent" id="eventDetails">
    <mat-card-header>
      <mat-card-title>
        <h3>{{selectedEvent.title | uppercase}}</h3>
      </mat-card-title>
      <mat-card-subtitle>
        <p>{{selectedEvent.location}} - {{selectedEvent.date | date: 'dd/MM/yyyy'}} </p>
      </mat-card-subtitle>
    </mat-card-header>

    <mat-card-content>
      <p>{{selectedEvent.description}}
      </p>
    </mat-card-content>
  </mat-card>
```

[[Commit 24]](https://github.com/pacobull/open-events-front/commit/d03f0dea46f9565c84f5ecb84c7b458f5d92d677)

Copy and paste this code in event-details.componet.html
```html
<mat-card *ngIf="event" id="eventDetails">
  <mat-card-header>
    <mat-card-title>
      <h3>{{event.title | uppercase}}</h3>
    </mat-card-title>
    <mat-card-subtitle>
      <p>{{event.location}} - {{event.date | date: 'dd/MM/yyyy'}} </p>
    </mat-card-subtitle>
  </mat-card-header>

  <mat-card-content>
    <p>{{event.description}}
    </p>
  </mat-card-content>
</mat-card>

```

[[Commit 25]](https://github.com/pacobull/open-events-front/commit/0c99c57e9703922b010b1468ffe714479eee8627)

In event-list.component.html add a click event as follow 

```html
...
<mat-list-item *ngFor="let event of events" (click)="onSelectEvent(event)" >
... 
```

[[Commit 26]](https://github.com/pacobull/open-events-front/commit/d11f8083c760d3ecfea9352773a1c01b27232c4e)

Then in event-list.component.ts add the ‘onSelectEvent’ function.

```javascript
export class EventListComponent implements OnInit {
  events = EVENTS;
  selectedEvent = EVENTS[0];
  . . .
  onSelectEvent(event: Event) {
    this.selectedEvent = event;
  }
}

```

[[Commit 27]](https://github.com/pacobull/open-events-front/commit/5f4964ac1540e1dd176b5996c5a123c16e6fbb1c)

In event-details.component.ts, import and add then input decorator to retrieve the event passed by event-list.

```javascript
import { Component, OnInit, Input } from '@angular/core';
import { Event } from '../models/event';
. . .
export class EventDetailsComponent implements OnInit {
  @Input()
  event: Event;
  . . .
}

```

[[Commit 28]](https://github.com/pacobull/open-events-front/commit/ce8baf8401be828048b1bdba46bb1d9a3c2c5205)

In event-details.component.scss add the following.

```javascript
#eventDetails{
  max-width: 600px;
}

```

[[Commit 29]](https://github.com/pacobull/open-events-front/commit/3860f97815feb535742eea01c06dd6afa8cd94f6)

Add the new event-details that we have created to be displayed in event-list.component.html

```html
<div class="container">
  ...
  <oevents-event-details [event]="selectedEvent"></oevents-event-details>
</div>

```

[[Commit 30]](https://github.com/pacobull/open-events-front/commit/53ef85c739923c3edb0db5f7bebd9f9b56aa94b7)

After saving all the changes, the application should update the event details according the selected event. 
<p align="center">
    <img src="./resources/eventListDetails02.png" border="1">
</p>

# Resources

[Structural Directives](https://angular.io/guide/structural-directives)

[Angular Pipes](https://angular.io/guide/pipes)

[mat-list](https://material.angular.io/components/list/overview")

[mat-card](https://material.angular.io/components/card/overview")

<br/>
<br/>
<br/>

[< Lab 01 - Starting a New Angular Project](../lab-01) | [Lab 03 - Routing Basics >](../lab-03)



<p align="center">
    <img src="../boring-theory-1/resources/header.png">
</p>
