<p align="center">
    <img src="../../boring-theory-1/resources/header.png">
</p>

# Lab 05 - Routing 2 and CRUD 

* Implement CRUD operations
* Send parameters through URL and read them 

> **_Side Note:_** Before starting this lab, download this new <a href="./app/db.json">Download db.json file </a>and update use it for your json-server previously installed in lab04. Remember that you can start the json server with the following command:
```sh
	json-server --watch db.json
```

## Read Event Details

* Add the MatTableModule from Material in the shared.module.ts

```javascript
...
import { MatTableModule } from "@angular/material/table";
...
 imports: [
	...
	MatTableModule
  ],
  ...
exports: [
    ...
    MatTableModule,
    ...
```

* Edit event-list.component.html

```javascript
<div class="container">

  <div id="eventTable">
    <table mat-table [dataSource]="events" class="mat-elevation-z8">
      <ng-container matColumnDef="Date">
        <th mat-header-cell *matHeaderCellDef>Date</th>
        <td mat-cell *matCellDef="let element">{{ element.date }}</td>
      </ng-container>

      <ng-container matColumnDef="Location">
        <th mat-header-cell *matHeaderCellDef>Location</th>
        <td mat-cell *matCellDef="let element">{{ element.location }}</td>
      </ng-container>

      <ng-container matColumnDef="Title">
        <th mat-header-cell *matHeaderCellDef>Title</th>
        <td mat-cell *matCellDef="let element">{{ element.title }}</td>
      </ng-container>

      <tr mat-header-row *matHeaderRowDef="displayedColumns"></tr>
      <tr
        mat-row
        *matRowDef="let row; columns: displayedColumns"
        [routerLink]="['/eventDetails/', row.id]"
      ></tr>
    </table>
  </div>
</div>
```

* copy and paste the following in event-list.component.scss:

```javascript
.container {
  display: flex;
  justify-content: center;
  flex-flow: column;
  margin: 10%;
  margin-top: 2%;
}

#add-event-btn {
  display: flex;
  justify-content: flex-end;
}

button {
  max-width: 100px;
}

table {
  width: 100%;
  margin-top: 10px;
}
```

* Add the columns names that will be displayed in the table headers. In event-list.component.ts change the code as follow:


```javascript
export class EventListComponent implements OnInit {
  ...
  displayedColumns: string[] = ["Date", "Location", "Title"];
```

* Add the eventDetails route in app-routing.module.ts
```javascript
...
import { EventDetailsComponent } from "./events/event-details/event-details.component";
...
const routes: Routes = [
  { path: "home", component: LandingPageComponent },
  { path: "events", component: EventListComponent },
  { path: "profile", component: ProfileComponent },
  { path: "login", component: LoginComponent },
  { path: "eventDetails/:id", component: EventDetailsComponent },
  ...

```

* Now we need to import RouterModule in our shared.moudule.ts

```javascript
import { RouterModule } from "@angular/router";
...
  imports: [
    ...
    RouterModule,
    ...
    ]
    ...
  exports:[
    ...
    RouterModule,
    ...
  ]
```



* As we said, we are going to change the event-details.component.html to use a mat-card and include a button to edit the event. Also, we are adding the 'Edit Event' button that will redirect us to a new view.
```javascript
<div class="container">
  <div id="event-card-details">
    <div id="edit-event-btn">
      <button mat-raised-button color="primary" [routerLink]="['/addEditEvent/', event.id]">
        Edit Event
      </button>
    </div>

    <mat-card *ngIf="event">
      <mat-card-header>
        <mat-card-title>
          <h3>{{ event.title | uppercase }}</h3>
        </mat-card-title>
        <mat-card-subtitle>
          <p>{{ event.location }} - {{ event.date | date: "dd/MM/yyyy" }}</p>
        </mat-card-subtitle>
      </mat-card-header>

      <mat-card-content>
        <p>{{ event.description }}</p>
      </mat-card-content>
    </mat-card>
  </div>
</div>
```

* And event-details.component.scss

```javascript
.container {
  display: flex;
  justify-content: center;
  flex-direction: column;
  align-items: center;
  justify-content: center;
}

#event-card-details {
  display: flex;
  flex: 1;
  flex-flow: column;
  margin: 2%;
  max-width: 800px;
}

#edit-event-btn {
  display: flex;
  justify-content: flex-end;
  margin-bottom: 15px;
}

```

