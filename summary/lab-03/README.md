<p align="center">
    <img src="../../boring-theory-1/resources/header.png">
</p>

# Lab 03 - Routing Basics
## Objectives and Outcomes

* We will have a URL for each view. 
* It will allow us to navigate back and forth (navigation history).
* It will allow to manage advanced features like URL parameters, child views, route guards...

## A new folder structure

## Feature Modules

```sh
ng g module events
ng g module profile
ng g module login
```

Move "event-list" and "event-details" to "events" folder. We have to modify some import paths to consider the new location of those files. In "event-list.component.ts" and "event-details.component.ts" files we need to change the import sentence from "../models/event" to "../../models/event" to reflect the correct location.

## Shared Module

We are going to create a folder named "shared" and a new module shared.module.ts inside. To do this in one step we need to execute:

```sh
ng g module shared
```
The new created module must look like next:

```javascript
import { NgModule } from "@angular/core";

//Angular material
import { BrowserAnimationsModule } from "@angular/platform-browser/animations";
import { MatToolbarModule } from "@angular/material/toolbar";
import { MatButtonModule } from "@angular/material/button";
import { MatListModule } from "@angular/material/list";
import { MatCardModule } from "@angular/material/card";
import "hammerjs";

@NgModule({
  declarations: [],
  imports: [
    BrowserAnimationsModule,
    MatToolbarModule,
    MatButtonModule,
    MatListModule,
    MatCardModule
  ],
  exports: [
    BrowserAnimationsModule,
    MatToolbarModule,
    MatButtonModule,
    MatListModule,
    MatCardModule
  ],
  entryComponents: []
})
export class SharedModule {}
```

Now, we will modify the "app.module.ts" and the "events.module.ts" for ordering the new composition of our app.

For app.module.ts:

```javascript
import { BrowserModule } from "@angular/platform-browser";
import { NgModule } from "@angular/core";

// Modules
import { SharedModule } from "./shared/shared.module";
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
    BrowserModule,
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

For events.module.ts:

```javascript
import { NgModule } from "@angular/core";
import { CommonModule } from "@angular/common";

//Modules
import { SharedModule } from "../shared/shared.module";

// Components
import { EventListComponent } from "./event-list/event-list.component";
import { EventDetailsComponent } from "./event-details/event-details.component";

@NgModule({
  imports: [CommonModule, SharedModule],
  declarations: [EventListComponent, EventDetailsComponent]
})
export class EventsModule {}
```

As you can see, the events module has their own components, the app module has the minimum necessary and the shared module have the shared components. 

Look at this last module: we need to "export" the components that other modules will need. Look at "events" and "app" modules: you can see that we import the shared module because we need them in some components, which are part of the shared module. 

## Basic Routing

We need to import required modules from "@angular/router". This RouterModule have to be imported by the main module (app.module.ts).
We will create a new file named "app-routing.module.ts" in the app folder with next content:

```javascript
import { NgModule } from "@angular/core";
import { Routes, RouterModule } from "@angular/router";

import { LandingPageComponent } from "./landing-page/landing-page.component";
import { EventListComponent } from "./events/event-list/event-list.component";
import { ProfileComponent } from "./profile/profile.component";
import { LoginComponent } from "./login/login.component";
import { PageNotFoundComponent } from "./page-not-found/page-not-found.component";

const routes: Routes = [
  { path: "home", component: LandingPageComponent },
  { path: "events", component: EventListComponent },
  { path: "profile", component: ProfileComponent },
  { path: "login", component: LoginComponent },

  { path: "", redirectTo: "/home", pathMatch: "full" },
  { path: "**", component: PageNotFoundComponent }
];

@NgModule({
  imports: [RouterModule.forRoot(routes)],
  exports: [RouterModule]
})
export class AppRoutingModule {}
```


> **_Side Note:_**  Remember to create your "Not Found" component.



Add this import in app.module.ts:

```javascript
import { AppRoutingModule } from "./app-routing.module";
```

and in the imports array:

```javascript
  imports: [
    BrowserModule,
    AppRoutingModule, <---- NEW
    SharedModule,
    EventsModule,
    LoginModule,
    ProfileModule
  ],
  ```

When a component is activated using their path, where can we see its html view? The response is a new element (directive) named `Router Outlet` that will indicate in the html where the new component will be shown. To do this you must change the app.component.html and insert our new element as a component:

```javascript
  <div class="body">
    <oevents-toolbar></oevents-toolbar>
    <router-outlet></router-outlet>
  </div>
  ```


```
ng g component /login
ng g component /profile
```

## Router Link

```javascript
<mat-toolbar color="primary">
  <a mat-button routerLink="/home" routerLinkActive="activeLink">Home</a>
  <a mat-button routerLink="/events" routerLinkActive="activeLink">Events</a>
  <a mat-button routerLink="/profile" routerLinkActive="activeLink">Profile</a>
  <a mat-button routerLink="/login" routerLinkActive="activeLink">Login</a>
</mat-toolbar>
```

## From model class to model interface

```javascript
export interface Event {
    id: string;
    title: string;
    location: string;
    date: Date;
    description: string;
    addedBy: string;
}
```

<br/>

<p align="center">
    <img src="../../boring-theory-1/resources/header.png">
</p>
