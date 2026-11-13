# Frontend - Setup and basic Angular example

## Setup

### Nvm

```bash
sudo apt install curl
curl https://raw.githubusercontent.com/creationix/nvm/master/install.sh | bash
nvm -v
```

to clean :

```bash
# remove "export NVM_DIR" from .bashrc and reload it

rm -rf ~/.nvm
rm -rf ~/.npm
rm -rf ~/.bower
```

### Npm 

```bash
sudo apt install npm
npm --version

npm config set proxy http://username:password@host:port
npm config set https-proxy http://username:password@host:port
# see config in ~/.npmrc
```

### Node JS

```bash
nvm current node -v
nvm install 14
nvm use 14
node -v

# clean non required versions
# ~/.nvm/versions/node$
```

if not installed, go to https://nodejs.org/en 
- download archive
- then add node path to your PATH
```bash
export NODEJS=/home/USER/apps/node-v18.15.0-linux-x64/bin
export PATH=$JAVA_HOME/bin:$NODEJS:$PATH
```

### Yarn

```bash
npm install --global yarn
yarn install --ignore-engines

# check installation
yarn --version

# connect to vpn
yarn config set proxy http://username:password@host:port
yarn config set https-proxy http://username:password@host:port
yarn config list
```

### Angular Cli

Install it globally using NPM
```bash
sudo npm install -g @angular/cli@latest
# check installation
ng version
```

### Visual Studio Code

See https://code.visualstudio.com

## Angular basics

* create project : go to project folder (use defaults)
```bash
ng new <project name>
```

* launch project in visual studio code
```bash
cd <project name>
code .
```

* add component to project
```bash
ng generate component <component name>
```

* build project
```bash
ng serve
```

* launch it : http://localhost:4200

* check  angular version of a project
Go into node_modules/@angular/core/package.json and check version field.

## Angular example : declaring a form

* hello.component.ts
```typescript
import { Component, OnInit } from '@angular/core';
import { FormControl } from '@angular/forms';
import { FormBuilder, FormGroup } from '@angular/forms';


@Component({
  selector: 'app-hello',
  templateUrl: './hello.component.html',
  styleUrls: ['./hello.component.css']
})
export class HelloComponent implements OnInit {

  userForm: FormGroup = new FormGroup({
    name: new FormControl(''),
    email: new FormControl(''),
  });  

  constructor(private formBuilder: FormBuilder) {  }

  ngOnInit(): void {
    this.userForm = this.formBuilder.group(
      {
        name: '',
        email: '',
      }
    );
  }

  onSubmit() {
    console.log('submitUserForm clicked');
    console.log(JSON.stringify(this.userForm.value, null, 2));

    var formData = this.userForm.value;
    console.log('name = ' + formData['name'] + ' - email = ' + formData['email']);
  }

  onShow() {
    console.log('Show button clicked');
  }

  onDivClick() {
    console.log('div clicked');
  }
}
```

* hello.component.html
```html
<h2>Hello Component Page</h2>

<div>    
    <form [formGroup]="userForm" (ngSubmit)="onSubmit()">
        <label>name:</label>
        <input id="name" type="text" formControlName="name">
        <label>email:</label>
        <input id="email" type="text" formControlName="email">
        <button class="button" type="submit">submit</button>
    </form>
</div>
<br/><br/>
<div (click)="onDivClick()">
    <button (click)="onShow()">Show</button>
</div>
```

* app.component.html
```html
<div>
  <app-hello></app-hello>
</div>
```

* app.module.ts
```typescript
import { NgModule } from '@angular/core';
import { BrowserModule } from '@angular/platform-browser';
import { FormsModule, ReactiveFormsModule } from '@angular/forms';

import { AppRoutingModule } from './app-routing.module';
import { AppComponent } from './app.component';
import { HelloComponent } from './hello/hello.component';

@NgModule({
  declarations: [
    AppComponent,
    HelloComponent
  ],
  imports: [
    BrowserModule,
    AppRoutingModule,
    FormsModule,
    ReactiveFormsModule
  ],
  providers: [],
  bootstrap: [AppComponent]
})
export class AppModule { }
```

sources :  
https://www.bezkoder.com/angular-15-form-validation/  
https://angular.io/start/start-forms  
https://www.tutorialsteacher.com/angular
