<p align="center">
    <img src="../../boring-theory-1/resources/header.png">
</p>

# Lab 04 - Services
## Objectives and Outcomes

* Dependency Injection.
* Get data from an API. 
* An introduction to Observables.
* Manage environment variables.


## Services and Dependency Injection

## Event service

```sh
ng g module core
ng g service core/event
```

Now, paste this code to the *event.service.ts* file:

```javascript
import { Injectable } from "@angular/core";
import { HttpClient, HttpHeaders } from "@angular/common/http";
import { Observable } from "rxjs";

@Injectable({
  providedIn: "root"
})
export class EventService {
  constructor(private http: HttpClient) {}

  getEvents(): Observable<any> {
    const headers = new HttpHeaders({
      "Content-Type": "application/json"
    });

    return this.http.get("assets/events.json", { headers });
  }
}
```

### Imports

### Reactive Extensions Library for JavaScript (RxJS)

### getEvents Observable

## Consume the Event service

The new *event-list.component.ts* looks like:

```javascript
import { Component, OnInit } from "@angular/core";
import { Event } from "../../models/event";

import { EventService } from "../../core/event.service";

@Component({
  selector: "oevents-event-list",
  templateUrl: "./event-list.component.html",
  styleUrls: ["./event-list.component.scss"]
})
export class EventListComponent implements OnInit {
  events: Event[];
  selectedEvent: Event;

  constructor(private eventService: EventService) {}

  ngOnInit() {
    this.getEvents();
  }

  onSelectEvent(event: Event) {
    this.selectedEvent = event;
  }

  getEvents() {
    this.eventService.getEvents().subscribe((events: Event[]) => {
      this.events = events;
      this.selectedEvent = events[0];
    });
  }
}
```

## The ending details

Create a new file *events.json* in the *assets* folder with next content (it have to be a well-formed json):

```json
[
  {
    "id": "0",
    "title": "Introduction to JS - basic",
    "location": "Barcelona",
    "date": "2020-03-16",
    "description": "Nulla aliqua duis adipisicing do amet et ullamco commodo id laborum nulla ipsum culpa. Lorem ipsum commodo quis amet consequat nostrud esse est deserunt. Laboris incididunt esse amet sunt tempor pariatur nisi irure nulla veniam id quis elit. Velit officia quis veniam aliqua. Cupidatat velit enim officia dolor ea veniam proident culpa ea duis labore nostrud. Occaecat in velit esse et. Duis anim ad elit ipsum occaecat Lorem veniam labore consequat laboris non.",
    "addedBy": "user01"
  },
  {
    "id": "1",
    "title": "Introduction to Angular",
    "location": "London",
    "date": "2019-10-28",
    "description": "Nulla aliqua duis adipisicing do amet et ullamco commodo id laborum nulla ipsum culpa. Lorem ipsum commodo quis amet consequat nostrud esse est deserunt. Laboris incididunt esse amet sunt tempor pariatur nisi irure nulla veniam id quis elit. Velit officia quis veniam aliqua. Cupidatat velit enim officia dolor ea veniam proident culpa ea duis labore nostrud. Occaecat in velit esse et. Duis anim ad elit ipsum occaecat Lorem veniam labore consequat laboris non.",
    "addedBy": "user01"
  },
  {
    "id": "2",
    "title": "Introduction to RXJS",
    "location": "London",
    "date": "2019-10-02",
    "description": "Nulla aliqua duis adipisicing do amet et ullamco commodo id laborum nulla ipsum culpa. Lorem ipsum commodo quis amet consequat nostrud esse est deserunt. Laboris incididunt esse amet sunt tempor pariatur nisi irure nulla veniam id quis elit. Velit officia quis veniam aliqua. Cupidatat velit enim officia dolor ea veniam proident culpa ea duis labore nostrud. Occaecat in velit esse et. Duis anim ad elit ipsum occaecat Lorem veniam labore consequat laboris non.",
    "addedBy": "user01"
  },
  {
    "id": "3",
    "title": "AWS",
    "location": "Berlin",
    "date": "2019-11-21",
    "description": "Nulla aliqua duis adipisicing do amet et ullamco commodo id laborum nulla ipsum culpa. Lorem ipsum commodo quis amet consequat nostrud esse est deserunt. Laboris incididunt esse amet sunt tempor pariatur nisi irure nulla veniam id quis elit. Velit officia quis veniam aliqua. Cupidatat velit enim officia dolor ea veniam proident culpa ea duis labore nostrud. Occaecat in velit esse et. Duis anim ad elit ipsum occaecat Lorem veniam labore consequat laboris non.",
    "addedBy": "user01"
  },
  {
    "id": "4",
    "title": "Angular NgRx - introduction",
    "location": "Madrid",
    "date": "2019-12-05",
    "description": "Nulla aliqua duis adipisicing do amet et ullamco commodo id laborum nulla ipsum culpa. Lorem ipsum commodo quis amet consequat nostrud esse est deserunt. Laboris incididunt esse amet sunt tempor pariatur nisi irure nulla veniam id quis elit. Velit officia quis veniam aliqua. Cupidatat velit enim officia dolor ea veniam proident culpa ea duis labore nostrud. Occaecat in velit esse et. Duis anim ad elit ipsum occaecat Lorem veniam labore consequat laboris non.",
    "addedBy": "user01"
  }
]
```