* Add a new method in event-service to get the event using the GET method.

```javascript
getEvent(id: string): Observable<any> {
    const headers = new HttpHeaders({
      "Content-Type": "application/json"
    });

    return this.http.get(environment.apiURL + "events/" + id, { headers }).pipe(
      retry(3),
      catchError(this.handleError)
    );
  }
```

* We are going to get the parameter id from the URL and using our event service to get the specific event from the Database. 

event-details.component.ts
```javascript
import { Component, OnInit, Input } from "@angular/core";
import { Event } from "../../models/event";
import { ActivatedRoute } from "@angular/router";
import { EventService } from "../../core/event.service";

@Component({
  selector: "oevents-event-details",
  templateUrl: "./event-details.component.html",
  styleUrls: ["./event-details.component.scss"]
})
export class EventDetailsComponent implements OnInit {
  event: Event;

  constructor(
    private route: ActivatedRoute,
    private eventService: EventService,
    private router: Router
  ) {}

  ngOnInit() {
    const id = this.route.snapshot.params["id"];
    this.eventService.getEvent(id).subscribe((event: Event) => {
      console.log(event);
      this.event = event;
    });
  }
}
```


* Save all the changes that we have made. You should be able to get the event list, click in one of them and see its details in a new view.


## Create and Update an Event

* Edit the event.service.ts and add the methods 'addEvent' and 'updateEvent' that will save our event in the db using the POST method or update the event using the PUT method.

```javascript

import { Event } from "../models/event";
...

 addEvent(event: Event): Observable<any> {
    const headers = new HttpHeaders({
      "Content-Type": "application/json"
    });

    return this.http
      .post(environment.apiURL + "events/", event, { headers })
      .pipe(
        retry(3),
        catchError(this.handleError)
      );
  }

   updateEvent(event: Event): Observable<any> {
    const headers = new HttpHeaders({
      "Content-Type": "application/json"
    });

    return this.http
      .put(environment.apiURL + "events/" + event.id, event, { headers })
      .pipe(
        retry(3),
        catchError(this.handleError)
      );
  }
```

* Add the MatFormFieldModule and MatInputModule from Material in the shared.module.ts

```javascript
...
import { FormsModule, ReactiveFormsModule } from "@angular/forms";
import { MatFormFieldModule } from "@angular/material/form-field";
import { MatInputModule } from "@angular/material/input";
...
 imports: [
  ...
  FormsModule,
  ReactiveFormsModule,
	MatFormFieldModule,
  MatInputModule
  ],
  ...
 exports: [
  ...
  FormsModule,
  ReactiveFormsModule,
  MatFormFieldModule,
  MatInputModule
  ]
  ...
```

* Create a new component named add-edit-event inside events

```javascript
 ng g component events/add-edit-event
```


* Copy and paste the following code in add-edit-event.component.html. As you can see, we are using a form an the <a href="https://material.angular.io/components/form-field/overview">mat-form-field</a> form Angular Material. The form is linked to 'addEditForm', a variable of type FormGroup declared in the controller, that will share its properties with the form fields through the formControlName. Furthermore, we have the 'save' button that will submit the form.

```html
<div class="container">
  <h2>Add Event</h2>
  <form
    novalidate
    [formGroup]="addEditForm"
    #fform="ngForm"
    (ngSubmit)="onSubmit()"
    *ngIf="addEditForm"
  >
    <mat-form-field>
      <input matInput placeholder="Title" type="text" formControlName="title" />
    </mat-form-field>
    <mat-form-field>
      <input matInput placeholder="Date" type="date" formControlName="date" />
    </mat-form-field>
    <mat-form-field>
      <input
        matInput
        placeholder="Location"
        type="text"
        formControlName="location"
      />
    </mat-form-field>

    <mat-form-field>
      <textarea
        matInput
        placeholder="Description"
        rows="4"
        type="text"
        formControlName="description"
      ></textarea>
    </mat-form-field>

    <div id="save-btn">
      <button mat-raised-button color="primary" type="submit">
        Save
      </button>
    </div>
  </form>
</div>

```

