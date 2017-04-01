# Getting Started with Bower

![Bower Logo](http://i.imgur.com/pMRTo67.png)

<div class=”table-of-contents”>
<h3>Table of Contents</h3>
<ul>
    <li><a href=”#Prerequisites”> Prerequisites </a></li>
    <li><a href=”#what-is-bower”> What is Bower?</a></li>
    <li><a href=”#1”> 1 - Create a Node.js Container</a></li>
    <li><a href=”#2”> 2 - Installing Bower</a></li>
    <li><a href=”#3”> 3 - Initialising a Project</a></li>
    <li><a href=”#4”> 4 - Searching Packages</a></li>
    <li><a href=”#5”> 5 - Installing Bower Packages</a></li>
      <ul>
	        <li><a href=”#bootstrap”> Boostrap</a></li>
	        <li><a href=”#fontawesome”>FontAwesome</a></li>
	        <li><a href=”#animate-css”>Animate.css</a></li>
	  </ul>    
    <li><a href=”#6”> 6 - Updating Packages</a></li>
	<li><a href=”#7”> 7 - Removing Packages</a></li>
    <li><a href=”#8”> 8 - Using Packages</a></li>
    <li><a href=”#conclusion”>Conclusion</a></li>
</ul>

<h2 id=”Prerequisites”>Prerequisites</h2>

<ul class=”prerequisites”>
     <li>An understanding of how <a href="https://git-scm.com/">Git version control</a> works is helpful but not essential to this tutorial.
     </li>
</ul>

<h2 id=”what-is-bower”>What is Bower?</h2>

Web applications are the sum of a thousand working parts. Web apps include databases, web servers, and in the context of Bower - frameworks, libraries, assets, and utilities. Bower makes managing front-end tools and libraries much easier.

>"A package manager for the web."

Simply put, Bower is a package manager for the web. Instead of manually setting up front-end libraries and frameworks in your project files, Bower will do it for you. It provides an easy means of acquiring, finding, and implementing the web components you need for your project. The most common of which are likely to be JavaScript or CSS libraries. Once specified, installed libraries are tracked through a central JSON configuration file, stored in the root directory of the project.

Bower doesn’t add overhead to your web application since it uses a *flat* dependency tree to eliminate increased page load times. This makes the overall project structure more modular and not tied to its various front-end dependencies.

In this article, we’ll teach you how to install and use Bower in an example web application.

<h2 id=”1”> 1 - Create a Node.js Container</h2>

Begin by creating a new container from within a pre-existing workspace. This is found at the top right of the screen inside the `+` symbol drop-down menu.  

The template you'll want to use is listed as "Node.js".

By using this container template we have Node Package Manager and several other useful setups already in place.

<h2 id=”2”> 2 - Installing Bower</h2>

The recommended way of installing Bower on the container is through the Node Package manager (aka `npm`).

Here's the command to install Bower in your container:

```command
npm install -g bower
```

This may take a minute or two to process.

Note the inclusive  `-g` switch that makes the install global to NPM.

<h2 id=”3”>3 - Initialising a Project</h2>

Let's say we have an ongoing web project that is currently being worked on, and it uses Git as the version control system.

First we clone the web project into our Nitrous container:

```command
git clone https://github.com/madhums/node-express-mongoose-demo.git 
```

The repository actually being cloned off of GitHub here is a demonstration project, in reality you would of course clone your own project. 

We’ll be utilizing Bower inside of this demo's repository as we go along. 

```command
cd node-express-mongoose-demo
```

Another separate common place to initialise Bower is from within a web server's root directory, or a specific virtual host's directory i.e.

`/var/www` **or** `/var/www/example-vhost.com`

In our example we are sticking to the the Git repository scenario from before though. 

Continue by initialising Bower inside of the repository:

```command
bower init
```

There are several questions to answer before Bower will create the *manifest* configuration file.

The first question below about `stats reporting` is up to you, pressing `ENTER` defaults the choice to **yes**.

```
May bower anonymously report usage statistics to improve the tool over time?
```

Next, you need to enter a `name` for the project configuration -- this will likely be the same if not similar to the Git project name.

```
name: mongoose-demo
```

Next we’ll define a `version` -- since this is the beginning of the project we'll hit `ENTER` for the default value.

```
version: 0.0.0
```

Next we’ll enter a `description` of the project. We're testing out Bower in this example, so here's the description we'll use.

```
description: A bower test project.
```

Next we’ll enter the `main file`. This is most frequently the landing page or entry point to your application. `index.html` or `index.php` are common as they usually serve as the homepage of a project.

```
main file: index.html
```

Next we’ll choose the `default modules` to expose. Pressing `ENTER` for the defaults is fine for most cases. Custom modules are usually only needed when building and importing your own libraries.

```
what types of modules does this package expose? (Press <space> to select)
 ◯ amd
 ◯ es6
 ◯ globals
 ◯ node
 ◯ yui
```

Next we’ll enter some `keywords` with the name of the project and the packages it makes use of.

```
keywords: bower bootstrap fontawesome animate.css
```

Next, enter the names of the developer(s) or organisation running the project.

```
authors: Nitrous Platform
```

Next, select the type of software `license` attributed to the project's code-base, if any.

```
license: (MIT)
```

Now assign a `homepage` for the Bower project. This has no impact on the way Bower operates and is only for informative purposes.

```
homepage: https://www.nitrous.io
```

Next, we’ll define `dependencies`. Selecting **yes** means front-end dependencies Bower recognises in the project directory are imported into Bower's configuration. We don’t have any in our example so just select the default.

```
set currently installed components as dependencies? (Y/n)
```

Next, we are asked to define any commonly ignored files. Some directories like `node_modules` and `bower_components` do not need tracking and can be safely ignored by Bower, so select the default **yes**.

```
add commonly ignored files to ignore list? (Y/n)
```

Next, define the package access level. It is generally a good idea to select `yes` to keep the package private. 

```
would you like to mark this package as private which prevents it from being accidentally published to the registry? (y/N)
```

After the output summary of our answers, there's one more final confirmation to accept.

```
Looks good (Y/n)
```

The resulting JSON manifest file named `bower.json` can be seen in our current working directory with:

```command
ls
```

Inside of the Nitrous IDE you can navigate to the `bower.json` file in the left pane, and see its contents on the right once selected.

![bower.json Image Preview](https://i.gyazo.com/5057e0dfb45b89ed48b54eb695693023.png)

As a side note, `bower.json` can be either edited manually or interactively with some of the commands listed below.

<h2 id=”4”>4 - Searching Packages</h2>

There are two main options to view the dependencies Bower offers. The first is the online search tool hosted at:

[http://bower.io/search/](http://bower.io/search/)

The second is on the command line of the workspace. Typing `bower search` followed by the approximate package name, yields a search result returning information on the term entered.

```command
bower search animate.css
```

The output lists matching packages, and shows the source URL of the package software.

```console
Search results:
animate.css git://github.com/daneden/animate.css.git
```

The Bower search tool is flexible so you don't have enter the exact package names.

If you want further information about a package, the `info` command can help:

```command
bower info animate.css
```

<h2 id=”5”>5 - Installing Bower Packages</h2>

The general Bower command for installing packages is:

```command
bower install <package-name> --save
```

The `--save` switch registers the package in our `manifest.json` config file.

Alternatively, if we had already specified some package names in the configuration file above, they can be retrieved and installed automatically by entering:

```command
bower install
```

Now that we've laid out the groundwork for managing our front-end dependencies and we know generally how to install them. Let's download and install some example packages into our demo repository.

<h3 id=”bootstrap”>Bootstrap</h3>

Bootstrap is a popular HTML, CSS, and JS framework. 

Install it with:

``` command
bower install bootstrap --save
```

Note that `jquery` is a dependency of the Bootstrap package, and is installed alongside Bootstrap and added as part of our Bower config.

```
bootstrap#3.3.5 bower_components/bootstrap
└── jquery#2.1.4
jquery#2.1.4 bower_components/jquery
```

<h3 id=”fontawesome”>FontAwesome</h3>

We can install FontAwesome, the popular font and CSS library using Bower too via:

``` command
bower install fontawesome --save
```

Check your newly updated `bower.json` file with a *pager* program like `[less](https://en.wikipedia.org/wiki/Less_(Unix))` to see its new contents:

```command
less bower.json
```

At the bottom you'll see the dependencies section containing the front-end packages we’ve installed.

```
"dependencies": {
    "bootstrap": "~3.3.5",
    "font-awesome": "fontawesome#~4.4.0"
```

<h3 id=”animate-css”>Animate.css</h3>

Animate.css provides a comprehensive set of CSS animations we can could use in our application.

``` command
bower install animate.css --save
```

<h2 id=”6”>6 - Updating Packages</h2>

Open source libraries get updated frequently, and we’ll want to keep them up-to-date as regularly as possible. You can update packages quickly and easily using the `update` command in your Nitrous console.

```command
bower update
```

Individual and multiple packages can be specified too!

```command
bower update fontawesome animate.css
```

To gain access to the latest most up to date (often alpha) package builds, install `bower-update` with NPM:

```command
npm install -g bower-update
```

Then run this separate updater with:

```command
bower-update
```

<h2 id=”7”>7 - Removing Packages</h2>

To review what's installed by Bower we can enter this command into the command line:

```command
bower list
```

Which outputs the following details:

```
bower check-new     Checking for new versions of the project dependencies...
website-project-name#0.0.0 /home/nitrous/web_project
├── animate.css#3.4.0
├─┬ bootstrap#3.3.5 (latest is 4.0.0-alpha)
│ └── jquery#2.1.4 (3.0.0-alpha1+compat available)
└── font-awesome#4.4.0
```

If we decide there's no more need for the `animate.css` package and it's files, we can run the `uninstall` command followed by the package name:

```command
bower uninstall animate.css --save
```

The `--save` switch in this instance also removes `animate.css` as a registered dependency from the configuration file.

<h2 id=”8”>8 - Using Packages</h2>
The most basic way to use installed dependencies is by importing them directly with HTML tags. Here is an example using the familiar Bootstrap and FontAwesome packages we have installed. 

The file named `head.html` sets up meta information, scripts, and stylesheets for use in the demo project. 

The path to this file is: `node-express-mongoose-demo/app/views/includes/head.html`

Navigate to `head.html` and open it in the Nitrous editor. 

![head.html Image Preview](https://i.gyazo.com/355d1804c871f54f7f68ae70ffacdfa7.png)

To alter the file so the demo project includes our Bower version of Bootstrap and FontAwesome. We have to change the code in the "Styles" section. 

By default It looks like this:

```html
{% block head %}
  {# Styles #}
  <link href="//netdna.bootstrapcdn.com/bootstrap/3.2.0/css/bootstrap.min.css" rel="stylesheet">
  <link href="/css/jquery.tagsinput.css" rel="stylesheet">
  <link href="//netdna.bootstrapcdn.com/font-awesome/4.2.0/css/font-awesome.min.css" rel="stylesheet">
  <link rel="stylesheet" href="/css/app.css">
{% endblock %}
```

Replace the Bootstrap file paths with our local Bower paths, so the code block ends up looking like this:

```html
{% block head %}
  {# Styles #}
  <link href="/bower_components/bootstrap/dist/css/bootstrap.min.css" rel="stylesheet">
  <script type="text/javascript" src="/bower_components/bootstrap/dist/js/bootstrap.min.js">
  <link href="/css/jquery.tagsinput.css" rel="stylesheet">
  <link rel="stylesheet" href="/css/app.css">
{% endblock %}
```

Then inculde the FontAwesome Bower library by adding another link tag, so the code now resembles:

```html
{% block head %}
  {# Styles #}
  <link href="/bower_components/bootstrap/dist/css/bootstrap.min.css" rel="stylesheet">
  <script type="text/javascript" src="/bower_components/bootstrap/dist/js/bootstrap.min.js">
  <link rel="stylesheet" href="/bower_components/font-awesome/css/font-awesome.css">
  <link href="/css/jquery.tagsinput.css" rel="stylesheet">
  <link rel="stylesheet" href="/css/app.css">
{% endblock %}
```

Remember to save the file in the editor when finished. 

All this is necessary as our Bower dependencies are stored locally in the `bower_components` directory. The old paths lead to versions of the packages provided by the demo app. 

<h2 id=”conclusion”>Conclusion</h2>

With all this in mind, Bower is designed to be used with similar tools. This establishes an efficient workflow for importing assets and resources into your app’s codebase.

If you’d like to learn about more complex and powerful front-end development tools, check out [Gulp](https://community.nitrous.io/tutorials/setting-up-gulp-with-livereload-sass-and-other-tasks), [Grunt](http://gruntjs.com/), & [Yeoman](http://yeoman.io/).

You can also check out Bower’s recommended tools and libraries:

http://bower.io/docs/tools/