Now, import the service in the *core.module.ts* file:

```javascript
import { NgModule } from "@angular/core";
import { CommonModule } from "@angular/common";
import { HttpClientModule } from "@angular/common/http";

import { EventService } from "./event.service";

@NgModule({
  declarations: [],
  imports: [CommonModule, HttpClientModule],
  providers: [EventService]
})
export class CoreModule {}
```

Now, we will register the core module in the main app.module.ts where we always must have all things we want to use in the app.

```javascript
import { BrowserModule } from "@angular/platform-browser";
import { NgModule } from "@angular/core";

// Modules
import { CoreModule } from "./core/core.module"; // <--- NEW
import { SharedModule } from "./shared/shared.module";
import { AppRoutingModule } from "./app-routing.module";
import { EventsModule } from "./events/events.module";
import { LoginModule } from "./login/login.module";
import { ProfileModule } from "./profile/profile.module";

// Components
import { AppComponent } from "./app.component";
import { LandingPageComponent } from "./landing-page/landing-page.component";
import { ToolbarComponent } from "./toolbar/toolbar.component";
import { PageNotFoundComponent } from "./page-not-found/page-not-found.component";

@NgModule({
  declarations: [
    AppComponent,
    LandingPageComponent,
    ToolbarComponent,
    PageNotFoundComponent
  ],
  imports: [
    CoreModule, // <--- NEW
    BrowserModule,
    AppRoutingModule,
    SharedModule,
    EventsModule,
    LoginModule,
    ProfileModule
  ],
  providers: [],
  bootstrap: [AppComponent]
})
export class AppModule {}
```

## Transform data from HttpClient

In our *event.service.ts* file we have:

```javascript
...
return this.http.get(environment.apiURL, { headers }).pipe(
      retry(3),
      catchError(this.handleError)
    );
...
```

### Environments management

The *event.service.ts* will be:

```javascript
import { Injectable } from "@angular/core";
import {
  HttpClient,
  HttpErrorResponse,
  HttpHeaders
} from "@angular/common/http";
import { Observable, throwError } from "rxjs";
import { catchError, retry } from "rxjs/operators";
import { environment } from "../../environments/environment";

@Injectable({
  providedIn: "root"
})
export class EventService {
  constructor(private http: HttpClient) {}

  getEvents(): Observable<any> {
    const headers = new HttpHeaders({
      "Content-Type": "application/json"
    });

    return this.http.get(environment.apiURL, { headers }).pipe(
      retry(3),
      catchError(this.handleError)
    );
  }

  // Error handling

  private handleError(error: HttpErrorResponse) {
    if (error.error instanceof ErrorEvent) {
      // A client-side or network error occurred. Handle it accordingly.
      console.error("An error occurred:", error.error.message);
    } else {
      // The backend returned an unsuccessful response code.
      // The response body may contain clues about what went wrong,
      console.error(
        `Backend returned code ${error.status}, ` + `body was: ${error.error}`
      );
    }
    // return an observable with a user-facing error message
    return throwError("Something bad happened; please try again later.");
  }
}
```

##JSON-Server

```sh
npm install json-server -g 
```
* Configure the json-server. Choose one location in your computer and create a new folder called 'json-server'. Then go to this 
 <a href="./db.json">Download db.json file </a> and move it into the folder that we just have created. 
 
* Open the command line and go into the json-server folder. Now we are going to run our server with the following command: 

```sh
	json-server --watch db.json
```

Once our server is running, we should be able to access to our database data through the following address:

http://localhost:3000/events



## Adding the API Url 
Now that we have our server running, we have to make some changes in our code. First let’s change the environment.ts file and add the new API URL.

```javascript
export const environment = {
  production: true,
  apiURL: "http://localhost:3000/"
};
```

In event.service.ts let's change the API call.
```javascript
  getEvents(): Observable<any> {
    const headers = new HttpHeaders({
      "Content-Type": "application/json"
    });

    return this.http.get(environment.apiURL + "events", { headers }).pipe(
      retry(3),
      catchError(this.handleError)
    );
  }
```

Save all the changes, the application should display now the event data.

<br/>

<p align="center">
    <img src="../../boring-theory-1/resources/header.png">
</p>