* Change the add-edit-event.component.ts as following. We are reading the parameter id that we are getting through the URL and decide then if we are adding or editing a new event. Then we are creating the form 'addEditForm' (previously we need to import 'FormBuilder' and 'FormGroup' in order to declare it) and then declare and set their properties and values when required (in case that we are editing an event). Finally, we have the 'submit()' method that will add or update the event through the 'EventService'.


```javascript
import { Component, OnInit } from "@angular/core";
import { FormBuilder, FormGroup } from "@angular/forms";
import { Event } from "../../models/event";
import { EventService } from "../../core/event.service";
import { Router } from "@angular/router";
import { ActivatedRoute } from "@angular/router";

@Component({
  selector: "oevents-add-edit-event",
  templateUrl: "./add-edit-event.component.html",
  styleUrls: ["./add-edit-event.component.scss"]
})
export class AddEditEventComponent implements OnInit {
  addEditForm: FormGroup;
  event: Event;

  constructor(
    private fb: FormBuilder,
    private eventService: EventService,
    private router: Router,
    private route: ActivatedRoute
  ) {}

  ngOnInit() {
    const id = this.route.snapshot.params["id"];

    if (id) {
      this.eventService.getEvent(id).subscribe((event: Event) => {
        console.log(event);
        this.event = event;
        this.createForm();
      });
    } else {
      this.createForm();
    }
  }

  createForm() {
    if (this.event) {
      this.addEditForm = this.fb.group({
        title: this.event.title,
        location: this.event.location,
        date: this.event.date,
        description: this.event.description,
        addedBy: this.event.addedBy,
        id: this.event.id
      });
    } else {
      this.addEditForm = this.fb.group({
        title: "",
        location: "",
        date: "",
        description: "",
        addedBy: "",
        id: ""
      });
    }
  }

  onSubmit() {
    this.event = this.addEditForm.value;
    if (this.event.id) {
      this.eventService.updateEvent(this.event).subscribe((event: Event) => {
        console.log(event);
        this.addEditForm.reset();
        this.router.navigate(["/events"]);
      });
    } else {
      this.eventService.addEvent(this.event).subscribe((event: Event) => {
        console.log(event);
        this.addEditForm.reset();
        this.router.navigate(["/events"]);
      });
    }
  }
}

```

* Add the following code in add-edit-event.component.scss

```css
.container {
  display: flex;
  justify-content: center;
  flex-direction: column;
  align-items: center;
  justify-content: center;
}
form {
  display: flex;
  flex-flow: column;
  width: 40%;
}

#save-btn {
  display: flex;
  justify-content: flex-end;
}

```



* In event-list.component.html add a button that will redirect to add event view. As we said before, we'll pass an empty string as the parameter because in this case, we are creating a new event.

```html
<div class="container">
  <div id="add-event-btn">
    <button mat-raised-button color="primary" [routerLink]="['/addEditEvent/', '']">
      Add Event
    </button>
  </div>
  ...
```
* Add the addEditEvent route in app-routing.module.ts
```javascript
import { AddEditEventComponent } from "./events/add-edit-event/add-edit-event.component";
...
const routes: Routes = [
  ...
  { path: "eventDetails/:id", component: EventDetailsComponent },
  { path: "addEditEvent/:id", component: AddEditEventComponent },
  ...
```



* Save all the changes and try it out. You should be able to create and edit events. 


## Delete an event

* The last thing that we need to do in order to have a complete CRUD operational web app is to be able to delete an event. We will include the delete button in event-details.component.html.

```javascript
<div class="container">
  <div id="event-card-details" *ngIf="event">
    <div id="edit-event-btn">
      <button id="delete-btn" mat-raised-button color="warn" (click)="deleteEvent(event)" >
        Delete Event
      </button>

      <button mat-raised-button color="primary" [routerLink]="['/addEditEvent/', event.id]" >
        Edit Event
      </button>
      ...
```

* Then in the event.service.ts include the 'deleteEvent' method that will use the DELETE method to remove the event from the db. 

```javascript
  deleteEvent(id: string): Observable<any> {
    const headers = new HttpHeaders({
      "Content-Type": "application/json"
    });
    return this.http
      .delete(environment.apiURL + "events/" + id, { headers })
      .pipe(
        retry(3),
        catchError(this.handleError)
      );
  }
```

