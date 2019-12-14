<p align="center">
    <img src="../../boring-theory-1/resources/header.png">
</p>

# Lab 01 - Starting a New Angular Project

## Objectives and Outcomes

- Create an Angular Project.
- Install Angular Material.
- Using a Material component.


## Creating a new Angular App

### First Steps

#### Step 1. Create the project
```sh
ng new open-events-front --style=scss --prefix=oevents
```

<br/>

#### Step 2. Execute the project
```sh
ng serve
```

[http://localhost:4200](http://localhost:4200)

<br/>

## Project Folder Structure

<br/>

## Create a new component

```sh
ng generate component landing-page
```

### Component Structure

### Changing the view

Delete the content from *landing-page.component.html* and copy and paste the following code on it:
```javascript
<div class="container">
  <div class="title">
    <h1>Open Events App</h1>
  </div>
  <div class="subtitle">
    <h2>All our events</h2>
  </div>
  <div class="message">
    <p>
      A new and fresh application to host all our tech events crafted from
      Accenture Openathons
    </p>
  </div>
</div>

```
Now, we want to change the styles. Copy and paste the following code in *landing-page.component.scss*

```javascript
.container {
  display: flex;
  flex-flow: column;
  text-align: center;
}
.title,
.subtitle,
.message {
  padding: 1rem;
}
.title {
  color: #8bc34a;
}
.subtitle {
}
.message {
}

```

Finally, Delete the content from *app.component.html* (remember, our project **Root Component**) and copy and paste the following code on it.

```javascript
<div class="body">
  <oevents-landing-page></oevents-landing-page>
</div>

```

## Angular Material 

```sh
 npm install @angular/material @angular/cdk @angular/animations hammerjs
```

<br/>

We want also to use **Material Design Icons**. Edit *index.html* (remember, our project **Main View**) file and include the following into the **head** section:

```javascript
...
<head>
...
<link href="https://fonts.googleapis.com/icon?family=Material+Icons" rel="stylesheet">
</head>
...

```

We are going to add now some nice Angular Materials components so we need to import them into *app.module.ts* again:

```javascript
...
import { BrowserAnimationsModule } from '@angular/platform-browser/animations';
import { MatToolbarModule } from '@angular/material/toolbar';
import { MatButtonModule } from '@angular/material/button';
import { MatListModule } from '@angular/material/list';
import 'hammerjs';

@NgModule({
 ...
  imports: [
    ...
    BrowserAnimationsModule,
    MatToolbarModule,
    MatButtonModule,
    MatListModule 
  ], 
...

```

### Adding a toolbar
Now let’s add a toolbar in our application. Create a new component named **toolbar**:
```sh
 ng generate component toolbar
```
Now, edit *toolbar/toolbar.component.html* and delete its content and add the following:
```javascript
<mat-toolbar>
  <a mat-button>Home</a>
  <a mat-button>Events</a>
  <a mat-button>Profile</a>
  <a mat-button>Login</a>
</mat-toolbar>
```

Finally, all we need is to tell Angular that we want this toolbar in the landing page: edit *app.component.html* and add oevents-toolbar.

```javascript
<div class="body">
  <oevents-toolbar></oevents-toolbar>
  <oevents-landing-page></oevents-landing-page>
</div>

```

<br/>

<p align="center">
    <img src="../../lab-01/resources/landingPageToolbar.png">
</p>


### Adding a theme
Import the following style into *src/styles.scss* (the global styles file):

```javascript
@import '@angular/material/prebuilt-themes/indigo-pink.css';

```
And edit *toolbar/toolbar.component.html* to apply the primary color:

```javascript
<mat-toolbar color="primary">
...

```
<br/>


<p align="center">
    <img src="../../boring-theory-1/resources/header.png">
</p>
