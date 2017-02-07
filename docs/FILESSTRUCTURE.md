# File Structure Outline
Below is a the file tree structure for both the **Backend** and the **Frontend**.
I will reference this file structure in future parts of this book.


## Backend
### Directory Structure
**data**:

**lib**: The _lib_ directory contains two types of files: middleware, and supporting functionality.
* Middleware functions (typically named `*-middleware.js` and is specific to _Express_) are functions that have access to the request object (req), the response object (res), and the next middleware function in the application’s request-response cycle. The next middleware function is commonly denoted by a variable named `next();`. [<sup>express docs</sup>](http://expressjs.com/en/guide/writing-middleware.html) [<sup>express docs</sup>](http://expressjs.com/en/guide/using-middleware.html)
* Supporting Functionality do not typically have access to the request/response objects like middleware and are used to abstract away functionality that needs to be called repeatedly throughout an app. For instance: `fuzzy-query` is used in both `gallery-router.js` and `pic-router.js` so instead of writing it's functionality into the router as part of the promise chain it is abstracted out.

**model**: The _model_ directory contains the mongoose/MonogoDB Schemas.
* Schemas are provided by MongooseJS and maps to a MongoDB collection and defines the shape of the documents within that collection. [<sup>Mongoose docs</sup>](http://mongoosejs.com/docs/guide.html)
* [MongoDB collection](https://docs.mongodb.com/manual/reference/glossary/#term-collection) is a grouping of MongoDB documents. A collection is the equivalent of an relational database management system (RDBMS a.k.a. SQL) table. A collection exists within a single database. Collections do not enforce a schema. Documents within a collection can have different fields. Typically, all documents in a collection have a similar or related purpose. [<sup>MongoDB docs</sup>](https://docs.mongodb.com/manual/core/data-modeling-introduction/)

**route**: The _route_ directory contains all of the routes logic or any required express routes used by the api.
* Routing refers to determining how an application responds to a client request to a particular endpoint, which is a URI (or path) and a specific HTTP request method (GET, POST, and so on). Each route can have one or more handler functions, which are executed when the route is matched. Typically each router should line up with each schema in the model directory. [<sup>express docs</sup>](http://expressjs.com/en/starter/basic-routing.html)

**test**: The _test_ directory is where all the test files are located.
* Test files are typically named `*-test.js`

**test/data**: The _test/data_ directory contains any mock data that may be used within a test.
* Typically the mock data that contained in the _test/data_ directory is static and does not contain any functions or logic. In the slugram app the only file located in the _test/data_ directory is an image used specifically for testing in `pic-router-test.js`.

**test/lib**: The _test/lib_ directory contains mocking files used to set up data before testing is run.
* Mocking files serve a similar function to middleware and can be required in anywhere and are used to set up and manipulate data prior to the tests running. An example of this would be the `test-env.js` file which sets some fake env variables that will be needed later in the testing.


#### Example
```
├── data
├── lib
│   ├── basic-auth-middleware.js
│   ├── bearer-auth-middleware.js
│   ├── error-middleware.js
│   ├── fuzzy-query.js
│   ├── fuzzy-regex.js
│   ├── item-query-middleware.js
│   ├── page-query-middleware.js
│   └── s3-upload-promise.js
├── model
│   ├── gallery.js
│   ├── pic.js
│   └── user.js
├── npm-debug.log
├── package.json
├── route
│   ├── auth-router.js
│   ├── gallery-router.js
│   └── pic-router.js
├── server.js
└── test
    ├── auth-router-test.js
    ├── data
    │   └── shield.png
    ├── error-middleware-test.js
    ├── fuzzy-query-test.js
    ├── fuzzy-regex-test.js
    ├── gallery-router-test.js
    ├── lib
    │   ├── aws-mocks.js
    │   ├── clean-db.js
    │   ├── everything-mock.js
    │   ├── gallery-mock.js
    │   ├── mock-many-gallerys.js
    │   ├── mock-many-pics.js
    │   ├── mock-many-users.js
    │   ├── pic-mock.js
    │   ├── server-ctrl.js
    │   ├── test-env.js
    │   └── user-mock.js
    ├── pic-router-test.js
    └── s3-upload-promise-test.js
```

## Front End
### Directory Structure
**app**: The _app_ directory is the root directory for any angular application. The _app_ directory will contain all of the project files and must include the `index.html - (required name)` file as well as the `entry.js - (non-required name)` file from which the app will be built. The directory does not need to be named app but for the sake of making it clear what the folder contains app is used since it is the most semantic name. [<sup>scotch.io article</sup>](https://scotch.io/tutorials/angularjs-best-practices-directory-structure#a-better-structure-and-foundation)

**app/assets**: The _app/assets_ directory is used to hold any static files (i.e. images, videos, etc.) that need to be accessed by the application. The directory can contain any number of sub-directories as required. It is important to keep the directory structure semantic since these files will be used throughout the application.

**app/component**: The _app/component_ directory is specifically intended to contain the all of the component files. There are several ways in which to organize the component directory and it will differ from project to project. One best practice suggestion is to [treat each component as a mini Angular app](https://scotch.io/tutorials/angularjs-best-practices-directory-structure#a-better-structure-and-foundation) where all the services, controller, view, and style files all live within their own named sub-directory in the component directory. This works particularly well if there are components within the application that are the sole consumers of a specific service (i.e. login or sigup).

Alternatively the app I built is structured slightly differently with components being being split into sub-directories with potentially more sub-directories inside of those. The intention is to break down each component into very small, moduler pieces that include only the necessary code to make each component work. _**At it's base level each controller should contain 3 files, a `JS` file for logic, an `HTML` file that where the component will be used, and a `SCSS/CSS` file for controlling styles only on that controller.**_

[scotch.io article on best practices for Angular app file structure](https://scotch.io/tutorials/angularjs-best-practices-directory-structure)

**app/config**: The _app/config_ directory should only config files. Config files get executed during the provider registrations and configuration phase. Only providers and constants can be injected into configuration blocks. This is to prevent accidental instantiation of services before they have been fully configured. [<sup>angular docs</sup>](https://docs.angularjs.org/guide/module#module-loading-dependencies)

In the case of the application I built the config files set up and export the routes available to the application.

**app/scss**: The _app/scss_ directory holds all of the generic (i.e. non-controller) specific styling for the application. This directory can contain as many sub-directories but typically a [SMACSS](https://smacss.com/) type structure is a good rule to follow.

**app/service**: The _app/service_ directory should only contain the services files. However, based on how large the application is or depending on design preference it is reasonable to not have a service file. Alternatively it's also reasonable to put globally used services in an _app/service_ directory while all other controller/component specific services are placed in the same folder as the controller/component that consumes that file.

[scotch.io article on best practices for Angular app file structure](https://scotch.io/tutorials/angularjs-best-practices-directory-structure)

**app/view**: The _app/view_ directory should hold the files needed to run each view. Views should line up with the routes specified in the files located within the _app/config_ directory. The _app/view_ directory can contain as many sub-directories as required to account for all routes. _**At it's base level each view should contain 3 files, a `JS` file for logic, an `HTML` file for that view (typically this will consume components), and a `SCSS/CSS` file for controlling styles only on that view.**_

**test**: The _test_ directory for Angular serves the same purpose as it does for the backend api. It's important to note that the test directory should not be included in the _app_ directory but should be instead located at the root of the repo.


#### Example
```
├── app
│   ├── assets
│   │   └── img
│   │       └── cf-logo.png
│   ├── component
│   │   ├── gallery
│   │   │   ├── create-gallery
│   │   │   │   ├── _create-gallery.scss
│   │   │   │   ├── create-gallery.html
│   │   │   │   └── create-gallery.js
│   │   │   ├── edit-gallery
│   │   │   │   ├── _edit-gallery.scss
│   │   │   │   ├── edit-gallery.html
│   │   │   │   └── edit-gallery.js
│   │   │   ├── gallery-item
│   │   │   │   ├── _gallery-item.scss
│   │   │   │   ├── gallery-item.html
│   │   │   │   └── gallery-item.js
│   │   │   ├── thumbnail
│   │   │   │   ├── _thumbnail.scss
│   │   │   │   ├── thumbnail.html
│   │   │   │   └── thumbnail.js
│   │   │   ├── thumbnail-container
│   │   │   │   ├── _thumbnail-container.scss
│   │   │   │   ├── thumbnail-container.html
│   │   │   │   └── thumbnail-container.js
│   │   │   └── upload-pic
│   │   │       ├── _upload-pic.scss
│   │   │       ├── upload-pic.html
│   │   │       └── upload-pic.js
│   │   ├── landing
│   │   │   ├── login
│   │   │   │   ├── _login.scss
│   │   │   │   ├── login.html
│   │   │   │   └── login.js
│   │   │   └── signup
│   │   │       ├── _signup.scss
│   │   │       ├── signup.html
│   │   │       └── signup.js
│   │   └── nav-bar
│   │       ├── _navbar.scss
│   │       ├── navbar.html
│   │       └── navbar.js
│   ├── config
│   │   ├── log-config.js
│   │   └── router-config.js
│   ├── entry.js
│   ├── index.html
│   ├── scss
│   │   ├── lib
│   │   │   ├── base
│   │   │   │   ├── _global.scss
│   │   │   │   ├── _reset.scss
│   │   │   │   └── _var.scss
│   │   │   ├── layout
│   │   │   └── theme
│   │   │       └── mvp
│   │   │           ├── _mvp-buttons.scss
│   │   │           ├── _mvp-input.scss
│   │   │           └── _mvp.scss
│   │   └── main.scss
│   ├── service
│   │   ├── auth-service.js
│   │   ├── gallery-service.js
│   │   └── pic-service.js
│   └── view
│       ├── home
│       │   ├── _home.scss
│       │   ├── home-controller.js
│       │   └── home.html
│       └── landing
│           ├── _landing.scss
│           ├── landing-controller.js
│           └── landing.html
├── karma.conf.js
├── package.json
├── test
│   ├── auth-service-test.js
│   ├── create-gallery-component-test.js
│   ├── edit-gallery-component-test.js
│   ├── gallery-item-component-test.js
│   └── gallery-service-test.js
└── webpack.config.js
```