* Finally, in the event-details.component.ts add the delete event method.

```javascript
...
import { Router } from "@angular/router";

...

constructor(
  ...
  private router: Router
) {}
...
 ngOnInit() {
    const id = this.route.snapshot.params["id"];
    this.eventService.getEvent(id).subscribe((event: Event) => {
      console.log(event);
      this.event = event;
    });
  }

  deleteEvent(event: Event) {
    console.log(event);
    this.eventService.deleteEvent(event.id).subscribe(() => {
      console.log("Event Removed");
    });
    this.router.navigate(["/events"]);
  }
  ...
  ```


Save all the changes. The application should allow you to delete events now.


## Login and Signup

If you've done it properly, two things happened: 
* The creation of a new *signup* folder inside *login* folder (please check it yourself).
* A new import inside *login.module.ts* which looks like:

```javascript
//Modules
import { SharedModule } from "../shared/shared.module"; //<-- NEW
import { NgModule } from '@angular/core';
import { CommonModule } from '@angular/common';


//Components
import { LoginComponent } from './login.component';
import { SignupComponent } from './signup/signup.component'; //<-- NEW

@NgModule({
  declarations: [
    LoginComponent, 
    SignupComponent //<-- NEW
    ],
  imports: [
    CommonModule,
    SharedModule //<-- NEW
  ]
})
export class LoginModule { }
```

### User Service

```bash
ng g service core/user 
```

We will need the next methods:

* signup: To register a new user.
* login: To login into the app.
* logout: To logout.
* checkUser: To check if we are logged when we need to know it.

At the end, the service must be like next:

```javascript
import { Injectable } from "@angular/core";
import {
  HttpClient,
  HttpErrorResponse,
  HttpHeaders
} from "@angular/common/http";
import { Observable, throwError } from "rxjs";
import { catchError, retry, map } from "rxjs/operators";
import { environment } from "../../environments/environment";
import { User } from "../models/user";

@Injectable({
  providedIn: "root"
})
export class UserService {
  constructor(private http: HttpClient) {}
  isAuthenticated: boolean;

  signup(user: User): Observable<any> {
    const headers = new HttpHeaders({
      "Content-Type": "application/json"
    });

    return this.http
      .post(environment.apiURL + "users/", user, { headers })
      .pipe(
        retry(3),
        map(r => {
          localStorage.setItem("user", JSON.stringify(r));
          this.setUser();
        }),
        catchError(this.handleError)
      );
  }

  login(user: User): Observable<any> {
    const headers = new HttpHeaders({
      "Content-Type": "application/json"
    });

    return this.http.get(environment.apiURL + "users?email="+user.email, { headers }).pipe(
      retry(3),
      map(us => {
        if(us[0].email) {
          localStorage.setItem("user", JSON.stringify(us[0]));
          this.setUser();
          return us[0].password === user.password ? us[0] : 'Password not valid.'
        }
      }),
      catchError(this.handleError)
    );
  }

  logout() {
    localStorage.setItem("user", '');
    return false;
  }

  checkUser(): boolean {
    this.setUser();
    return this.isAuthenticated;
  }

  private setUser() {
    this.isAuthenticated = localStorage.getItem("user") ? true : false;
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

Like the *event* service (and all services) we have to declare it in a module, the *core* module in our case which is the module we've created to gather all services.

```javascript
import { NgModule } from "@angular/core";
import { CommonModule } from "@angular/common";
import { HttpClientModule } from "@angular/common/http";

import { EventService } from "./event.service";
import { UserService } from "./user.service"; // <-- NEW

@NgModule({
  declarations: [],
  imports: [CommonModule, HttpClientModule],
  providers: [EventService, UserService] // <-- NEW
})
export class CoreModule {}
```

Now, as we did with the event data, we need to create a data model for the *User*. Note that we already imported it into the *user.service.ts*.

```bash
ng g interface models/user
```

with this property:

```javascript
export interface User {
  id: string;
  email: string;
  password: string;
}
```

### Signup component

The *signup.component.ts* should look like:

```javascript
import { Component, OnInit } from '@angular/core';
import { FormBuilder, FormGroup } from "@angular/forms";
import { Router } from "@angular/router";
import { User } from "../../models/user";
import { UserService } from "../../core/user.service";

