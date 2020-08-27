# ng-rdx-mat-spring-task

#Introduction
Recently I explored a consulting engagement with a company using the `React` / `Redux` / `Material` / `Spring Boot`; although I've been writing user experience software for a long time, other than a brief `React` assessment in '15, I had no experience with any of these technologies. 

Even though the engagement did not pan out, I decided to learn the `React` / `Redux` / `Material` / `Spring Boot`, found the [React.js and Spring Data REST](https://spring.io/guides/tutorials/react-and-spring-data-rest/) blog, and decided to use it as the basis for me to build a `Todo` application using these technologies. I started with `React` and `Spring`, which is the basis of the blog, then integrated `Redux`, then `Material UI`, and finally I added a `CI/CD` that deploys my [Todo application](https://github.com/RodrigoMattosoSilveira/react-rdx-mat-spring-todo) to [Heroku](https://react-springboot-todo.herokuapp.com//); there are two users: `donald / trump` and `nancy / pelosi`.

Once I completed my `Todo` application I decided to migrate it to `Angular` / `Redux` / `Material` / `Spring Boot`; although the idea seems simple, I learned that integrating `Angular` and `Spring Boot` is more complicated:
* **Project Structure** - The application requires two modules, `frontend` and `backend`, managed by technolgies with different views about file placement;
* **Technology Cooperation** - We use `angular-cli` to manage the `frontend` and `maven` the backend; they rely on `Node.js` and `Tomcat`, respectively, to build and serve the `frontend` and `backend`; their workflows are also different;
* **NGRX** - Although the principles are the same, I learned about `selectors` and `effects`, both available in `React` but which I did not use, which have deep architectural implications;
* **Angular Material** - It is very different than `Material UI`, hence forcing us to rewrite the UI; although this seems hard, it a welcome opportunity to de-couple data accesss and filtering logic from the components;

##Project Structure
I invest a fair amount of time looking for a `natural` way to use `angular-cli` and `spring-initializr` to create the `frontend` and `backend` modules. Eventually I decided to adopt the framework suggested in the [Integrate Angular with a Spring Boot project](https://keepgrowing.in/java/springboot/integrate-angular-with-a-spring-boot-project/):
````text
|- <project root>
   |- backend
   |- frontend
   ...
````
We use `angular-cli` and `spring-initializr`, move files around, and glue them using the wonderful [frontend-maven-plugin](https://github.com/eirslett/frontend-maven-plugin);

##Technology Cooperation
We will address this based on the workflow:
* **Development** - We will work on multiple terminal windows, each handling its own work using the right tools for it; we will rely on `ng` to build and test the `frontend` and `mvn` to build and test the `backend`; we wil rely on `Node,js` to serve the `frontend` and `Tomcat` the backend, and will use `CORS` to mitigate the same-origin issue that would arise from this setup; 
* **Deployment** - We will use the `Maven` `frontend-maven-plugin` to drive the `frontend` build and test, while still relying on `Node.js` to support these task, and use `mvn` to build and test the backend, build the jar;
* **Production** - `Tomcat` will host the `backend` and serve the `frontend`;

##NGRX
* **Selectors** - We will use `selectors` to minimize the amount of state data, constraining ourselves to only raw data, using selectors to compute derived data, and removing these operations from components. An example is a component that renders `tasks` with the functionality to filter them by state; by setting task list and the task state as simple states, we can build a selector that filters `completed` tasks and de-coupling this logic from the component, also making it easier to test it;
* **Effects** - We will use `effects` to execute logic required prior, `REST call` or after event, `WEBSOCKET event`, again decoupling from `actions`, `reducers`, and `components, and, again, simplfying logic and testing;

##Angular Material
My original `Todo` application was a tour de force, quite ugly! We will strive for a more elegant approach.

#Project Structure
We will create the `backend` and `frontend`, grooming them to fit our target structure;
##Project root folder
Create the `project root folder`
````shell script
$ cd ~/projects
$ mkdir ng-rdx-mat-spring-task
````
Notes:
1. I'm a `MAC` user;
1. I keep all my projects on my `home's project folder`;
1. I named my `project root folder` `ng-rdx-mat-spring-task`;
1. Note that I used `tssk` instead of `todo` to overcome my IDE use of `todo` as a reserved word;

##Project repository
We will commit our work as we go along, you must have `git` installed:
````shell script
$ echo "# ng-rdx-mat-spring-task" >> README.md
$ git init
$ git add README.md
$ git commit -m "first commit"
$ git remote add origin git@github.com:RodrigoMattosoSilveira/ng-rdx-mat-spring-task.git
$ git push -u origin master
````

This yields the following:
````text
|- ng-rdx-mat-spring-task
   |- ./
   |- ../
   |- .git/
   README.md
````
Notes:
1. I used `ng-rdx-mat-spring-task` as my `project root name`;
1. My `GitHub` account is `https://github.com/RodrigoMattosoSilveira`; you have to substitute it for yours;

##Backend
###Spring Boot Structure
I'll use my IDE, IntelliJ IDEA, to create the `backend` module for me, as follows; 
   1. Module SDK: 
      1. Project SDK 14
   1. Project Settings:
      1. Default: https://start.spring.io
      1. Group: com.madronetek
      1. Java version: 14
   1. Dependencies <we will use them with the application>:
      1. Developer Tools: Spring Boot DevTools
      1. Web: Spring Web
      1. Template Engines: Thymeleaf
      1. Security: Spring Security
      1. SQL: H2 Database
   1. Location:
      1. Module Name: backend
      1. Content root<filled up by the IDE>: ~/projects/ng-rdx-mat-spring-task/backend
      1. Module file location: <filled up by the IDE>: ~/projects/ng-rdx-mat-spring-task/backend
 
The file structure after the configuration is:
```text
|- ng-rdx-mat-spring-task
   |- ./
   |- ../
   |- .git/
   |- backend/
      |- .mvn/
      |- src/
         |- main
            |- java
               |- com
                  |- madronetek
                     |- task
                        |- TaskApplication.java
            |- resources
               |- static/
               |- templates/
               |- application.properties
         |- test
            |- java
               |- com
                  |- madronetek
                     |- task
                        |- TaskApplicationTests.java
      |- .gitignore
      |- backend.iml
      |- HELP.md
      |- mvnw
      |- mvnw.cmd
      |- pom.xml
  |- README.md
````

Notes:
1. If not using your IDE,you can navigate to [Spring Boot Initializer](https://start.spring.io/), create it, save the `zip` file to your sandbox, expand the `zip` file and copy its content to your `project root folder`;
1. I used `com.madronetek` as the group name, you can pick your own; 

#Links
##Blogs
  * [Building a Web Application with Spring Boot and Angular](https://www.baeldung.com/spring-boot-angular-web);
  * [How to structure a spring boot project](https://springhow.com/spring-boot-project-structure-and-convention/#:~:text=How%20to%20structure%20a%20spring%20boot%20project%3F%201,template%20engines%20by%20default.%20...%206%20pom.xml.%20) - The Spring Boot project folder convention;
  * [Introduction to Reactive Programming](https://dzone.com/articles/introduction-to-reactive-programming-2);
  * [Integrate Angular with a Spring Boot project](https://keepgrowing.in/java/springboot/integrate-angular-with-a-spring-boot-project/) - This repository is based on this blog;
  * [React.js and Spring Data REST](https://spring.io/guides/tutorials/react-and-spring-data-rest/) - The blog I based my `React` `Todo` project;
  * [React Redux Mat Spring Todo](https://github.com/RodrigoMattosoSilveira/react-rdx-mat-spring-todo/) - My `React` `Todo` project;
  * [React Redux Mat Spring Todo Heroku](https://react-springboot-todo.herokuapp.com/login)

##Technologies
  * [Anguar](https://angular.io/) - Application design framework and development platform for creating efficient and sophisticated single-page apps;
  * [Angular-CLI](https://angular.io/cli) - A command-line interface tool that you use to initialize, develop, scaffold, and maintain Angular applications directly from a command shell;
  * [Angular Material](https://material.angular.io/) - Material Design components for Angular;
  * [frontend-maven-plugin](https://github.com/eirslett/frontend-maven-plugin) - Maven plugin to support frontend development, leveraging Node.js, while decoupling it from the backend;
  * [git](https://git-scm.com/) - A free and open source distributed version control system designed to handle everything from small to very large projects with speed and efficiency;
  * [GitHub](https://github.com/) -A development platform inspired by the way you work; you can host and review code, manage projects, and build software alongside 50 million developers.
  * [Heroku](https://www.heroku.com/) - Platform as a service based on a managed container system, with integrated data services and a powerful ecosystem, for deploying and running modern apps;
  * [Material UI]() - Material Design components for React;
  * [Maven](https://maven.apache.org/) - A software project management and comprehension tool;
  * [ng](https://angular.io/cli) - Maven's counterpart in `angular-cli`;
  * [ngrx](https://ngrx.io/) - Reactive State for Angular;
  * [Node.js](https://nodejs.org/en/) - JavaScript runtime built on Chrome's V8 JavaScript engine.
  * [React](https://reactjs.org/) - A JavaScript library for building user interfaces;
  * [Redux](https://redux.js.org/) - A Predictable State Container for JS Apps;
  * [react-redux](https://react-redux.js.org/) - Official React bindings for Redux;
  * [Reselect](https://github.com/reduxjs/reselect) - Simple “selector” library for Redux (and others) inspired by getters in NuclearJS, subscriptions in re-frame and this proposal from speedskater;
  * [Spring Boot](https://spring.io/projects/spring-boot) - Makes it easy to create stand-alone, production-grade Spring based Applications that you can "just run";
  * [Spring Boot Initializer](https://start.spring.io/) - Creates the basic Spring Boot project framework;
  * [Tomcat](http://tomcat.apache.org/) - open source implementation of the Java Servlet, JavaServer Pages, Java Expression Language and Java WebSocket technologies;

