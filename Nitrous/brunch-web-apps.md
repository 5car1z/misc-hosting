# Getting Started with Brunch 

![Brunch Logo](http://i.imgur.com/aePKLBU.png)

<div class=”table-of-contents”>
<h3>Table of Contents</h3>
<ul>
    <li><a href=”#introduction”> Introduction</a></li>
    <li><a href=”#1”> 1 - Create a Nitrous Project</a></li>
    <li><a href=”#2”> 2 - Install Brunch</a></li>
    <li><a href=”#3”> 3 - Create a Basic Brunch Project</a></li>
    <li><a href=”#4”> 4 - Setup the Project Files</a></li>
    <li><a href=”#5”> 5 - Install Brunch Plugins</a></li>
    <li><a href=”#6”> 6 -  Build the Project Files</a></li>
    <li><a href=”#7”> 7 - Watching Changes and Test Server</a></li>
    <li><a href=”#8”> 8 - Using Brunch Skeletons</a></li>
    <ul>
	     <li><a href=”#react-skeleton”> React Skeleton</a></li>
	     <li><a href=”#chaplin-skeleton”>Chaplin Skeleton</a></li>
	     <li><a href=”#more-skeletons”> More Skeletons</a></li>
	</ul>   
	<li><a href=”#9”> 9 - Nitrous Quickstarts (Optional)</a></li>
    <li><a href=”#conclusion”> Conclusion</a></li>
</ul>

<h2 id=”introduction”>Introduction</h2>

[Brunch](http://brunch.io/) is a powerful HTML5 build tool that can help you setup and maintain a productive workflow for front-end development. If you prefer using SASS over CSS, if you want to use functionality only available in the new ES6 JavaScript spec, or if you want to enforce conventions using code linters, then Brunch is for you.

Brunch will help you:

* Compile your scripts, templates, and styles.
* Lint and wrap the scripts/templates into common.js or AMD modules.
* Concatenate the scripts and styles.
* Copy assets and static files automatically. 
* Watch your files for changes.
* Notify you of any errors.

In this tutorial we will implement a simple Brunch demo application to get a feel for the layout and structure of a project. Then we will show you how to use skeletons to begin building and creating applications that use familiar frameworks.

<h2 id=”1”> 1 - Create a Nitrous Project</h2>

Create a new project from within your [Nitrous workstation](https://community.nitrous.io/docs/setup-a-dev-environment) to get started. 

This is found on the default “Projects” tab - indicated by the `+` symbol inside of the square. Click it to be taken to the “New Project” creation page. 

![Projects Image](http://i.imgur.com/oEdgpDF.png)

Name your project and select the "Node.js" option from the available Nitrous templates.

![Node.js Project Creation Image](http://i.imgur.com/4FQol5o.png)

Once the project is created and ready, launch the Nitrous IDE. 

<h2 id=”2”> 2 - Install Brunch</h2>

It's very easy to install Brunch and only takes around a minute or two. 

Use Node Package Manager (npm) which comes as part of this Node.js project template, to install Brunch.

```command
npm install -g brunch
```

Confirm the installation was successful after the processing completes by checking for Brunch's version number.

```command
brunch -v 
```

The successful output means Brunch is installed and ready!

``` Output
2.6.4
```

<h2 id=”3”> 3 - Create a Basic Brunch Project</h2>

To demonstrate the structure and workflow of a Brunch project we're creating a simple demo application that uses a static HTML page, a CSS style-sheet (using [SASS](http://sass-lang.com/)), and a basic ES3/ES5 JavaScript file. By creating/completing these tasks, we are working through the different areas of Brunch as we go along. 

First, create the project directories and base configuration files using the `new` command plus a suitable name for the project. 

```command
brunch new brunch-demo-app
```

We called ours `brunch-demo-app` so we will refer to it as that in future commands. 

There are other options and possibilities available with this creation command, but we'll come back and explore them later on in the tutorial. 

The output you receive explains what the previous Brunch command does. 

``` Output
07 Apr 13:41:18 - log: Cloning git repo "git://github.com/brunch/dead-simple.git" to "/home/nitrous/.brunch/skeletons/c76c330b8d54d134fca1e696f29bfc30993e5fd1"...
07 Apr 13:41:19 - log: Cloned "git://github.com/brunch/dead-simple.git" into "/home/nitrous/.brunch/skeletons/c76c330b8d54d134fca1e696f29bfc30993e5fd1"
07 Apr 13:41:19 - log: Created skeleton directory layout
07 Apr 13:41:19 - log: Installing packages...
```

Brunch uses a project skeleton sourced from a Git repository on [GitHub](https://github.com/) to build the base configuration files. Whenever a project is created without any extra options or parameters, this "dead-simple" skeleton is used. The default skeleton does not add any extra libraries or frameworks for us. 

Move into your new Brunch project by changing directory.

```command
cd brunch-demo-app
```

And take a look at the files.

```command
ls -la 
```

Here are the Brunch generated files you're seeing in your output - but from the top down in a tree style view:

```
├── app
│   ├── assets
│   │   ├── index.html
│   │   └── README.md
│   ├── initialize.js
│   └── README.md
├── brunch-config.js
├── package.json
└── README.md
```

>**Note:** The `node_modules` directory and its contents have been omitted from the above layout.

`app` holds the *source codebase* of the project - scripting files, style sheets, template files, etc that are to be compiled by Brunch are stored in here. 

`assets` has its contents copied recursively into the target public folder, without any processing or modifications. 

`public` which hasn't been generated yet is the default target folder for build processes, and has content served from it when needed. 

The next step covers editing and adapting these files for use in the demo web app. 

<h2 id=”4”> 4 - Setup the Project Files</h2>

Using the [Nitrous IDE](https://community.nitrous.io/docs/ide-overview) open up the HTML file located at: `app/assets/index.html` 

Replace the entirety of the code inside for the code shown below in the next snippet:

``` app/assets/index.html
<!doctype html>
<html>
<head>
  <meta charset="utf-8">
  <title>Simple Brunch Demo</title>
  <link rel="stylesheet" href="app.css">
</head>
<body>
  <h1>
    Brunch
    <small>-- A Simple Demo</small>
  </h1>
  <img src="http://i.imgur.com/x2hOV2y.png" alt="Nitrous Logo" height="150" width="150">
  <p>Lorem ipsum dolor sit amet, consectetur adipisicing elit.</p>
  <script src="app.js"></script>
  <script>require('initialize').init();</script>
</body>
</html>
```

These changes add new information to the page but more importantly now reference the style-sheet file for the CSS, and the JavaScript file that we are rewriting next. 

Save these changes to the file and continue. 

The JavaScript file is stored at: `brunch-demo-app/app/initialize.js`

Similar to before - copy over the code in the next code snippet to replace the previous contents of the JavaScript file. 

``` app/initialize.js
"use strict";

var App = {
  init: function init() {
    console.log('App initialized.');
  }
};

module.exports = App;
```

With these additions we can easily verify the JS is working correctly once the demo app is running (later on). 

Remember to save the file after making these changes. 

The next set of alterations for the CSS require a new directory and file to be created. Do this with the following commands:

```command
mkdir ~/brunch-demo-app/app/styles
touch ~/brunch-demo-app/app/styles/main.scss
```

Open up the new `brunch-demo-app/app/styles/main.scss` file in the Nitrous IDE and add in the SASS code provided below:

``` app/styles/main.scss
@import url(http://fonts.googleapis.com/css?family=Roboto+Slab);

$default-bg: lightblue;
$default-text: black;

$roboto-slab: 'Roboto Slab';

body {
  font-family: $roboto-slab;
  font-size: 16px;
  background: $default-bg;
  color: $default-text;
}

h1 {
  font-size: 2em;
  margin: 0.5em 0 1em;

  & > small {
    color: red;
    font-size: 70%;
  }
}
```

Make sure to save the new content in this file before moving on.

The final file to take into consideration here is the `brunch-config.js` file at the root of the project. This holds the the main configuration settings for how Brunch will operate. It can be written in [CoffeScript](http://coffeescript.org/) or plain JS depending upon the developer's preference. 

Previewing apps on Nitrous requires the server process to be run on an open port, as well as the `0.0.0.0` host address. 

To make sure the [server preview](https://community.nitrous.io/docs/preview-your-app) works later on, replace the code in this `brunch-config.js` file with this supplied code:

``` brunch-config.js
module.exports = {
  files: {
    javascripts: {joinTo: 'app.js'},
    stylesheets: {joinTo: 'app.css'},
  },
  
  server: {
   hostname: '0.0.0.0',
   port: '3000'
  }
};
```

Save the file through the Nitrous IDE as normal and continue. 

<h2 id=”5”> 5 - Install Brunch Plugins</h2>

As you know we're using JS and SASS for this demo app, which means we need the [Brunch plugins](http://brunch.io/plugins) that provide support for them. To install extra plugins Brunch uses packages hosted through [npm](https://www.npmjs.com/). 

Install the two Brunch plugins we need and attribute them to the project.

```command
npm install --save-dev javascript-brunch sass-brunch
```

> There are plenty of other plugins available for all types of frameworks and projects: [http://brunch.io/plugins](http://brunch.io/plugins)

<h2 id=”6”> 6 -  Build the Project Files</h2>

Now comes the time to build the source files from the previous steps. 

```command
brunch build 
```

This generates the files inside of the `public` directory for previewing or use. Check the "app" files in the `public` directory to see what has been created. 

If we want to minify, concatenate (etc) the project files when they're built, we can pass the `-p` or `--production` option to the build command:

```command
brunch build -p
```

Check out the code in the `public` "app" files again to see the difference.

<h2 id=”7”> 7 - Watching Changes and Test Server</h2>

Running the build command every time adjustments are made to the project can get repetitive. So Brunch can "watch" and rebuild the files automatically for you like a JS task runner would. 

To have Brunch continually re-build the source files each time an adjustment is made, run:

```command
brunch watch 
```

Files are now monitored for changes and rebuilt on the fly. You can test this by altering files temporarily if you like, and noticing the terminal output. 

As an aside, it's a good idea to run `watch` in the background by appending `&` to the command. Alternatively, you can also run it in a separate terminal (if you use SSH with Nitrous). 

Before continuing on you need to open port `3000` in your workstation control panel for this Nitrous project. 

Do this by clicking the "Configure Ports" option of the "Preview" drop-down menu:

![Empty Preview Menu](http://i.imgur.com/oeD0kVH.png)

On your Brunch project you created choose "Manage Ports..." from the "Settings" button drop-down:

![Nitrous Project Listing](http://i.imgur.com/7wbCxBG.png)

Add in port number "3000" and enable the "Primary" option, then apply your changes to restart the project.  

![Port Forwarding Settings](http://i.imgur.com/MbQKq5M.png)

Return to the project's IDE page in your browser, and wait for the reconnect to finish, then return to the correct directory:

```command
cd brunch-demo-app
```

The `watch` command we used before can now properly run a simple HTTP server ([pushserve](https://github.com/paulmillr/pushserve)) that serves the generated files in the `public` directory.

Run this server by adding `-s` to the `watch` command.

```command
brunch watch -s
```

Click the "Preview" tab and the newly enabled "Port 3000" option to see the demo application in your browser. 

![Nitrous App Preview](http://i.imgur.com/T24irQq.png)

From this we can tell whether everything has been setup correctly. The web page should resemble the one in this image if it has been setup correctly:

![Index Page](http://i.imgur.com/G7TiEot.png)

Inspecting the page in a browser like Chrome shows the JS app initialisation is working as indicated by the message in the console output. 

![JS Console Output](http://i.imgur.com/nHz94fw.png)

Great! The project is up and running on the local server, which means we are successful! 

Use `CTRL` + `C` in the IDE terminal to interrupt the server process when needed. 

<h2 id=”8”> 8 - Using Brunch Skeletons</h2>

The default "dead-simple" skeleton we used to create the demo app is great for starting from scratch, but when you want to use other frameworks, additional tools, or work from a less vanilla starting point, this isn't ideal. The real flexibility and appeal of Brunch comes in the form of other pre-made skeletons. 

The process for setting up a project using one of these skeletons is very similar to before. All that's needed is to find the right skeleton for the project you want to create. 

The next few sections show how to setup two real example skeletons, and links to some others. 

<h3 id=”react-skeleton”> React Skeleton</h3>

If you want a Brunch project that's ready made to use [React](https://facebook.github.io/react/) and [Babel](https://babeljs.io/) from the get go, then this is a great skeleton to use. 

This skeleton, like most pre-made Brunch skeletons is a GitHub hosted project -  https://github.com/brunch/with-react

Create a new Brunch project using `-s` and this skeleton name.  

```command
brunch new ~/brunch-react-app -s brunch/with-react
```

Move into the new React project.

```command
cd ~/brunch-react-app 
```

Add the `server:` function we used in the "dead simple" app to this projects `brunch-config.js` configuration file.

The file should mirror the below when amended and saved:

``` brunch-config.js
module.exports = {
  files: {
    javascripts: {
      joinTo: {
        'vendor.js': /^(?!app)/,
        'app.js': /^app/
      }
    },
    stylesheets: {joinTo: 'app.css'}
  },

  server: {
   hostname: '0.0.0.0',
   port: '3000'
  },

  plugins: {
    babel: {presets: ['es2015', 'react']}
  }
};
```

Run the test server and build the source files.

```command
brunch watch -s
```

Click the "Preview" tab and "Port 3000" to see the default React app running in your browser!

![Default React Web App](http://i.imgur.com/XulfpFA.png)

Use `CTRL` + `C` in the IDE terminal to interrupt the server process when needed. 

<h3 id=”chaplin-skeleton”>Chaplin Skeleton</h3>

>"Chaplin is an architecture for JavaScript applications using the Backbone.js library. Chaplin addresses Backbone’s limitations by providing a lightweight and flexible structure that features well-proven design patterns and best practices."

This project uses CoffeeScript instead of JS for its configuration file, and testing must be setup separately - https://github.com/paulmillr/brunch-with-chaplin

Bower is also required:

```command
npm install -g bower
```

Create a new Brunch project using `-s` and the GitHub project URL or skeleton name.   

```command
brunch new ~/brunch-chaplin-app -s https://github.com/paulmillr/brunch-with-chaplin
```

Change into the new Chaplin project.

```command
cd ~/brunch-chaplin-app 
```

Add the `server:` function we used in the "dead simple" app to this project too. Remember the configuration file is `brunch-config.coffee` in this instance so make changes accordingly to the function syntax. 

The file needs to hold the below after amendment:

``` brunch-config.coffee
exports.config =
  # See http://brunch.io/#documentation for docs.
  files:
    javascripts:
      joinTo:
        'javascripts/app.js': /^app/
        'javascripts/vendor.js': /^(?!app)/

    stylesheets:
      joinTo: 'stylesheets/app.css'

    templates:
      joinTo: 'javascripts/app.js'

  server:
    hostname: '0.0.0.0'
    port: '3000'

  npm:
    aliases:
      backbone: 'exoskeleton'

    globals:
      _cp: 'console-polyfill'
      $: 'jquery'

    styles:
      'normalize.css': ['normalize.css']
```

Run the test server and build these source files.

```command
brunch watch -s
```

Click the "Preview" tab and "Port 3000" to see the default Chaplin app running in your browser!

![Default Chaplin Web App](http://i.imgur.com/SyfIlXz.png)

Use `CTRL` + `C` in the IDE terminal to interrupt the server process when finished. 

<h3 id=”more-skeletons”>More Skeletons</h3>

The best skeletons are the ones that are maintained and kept up to date. Here are some hand-picked examples worth exploring:

* Angular 2 - [https://github.com/colinbate/ng2-brunch](https://github.com/nblackburn/brunch-with-vue)
* Vue - [https://github.com/nblackburn/brunch-with-vue](https://github.com/nblackburn/brunch-with-vue)
* Cordova - [https://github.com/brunch/with-cordova](https://github.com/brunch/with-cordova)
* Exim - [https://github.com/hellyeahllc/brunch-with-exim](https://github.com/hellyeahllc/brunch-with-exim ) 
* MarionetteJS - [https://github.com/denar90/brunch-with-marionettejs](https://github.com/denar90/brunch-with-marionettejs)
* Weekend Brunch - [https://github.com/jamesabels/Weekend-Brunch](https://github.com/jamesabels/Weekend-Brunch)
* Bespoke.js  - [https://github.com/makenew/deck-bespoke.js](https://github.com/jamesabels/Weekend-Brunch)
* Tasty Brunch App - [https://github.com/makenew/tasty-brunch](https://github.com/jamesabels/Weekend-Brunch)
* Brunch + Babel/ES6 -  [https://github.com/brunch/with-es6](https://github.com/jamesabels/Weekend-Brunch)

More are listed on the Brunch website at: http://brunch.io/skeletons

<h2 id=”9”> 9 - Nitrous Quickstarts (Optional)</h2>

A new feature named "[Quickstarts](https://community.nitrous.io/docs/nitrous-quickstarts)" let's you create custom Nitrous projects from external GitHub repositories with the click of an embedded HTML button from any GitHub README. This step shows how you would go about creating one for a Brunch app's project, and uses the demo app we made. 

Move back into the original Brunch demo app project. 

```command
cd ~/brunch-demo-app
```

Create `nitrous.json` - a template configuration file.

```command
touch nitrous.json
```

Add in these contents to configure the base Nitrous template and define a [Run Script](https://community.nitrous.io/docs/run-scripts):

``` nitrous.json
{
  "template": "nodejs", 
  "ports": [3000],
  "name": "Brunch Applications Template",
  "logo": "http://i.imgur.com/aePKLBU.png",
  "description": "An ultra-fast HTML5 build tool",
  "scripts": {
    "Start Brunch Demo App Server": "cd ~/code/nitrous-brunch-demo-app && brunch watch -s"
  }
}
```

Save the file. 

Then create the `nitrous-post-create.sh` script that's run by Quickstart after the template creation.

```command
touch nitrous-post-create.sh
```

Enter these commands into the `nitrous-post-create.sh` file. 

``` nitrous-post-create.sh
#!/bin/bash

npm install -g brunch

cd ~/code/nitrous-brunch-demo-app

npm install 

brunch build
```

Make the script executable.

```command
chmod +x nitrous-post-create.sh
```

The last file holds commands or steps that need to be carried out by the user after the Quickstart feature finishes, once they enter the IDE or project. 

```command
touch nitrous.readme.md
```

Here are some suitable instructions for the Brunch demo app to enter into the `nitrous.readme.md` file. 

``` nitrous.readme.md
Click the "Start Brunch Demo App Server" option from the "Run" tab to launch. 

![Nitrous Run Menu](http://i.imgur.com/jFLLZGF.png)

Then click the "Preview" tab and "Port 3000" to see the demo application in your browser. 

![Nitrous App Preview](http://i.imgur.com/T24irQq.png)
```

These files and the project files as a whole now need be placed into a repository on GitHub.

To do this work through and enter the following commands:

```command
git init
git add .
git commit -m "first commit - added project files and nitrous quickstart files."
```

Assuming you have a user account already on GitHub, sign in and create a new repository through the page here: https://github.com/new

Back on IDE command line enter the new remote GitHub repository details by substituting your username and repo name into the first command: 

```command
git remote add origin https://github.com/github-username/repository-name.git
git push -u origin master
```

To use the Nitrous Quickstart you created, users must click a trigger button you embed into web pages. The code and variations for this button are described [here]( https://community.nitrous.io/docs/nitrous-quickstarts) in detail. 

This is the button markdown code and its output for **our** Brunch demos [example Quickstart repo.](https://github.com/5car1z/nitrous-brunch-demo-app) Remember you will need to create and embed your repo's button if you want to complete this step with your own example. 

```
[![Nitrous Quickstart](https://nitrous-image-icons.s3.amazonaws.com/quickstart.svg)](https://www.nitrous.io/quickstart?repo=https://github.com/5car1z/nitrous-brunch-demo-app)
```

[![Nitrous Quickstart](https://nitrous-image-icons.s3.amazonaws.com/quickstart.svg)](https://www.nitrous.io/quickstart?repo=https://github.com/5car1z/nitrous-brunch-demo-app)

<h2 id=”conclusion”>Conclusion</h2>

Everything you need to begin writing a web app with Brunch has been covered here. You may have to do some more searching to find a skeleton that's up to date and that matches your requirements, but the skeletons we mentioned earlier are a good place to start. 

For anything we missed there's likely an answer or solution on [Brunch's official website](http://brunch.io/). Thanks for reading!