@Component({
  selector: 'oevents-signup',
  templateUrl: './signup.component.html',
  styleUrls: ['./signup.component.scss']
})
export class SignupComponent implements OnInit {
  signupForm: FormGroup;
  user: User;

  constructor(
    private fb: FormBuilder,
    private router: Router,
    private userService: UserService
  ) { }

  ngOnInit() {
    this.createForm();
  }

  createForm() {
    this.signupForm = this.fb.group({
      email: "",
      password: ""
    });
  }

  onSubmit() {
    this.user = this.signupForm.value;

    this.userService.signup(this.user).subscribe((event: Event) => {
      this.router.navigate(["/events"]);
    });
  }

}
```

In order to pick up the data from the user we need the HTML view like this:

```html
<div class="container">
  <h2>Signup</h2>

  <form class="form-signup" novalidate [formGroup]="signupForm" #fform="ngForm" (ngSubmit)="onSubmit()">
    <mat-form-field>
      <input matInput placeholder="Email" type="text" formControlName="email" />
    </mat-form-field>
    <mat-form-field>
      <input matInput placeholder="Password" type="password" formControlName="password" />
    </mat-form-field>

    <div class="login-btn">
      <div>Already have an account? <a routerLink="/login">Sign in</a></div>
      <button mat-raised-button color="primary" type="submit">
        Signup
      </button>
    </div>
  </form>
</div>
```

To show a minimum style we can set up the *signup.component.scss* like this, for example:

```css
.container {
  display: flex;
  justify-content: center;
  flex-direction: column;
  align-items: center;
  justify-content: center;
}

.form-signup {
  display: flex;
  flex-flow: column;
  width: 40%;
}
```

What we do need is set up the routing to this link in our *app-routing.module.ts*. To do this copy and paste the next snippets in their correct locations inside this file.

```javascript
...
import { SignupComponent } from "./login/signup/signup.component";
...
```

```javascript
...
{ path: "signup", component: SignupComponent },
...
```

> **_Side Note:_**  If you aren't sure about how to do it, you always can have a look at the app folder of this lab.


### Login component

The *login.component.ts* will be:

```javascript
import { Component, OnInit } from '@angular/core';
import { FormBuilder, FormGroup } from "@angular/forms";
import { Router } from "@angular/router";
import { UserService } from "../core/user.service";
import { User } from "../models/user";

@Component({
  selector: 'oevents-login',
  templateUrl: './login.component.html',
  styleUrls: ['./login.component.scss']
})
export class LoginComponent implements OnInit {
  loginForm: FormGroup;
  msgs: string;

  constructor(
    private fb: FormBuilder,
    private router: Router,
    private userService: UserService
  ) { }

  ngOnInit() {
    this.createForm();
  }

  createForm() {
    this.loginForm = this.fb.group({
      email: "",
      password: ""
    });
  }

  onSubmit() {
    this.userService.login(this.loginForm.value).subscribe((res: any) => {
      console.log(res)
      if(res.email) {
        this.router.navigate(["/events"]);
      } else {
        this.msgs = res;
      }
    }, err => this.msgs = 'Email not found.')
  }

}
```

Note how we manage the messages and errors that come from the service. If the email is not present in db.json, the service will throw an error and this will be processed by our subscription:

```javascript
// login.component.ts
...
}, err => this.msgs = 'Email not found.')
...
```

If the email is correct but not the password, then the service is returning *Password not valid*...

```javascript
// user.service.ts
...
return us[0].password === user.password ? us[0] : 'Password not valid.'
...
```

...and our component is showing the message...

```javascript
// login.component.ts
...
this.msgs = res;
...
```

...using the *msgs* variable present in the view as you can see in the next code snippet.

The *login.component.html*:

```html
<div class="container">
  <h2>Login</h2>

  <form class="form-login" novalidate [formGroup]="loginForm" #fform="ngForm" (ngSubmit)="onSubmit()">
    <mat-form-field>
      <input matInput placeholder="Email" type="text" formControlName="email" />
    </mat-form-field>
    <mat-form-field>
      <input matInput placeholder="Password" type="password" formControlName="password" />
    </mat-form-field>

    <div *ngIf="msgs" class="msgs">
      {{msgs}} <!-- Data binding for messages -->
    </div>

    <div class="login-btn">
      <div>Need an account? <a routerLink="/signup">Sign up</a></div>
      <button mat-raised-button color="primary" type="submit">
        Login
      </button>
    </div>
  </form>
