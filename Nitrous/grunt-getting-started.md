
# Getting Started with Grunt

![Grunt Logo](https://cdn.tutsplus.com/wp/uploads/2014/01/grunt-logo-400.png)

<div class=”table-of-contents”>
<h3>Table of Contents</h3>
<ul>
    <li><a href=”#Prerequisites”> Prerequisites </a></li>
    <li><a href=”#what-does-grunt-do”> What Does Grunt Do?</a></li>
    <li><a href=”#1”> 1 - Create a Node.js Container</a></li>
    <li><a href=”#2”> 2 - Installing Base Grunt Packages</a></li>
    <li><a href=”#3”> 3 - Initialising a Project</a></li>
    <li><a href=”#4”> 4 - Installing Grunt & Default Plugins</a></li>
    <li><a href=”#5”> 5 - Configuring the Gruntfile</a></li>
    <ul>
        <li><a href=”#grunt-contrib-concat”> grunt-contrib-concat</a></li>
	    <li><a href=”#grunt-contrib-uglify”>grunt-contrib-uglify</a></li>
	    <li><a href=”#grunt-contrib-jshint”>grunt-contrib-jshint</a></li>
	    <li><a href=”#grunt-contrib-qunit”>grunt-contrib-qunit</a></li>
	    <li><a href=”#grunt-contrib-watch”>grunt-contrib-watch</a></li>
	</ul>    
    <li><a href=”#6”> 6 - Enabling & Running Grunt Tasks</a></li>
    <li><a href=”#conclusion”> Conclusion</a></li>
</ul>

<h2 id=”Prerequisites”>Prerequisites</h2>
<ul class=”prerequisites”>
     <li>An understanding of how <a href="https://git-scm.com/">Git version control</a> works is helpful but not essential to this tutorial.
     </li>
</ul>

<h2 id=”what-does-grunt-do”>What Does Grunt Do?</h2>
Surprisingly sometimes a lot more work goes into maintaining web applications than it does actually creating them. This can generate a substantial amount of extra workload for those running the service, website, or online platform. 

Thankfully though there are web app automation tools out there to help with this. Often they are referred to as *task runners*, and their job is to to help complete routine work activities such as minification, compilation, unit testing, linting, and any most repetitive tasks. 

"Grunt" is a JavaScript focused task runner, and exists to remove the mundane side of web app maintenance with minimal effort on the maintainers part. It also has a wide variety of plugins on offer, so you can automate almost anything and everything with relative ease. 

In this tutorial we cover the fundamentals of how to install, setup, and make use of Grunt in an example web project. 

<h2 id=”1”> 1 - Create a Node.js Container</h2>
Start off by creating a new container from within a pre-existing workspace. This is found at the top right of the screen inside the `+` symbol drop-down menu.  

The template you'll want to use is listed as "Node.js".

![Node.js Container Image](https://nitrous-community.s3.amazonaws.com/images/nodejs-template.png)

This container template is ideal for using Grunt and gives us access to Node Package Manager, plus some of the other background tools we may need. 

Update these tools and packages to their latest versions using:

```command
sudo apt-get update && sudo apt-get upgrade 
```

Enter `y` for any prompts and wait whilst the updates are downloaded then installed to the container.

<h2 id=”2”> 2 - Installing Base Grunt Packages</h2>
Grunt is split into multiple components by design. The core package we need from the onset is the CLI package. Which adds the `grunt` command to your *system path* and allow us to run it from anywhere in the container's file-system. 

Install it with Node Package Manager onto your container:

```command
npm install -g grunt-cli
```

The only other package you need to install for now is the initialisation package, with:

```command
npm install -g grunt-init
```

There are some more packages to install later on once we get into adding the actual Grunt program and plugins. 

<h2 id=”3”> 3 - Initialising a Project</h2>
Most projects are run and developed using some form of version control, our example project is no different. 

Clone this **example** web project into your Nitrous container:

```command
git clone https://github.com/madhums/node-express-mongoose-demo.git
```

We're going to be using the demo project to demonstrate setting up and running Grunt.

Let's change into the project so it's the current working directory:

```command
cd node-express-mongoose-demo/
```

Run this next command to have `npm` install all our demo projects dependencies into the container for us. It will take a few minutes to complete. 

```command
npm install
```

Grunts main configuration is handled by two files, one named `package.json` and another named `Gruntfile.js` .

Proceed by cloning the official Grunt template repository, so we can generate one of these two files:

```command
git clone https://github.com/gruntjs/grunt-init-gruntfile.git ~/.grunt-init/gruntfile
```

Run the template through the `grunt-init` package from earlier:

```command
grunt-init gruntfile
```

A series of prompts containing questions will be displayed. 

Answer the questions with the corresponding letters found in the below output:

```
[?] Is the DOM involved in ANY way? (Y/n) Y
[?] Will files be concatenated or minified? (Y/n) Y
[?] Will you have a package.json file? (Y/n) N
[?] Do you need to make any changes to the above before continuing? (y/N) N
```

It's important **not** to generate a new `package.json` file here as we already have one in our demo project, and don't want to overwrite its contents.

The correct output you should see is as follows:

```
Writing Gruntfile.js...OK
Initialized from template "gruntfile".
Done, without errors.
```

We are now ready to install Grunt itself and the initial plugins. 

<h2 id=”4”> 4 - Installing Grunt & Default Plugins</h2>
The `package.json` file holds all the metadata related to our project. In regards to Grunt, it lists all the plugins and dependencies required to run tasks successfully. 

Installing and scheduling tasks with Grunt is intended to be easy and intuitive. This is achieved through using *plugin* packages that we must install and configure. 

Install Grunt and the core plugins using:

```command
npm install grunt grunt-contrib-jshint grunt-contrib-watch grunt-contrib-qunit grunt-contrib-concat grunt-contrib-uglify --save-dev
```

This brings in `grunt` itself and attributes the plugins as dependencies inside the `package.json` file. vim 

Navigate to the `package.json` file in your Nitrous IDE to see it's contents.

Scrolling down towards the bottom of the file will show Grunt and the plugins as new dependencies:

![Nitrous IDE - package.json Image](http://i.imgur.com/0lmEAce.png)

Move on to examine the Gruntfile we generated earlier. 

<h2 id=”5”>5 - Configuring the Gruntfile</h2>
`Gruntfile.js` or just *"the Gruntfile"* is where we configure the plugins and tasks we want to run through Grunt. 
 
Navigate to `Gruntfile.js` in your Nitrous IDE and check out the code.

![Nitrous IDE - Gruntfile.js Image](http://i.imgur.com/dcqomne.png)

First incorporate the new code below to the `// Project configuration.` section, so the Gruntfile looks like this:

```JavaScript
   /*global module:false*/
  module.exports = function(grunt) {

  // Project configuration.
  grunt.initConfig({
    jsDir: 'public/js/',
    jsDistDir: 'dist/js/',
    cssDir: 'public/css/',
    cssDistDir: 'dist/css/',
    pkg: grunt.file.readJSON('package.json'),
```

<h3 id=”grunt-contrib-concat”>grunt-contrib-concat</h3>
If we continue and narrow down the code in here to the sections that are relevant to our plugins, we can see they begin at the comment that reads:  `// Task configuration.` 

![Nitrous IDE - Gruntfile.js Image](http://i.imgur.com/o5dTmQm.png)

Below the comment we find a `concat` code block. 

This contains the task settings and configuration options for the `grunt-contrib-concat` plugin. Which groups together (or concatenates) all the JavaScript file contents in the `src:` path, and then outputs the results into the location defined on the `dest:` line.
 
Adapt the code for this plugin so it now contains:

```JavaScript
     // Task configuration.
     concat: {
      js: {
        options: {
          separator: ';'
          },
        src: ['<%=jsDir%>*.js'],
        dest: '<%=jsDistDir%><%= pkg.name %>.js'
      },
      css: {
        src: ['<%=cssDir%>*.css'],
        dest: '<%=cssDistDir%><%= pkg.name %>.css'
      }
    },
```

<h3 id=”grunt-contrib-uglify”>grunt-contrib-uglify</h3>
Further down is another code block containing the settings for the `grunt-contrib-uglify ` plugin. This takes the results of the JS *concat* task and then *minifies* it. 

Alter this code to instead contain:

```JavaScript
  uglify: {
      options: {
        banner: '/*! <%= pkg.name %> <%=grunt.template.today("dd-mm-yyyy") %> */\n'
      },
      dist: {
        files: {
          '<%=jsDistDir%><%= pkg.name %>.min.js': ['<%= concat.js.dest %>']
        }
      }
    },
```

<h3 id=”grunt-contrib-jshint”>grunt-contrib-jshint</h3>
Next are the options for the `grunt-contrib-jshint` plugin. This task lists any errors, potential problems, and issues in the JavaScript coding of files in the `:src` directories. There is no need to change anything for this plugin's code. 

```JavaScript
    jshint: {
      options: {
        curly: true,
        eqeqeq: true,
        immed: true,
        latedef: true,
        newcap: true,
        noarg: true,
        sub: true,
        undef: true,
        unused: true,
        boss: true,
        eqnull: true,
        browser: true,
        globals: {}
      },
      gruntfile: {
        src: 'Gruntfile.js'
      },
      lib_test: {
        src: ['lib/**/*.js', 'test/**/*.js']
      }
    },
```

<h3 id=”grunt-contrib-qunit”>grunt-contrib-qunit</h3>
`grunt-contrib-qunit` used to carries out *unit tests* with [Phantom JS](http://phantomjs.org/).  This current config tests all `.html` files in the `test` directory and subdirectories. We are not going to test out or use it in this tutorial however. 

```JavaScript
    qunit: {
      files: ['test/**/*.html']
    },
```

<h3 id=”grunt-contrib-watch”>grunt-contrib-watch</h3>
The `watch` task is run constantly in the background and monitors file changes. If any of "changes" affect our selected tasks in any way, it will re-run the tasks to update their results with the changes. 

Here is what you need to add and change for our example project:

```JavaScript
 watch: {
       files: ['<%=jsDir%>*.js', '<%=cssDir%>*.css'],
       tasks: ['concat', 'uglify']
      },              
      gruntfile: {
        files: '<%= jshint.gruntfile.src %>',
        tasks: ['jshint:gruntfile']
      },
      lib_test: {
        files: '<%= jshint.lib_test.src %>',
        tasks: ['jshint:lib_test', 'qunit']
      }
  });
```

> `Gruntfile.js` should be committed and tracked by version control, so others can easily clone and setup the project as and when needed. 

**Save** your changes to the Gruntfile before moving on with `CTRL` + `S` or via the "File" drop down menu. 

<h2 id=”6”> 6 - Enabling & Running Grunt Tasks</h2>
From looking at the Gruntfile we can see the tasks are configured and ready to run, but the plugins themselves still need to be enabled.

Scroll even further down and you'll see where these task plugins are enabled. 

```JavaScript
  // These plugins provide necessary tasks.
  grunt.loadNpmTasks('grunt-contrib-concat');
  grunt.loadNpmTasks('grunt-contrib-uglify');
  grunt.loadNpmTasks('grunt-contrib-qunit');
  grunt.loadNpmTasks('grunt-contrib-jshint');
  grunt.loadNpmTasks('grunt-contrib-watch');
```

From here onwards configured and enabled plugins are added as "default tasks". Add the `watch` task at the end, and remove `'jshint'` plus `'qunit'` so your line now looks like this: 

```JavaScript
  // Default task.
  grunt.registerTask('default', ['concat', 'uglify']);
```

Back on the command line outside of the Gruntfile. These default tasks are now triggered and run by calling `grunt` as a program.

Run it with:

```command
grunt
```

A successful output reads:

```
Running "concat:js" (concat) task
File dist/js/nodejs-express-mongoose-demo.js created.
Running "concat:css" (concat) task
File dist/css/nodejs-express-mongoose-demo.css created.
Running "uglify:dist" (uglify) task
>> 1 file created.
Done, without errors.
```

Inside of the `dist/css` and `dist/js` directories there are now the concatenated and *minified* files!

![Concatenated CSS](http://i.imgur.com/TF9Dqxm.png)

Alternatively the tasks that weren't added as a default task are run when passed as parameters to Grunt. 

For example:

```command
grunt watch &
```

`watch` now re-runs the selected tasks again if necessary. 

Further plugins to add to your Grunt setup can be searched for on the official Grunt website:

http://gruntjs.com/plugins

<h2 id=”conclusion”> Conclusion</h2>
Grunt works even better when it's used with similar like-minded tools. This helps establish an efficient workflow for manipulating assets and resources within your app’s codebase.

If you’d like to learn about more complex and powerful front-end development tools, check out [Gulp](https://community.nitrous.io/tutorials/setting-up-gulp-with-livereload-sass-and-other-tasks), [Bower](https://community.nitrous.io/tutorials/getting-started-with-bower), & [Yeoman](http://yeoman.io/).

You can also check out Grunt's documentation on creating plugins; if you can't find an official plugin that meets your project's needs:

http://gruntjs.com/creating-plugins

