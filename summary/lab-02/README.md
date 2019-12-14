<p align="center">
    <img src="../../boring-theory-1/resources/header.png">
</p>

# Lab 02 - Angular Basics
## Objectives and Outcomes

* Work with structural directives
* Use different data binding methods - data flow from component to template
* User interaction through events

# Directives

## Structural Directives

# Data Binding

# Pipes

# Coding Time

Add a new component named event-list 

```sh
ng g component event-list

```

Create a folder under src/app named ‘models’ then inside of it create a file named ‘event.ts’

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

Copy and paste the following code in event-list.component.ts. 

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
Add the following in event-list.component.scss

```css
.container {
    display: flex;
    justify-content: center;
}

```
Edit app.component.html and add:

```html
<oevents-event-list></oevents-event-list>
```
<p align="center">
    <img src="../../lab-02/resources/eventList.png" border="1">
</p>

Import the mat-card component in app.module.ts

```javascript
import { MatCardModule } from '@angular/material/card';
. . .
  imports: [
  . . . 
    MatCardModule,
  . . . 

```
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
After saving all your changes you should see the event list and the first event’s details in your browser.
<p align="center">
    <img src="../../lab-02/resources/eventListDetails01.png" border="1">
</p>

Let’s refactor the code to separate our event details from the even list component.

Create a new component named event-details.
```sh
ng g component event-details
```

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

In event-list.component.html add a click event as follow 

```html
...
<mat-list-item *ngFor="let event of events" (click)="onSelectEvent(event)" >
... 

```
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
In event-details.component.scss add the following.

```javascript
#eventDetails{
  max-width: 600px;
}

```

Add the new event-details that we have created to be displayed in event-list.component.html

```html
<div class="container">
  ...
  <oevents-event-details [event]="selectedEvent"></oevents-event-details>
</div>

```

After saving all the changes, the application should update the event details according the selected event. 
<p align="center">
    <img src="../../lab-02/resources/eventListDetails02.png" border="1">
</p>

# Resources

<br/>

<p align="center">
    <img src="../../boring-theory-1/resources/header.png">
</p>