</div>
```

Note that the messages only will be showed if the *msgs* variable has some value. This is done by the **ngIf* Angular directive.

```html
// login.component.html
...
<div *ngIf="msgs" class="msgs">
...
```

Also note the new *routerLink* binding:

```html
...
<div>Need an account? <a routerLink="/signup">Sign up</a></div>
...
````

The value */signup* match with our path in *app-routing.module.ts* previously configured.

Lastly the *login.component.scss*:

```css
.container {
  display: flex;
  justify-content: center;
  flex-direction: column;
  align-items: center;
  justify-content: center;
}

.form-login {
  display: flex;
  flex-flow: column;
  width: 40%;
}
```

With this new components we can sign up and login in the app... but before we need to add the *users* model to our db.json to be ready to create new users. To do this, add inside db.json and next to the *events" model an empty users array like this:

```json
{
  "events": [
    ... <-- Events objects
  ],
  "users": []
}
```

> **_Side Note:_**  The real authorization and authentication process is different from this as we already said before, but a suicide security risk is to store the password a plain view like we are doing in our db.json.

Now you already could try the login/signup process. The next step will be to create some mechanism to inform if the user is logged in or not.

### Knowing if the user is logged in

To inform the user if it is logged in we are going to change the toolbar to show the email (and an option to logout) when the user is logged in. When the user is logged out, we will show the usual *Login* link.

Logged out:

<p align="center">
    <img src="./resources/logged-out.png" width="350">
</p>


Logged in:

<p align="center">
    <img src="./resources/logged-in.png" width="450">
</p>

The *toolbar.component.ts* component will be:

```javascript
import { Component, DoCheck } from '@angular/core';
import { User } from "../models/user";
import { UserService } from "../core/user.service";
import { Router } from "@angular/router";

@Component({
  selector: 'oevents-toolbar',
  templateUrl: './toolbar.component.html',
  styleUrls: ['./toolbar.component.scss']
})
export class ToolbarComponent implements DoCheck {
  user: User;
  isAuthenticated: boolean;

  constructor(
    private router: Router,
    private userService: UserService
  ) { }

  ngDoCheck() {
    this.checkUser();
  }

  checkUser() {
    this.isAuthenticated = this.userService.checkUser();
    if(this.isAuthenticated) {
      this.user = JSON.parse(localStorage.getItem("user"));
    }
  }

  logout() {
    this.userService.logout();
    this.isAuthenticated = false;
    this.router.navigate(["/home"]);
  }

}
```

To complete the work, we need to change the *toolbar.component.html* view:

```html
<mat-toolbar color="primary">
  <a mat-button routerLink="/home" routerLinkActive="activeLink">Home</a>
  <a mat-button routerLink="/events" routerLinkActive="activeLink">Events</a>
  <a mat-button routerLink="/profile" routerLinkActive="activeLink">Profile</a>
  <a *ngIf="!isAuthenticated" mat-button routerLink="/login" routerLinkActive="activeLink">Login</a>
  <a *ngIf="isAuthenticated" mat-button (click)="logout()">{{user.email}} - Logout</a>
</mat-toolbar>
```

## Route Guards

The *app-routing.module.ts* will be now:

```javascript
import { NgModule } from "@angular/core";
import { Routes, RouterModule } from "@angular/router";

import { AuthGuard } from "./core/auth-guard.service"; // <-- NEW

import { LandingPageComponent } from "./landing-page/landing-page.component";
import { EventListComponent } from "./events/event-list/event-list.component";
import { ProfileComponent } from "./profile/profile.component";
import { LoginComponent } from "./login/login.component";
import { SignupComponent } from "./login/signup/signup.component";
import { PageNotFoundComponent } from "./page-not-found/page-not-found.component";
import { EventDetailsComponent } from "./events/event-details/event-details.component";
import { AddEditEventComponent } from "./events/add-edit-event/add-edit-event.component";

const routes: Routes = [
  { path: "home", component: LandingPageComponent },
  { path: "events", component: EventListComponent },
  { path: "login", component: LoginComponent },
  { path: "signup", component: SignupComponent },
  { path: "eventDetails/:id", component: EventDetailsComponent },
  { path: "addEditEvent/:id", component: AddEditEventComponent, canActivate: [AuthGuard] },  // <-- NEW

  { path: "", redirectTo: "/home", pathMatch: "full" },
  { path: "**", component: PageNotFoundComponent }
];

@NgModule({
  imports: [RouterModule.forRoot(routes)],
  exports: [RouterModule],
  providers: [AuthGuard] // <-- NEW
})
export class AppRoutingModule {}
```

Now we will create the *auth-guard.service.ts" file inside of the core folder like this:

```javascript
import { Injectable } from '@angular/core';
import { CanActivate, Router } from '@angular/router';
import { UserService } from "../core/user.service";


@Injectable()
export class AuthGuard implements CanActivate {

  constructor(
    public router: Router,
    private userService: UserService
  ) {}

  canActivate(): boolean {
    return this.checkLogin();
  }

  checkLogin(): boolean {
    if(this.userService.checkUser()) {
      return true;
    } else {
      this.router.navigate(['/login']);
      return false;
    }
  }

}
```

We realize that is an injectable service (note *@Injectable*) and we have to implement an interface for the *canActivate* guards:

```javascript
...
export class AuthGuard implements CanActivate {
...
```

## The Profile view

First, let's add the route and the guard to it (we don't want that unregistered people can see our profile). The new *app-routing.module.ts* will be:

```javascript
import { NgModule } from "@angular/core";
import { Routes, RouterModule } from "@angular/router";

import { AuthGuard } from "./core/auth-guard.service";

import { LandingPageComponent } from "./landing-page/landing-page.component";
import { EventListComponent } from "./events/event-list/event-list.component";
import { ProfileComponent } from "./profile/profile.component";
import { LoginComponent } from "./login/login.component";
import { SignupComponent } from "./login/signup/signup.component";
import { PageNotFoundComponent } from "./page-not-found/page-not-found.component";
import { EventDetailsComponent } from "./events/event-details/event-details.component";
import { AddEditEventComponent } from "./events/add-edit-event/add-edit-event.component";

const routes: Routes = [
  { path: "home", component: LandingPageComponent },
  { path: "events", component: EventListComponent },
  { path: "profile", component: ProfileComponent, canActivate: [AuthGuard] }, // <-- NEW
  { path: "login", component: LoginComponent },
  { path: "signup", component: SignupComponent },
  { path: "eventDetails/:id", component: EventDetailsComponent },
  {
    path: "addEditEvent/:id",
    component: AddEditEventComponent,
    canActivate: [AuthGuard]
  },

  { path: "", redirectTo: "/home", pathMatch: "full" },
  { path: "**", component: PageNotFoundComponent }
];

@NgModule({
  imports: [RouterModule.forRoot(routes)],
  exports: [RouterModule],
  providers: [AuthGuard]
})
export class AppRoutingModule {}
```

Now, the profile component files will be:

The profile.component.ts file;

```javascript
import { Component, OnInit } from '@angular/core';
import { UserService } from "../core/user.service";
import { User } from '../models/user';


@Component({
  selector: 'oevents-profile',
  templateUrl: './profile.component.html',
  styleUrls: ['./profile.component.scss']
})
export class ProfileComponent implements OnInit {
  user: User;

  constructor(private userService: UserService) { }

  ngOnInit() {
    this.getUser();
  }

  getUser() {
    this.user = JSON.parse(localStorage.getItem("user"));
  }

}
```

Look how we obtain the profile through the localStorage. We will always try to save HTTP calls to the API. The next files are easy to follow (we hope).

The profile.component.html file;

```html
<div class="container">
  <div class="userData">
    <div class="item">
      <h4>ID: </h4>
      <span> {{user.id}}</span>
    </div>

    <div class="item">
      <h4>Email: </h4>
      <span> {{user.email}}</span>
    </div>
  </div>
</div>
```

The profile.component.scss file;

```css
.container {
  display: flex;
  justify-content: center;
  align-items: center;
}

.userData {
  margin-top: 5rem;

  h4 {
    display: inline-block;
  }
}
```

<br/>


<p align="center">
    <img src="../boring-theory-1/resources/header.png">
</p>
