
# Using Z Shell (zsh) in Nitrous Projects

<div class=”table-of-contents”>
<h3>Table of Contents</h3>
<ul>
    <li><a href=”#introduction”> Introduction</a></li>
    <li><a href=”#1”> 1 - Create a Nitrous Project</a></li>
    <li><a href=”#2”> 2 - Download & Run Nitrous Script</a></li>
    <li><a href=”#3”> 3 - Auto-completion</a></li>
    <li><a href=”#4”> 4 - Event Designation</a></li>
    <li><a href=”#5”> 5 - Glob Operators</a></li>
    <li><a href=”#6”> 6 - Glob Qualifiers</a></li>
    <li><a href=”#7”> 7 - Modifiers</a></li>
    <li><a href=”#conclusion”> Conclusion</a></li>
</ul>

<h2 id=”introduction”>Introduction</h2>

The projects you create on Nitrous have Z shell (`zsh`) in place as the default shell. This article shows some of its features and how you can get the most out of using it. Several of the individual features found here are not exclusive to Z Shell, but are in most instances implemented in a better fashion. With a few of them further enhanced by the oh-my-zsh framework in use by the projects. 

<h2 id=”1”> 1 - Create a Nitrous Project</h2>

>You can skip this step and use one of your pre-existing projects if you prefer. 

Create a new project from within your Nitrous workstation to get started. 

This is found on the default “Projects” tab near the center of the screen - indicated by a `+` symbol inside of a square. Click it to be taken to the “New Project” creation page. 

![New Project Image](http://nitrous-community.s3.amazonaws.com/images/new-dashboard.png)

For this article you can use just about any of the Nitrous project templates, but choosing the "Ubuntu" template here is a suitable choice. 

![Ubuntu Project Image](http://i.imgur.com/nhgufqO.png)

You can now either launch the Nitrous IDE as usual, or SSH into the project with a terminal emulator instead (the latter requires a paid Nitrous plan). 

Once you've gained access to the project - move on to **step 2** to create the Z shell test directories and files. 

<h2 id=”2”> 2 - Create & Run Nitrous Script </h2>

Here's a script that creates some false place-holder directories and files on the system for us to test out the Z shell features.

``` https://gist.github.com/5car1z/b26477757b57be836bb9
#!/bin/zsh

mkdir -p zsh_nitrous_test/{project_one,project_two}/html/{main,public}/ zsh_nitrous_test/{project_one,project_two}/css/{style,min}/ zsh_nitrous_test/{project_one,project_two}/js/{lib,test}/

# create dummy files inside the html directories
for directory in zsh_nitrous_test/*/html/*; do
    dd if=/dev/zero of="${directory}/homepage.html" bs=1024 count=1
    dd if=/dev/zero of="${directory}/about.html" bs=2048 count=1
    dd if=/dev/zero of="${directory}/contact.html" bs=4096 count=1
done

# create dummy files inside the css directories
for directory in zsh_nitrous_test/*/css/*; do
    dd if=/dev/zero of="${directory}/homepage.css" bs=1024 count=1    
    dd if=/dev/zero of="${directory}/about.css" bs=2048 count=1
    dd if=/dev/zero of="${directory}/contact.css" bs=4096 count=1
done

# create dummy files inside the js directories
for directory in zsh_nitrous_test/*/js/*; do
    touch "${directory}/main.js"      #plain-text non binary file  
    touch "${directory}/helper.js"    #plain-text non binary file  
done
```

Either copy the above contents into a new file named `zsh-nitrous-test-script.sh` using the Nitrous IDE.

Or instead download the script from the [GitHub Gist](https://gist.github.com/5car1z/b26477757b57be836bb9) using: 

``` command
wget -O http://git.io/vB3oe zsh-nitrous-test-script.sh
```

After make the new script file you created/downloaded executable:

```command
chmod +x zsh-nitrous-test-script.sh 
```

And run it:

```command
./zsh-nitrous-test-script.sh
```

There's now a new `zsh_nitrous_test` directory with more subdirectories inside - all created by the script.

This what the inner directories look like in a tree style layout:

```
├── project_one
│   ├── css
│   │   ├── min
│   │   │   ├── about.css
│   │   │   ├── contact.css
│   │   │   └── homepage.css
│   │   └── style
│   │   ├── about.css
│   │   ├── contact.css
│   │   └── homepage.css
│   ├── html
│   │   ├── main
│   │   │   ├── about.html
│   │   │   ├── contact.html
│   │   │   └── homepage.html
│   │   └── public
│   │   ├── about.html
│   │   ├── contact.html
│   │   └── homepage.html
│   └── js
│   ├── lib
│   │   ├── helper.js
│   │   └── main.js
│   └── test
│   ├── helper.js
│   └── main.js
└── project_two
├── css
│   ├── min
│   │   ├── about.css
│   │   ├── contact.css
│   │   └── homepage.css
│   └── style
│   ├── about.css
│   ├── contact.css
│   └── homepage.css
├── html
│   ├── main
│   │   ├── about.html
│   │   ├── contact.html
│   │   └── homepage.html
│   └── public
│   ├── about.css
│   ├── about.html
│   ├── contact.css
│   ├── contact.html
│   ├── homepage.css
│   └── homepage.html
└── js
├── lib
│   ├── helper.js
│   └── main.js
└── test
├── helper.js
└── main.js

20 directories, 35 files
```

Move on to testing the Z shell features with this new file hierarchy. 

<h2 id=”3”> 3 - Tab Auto-completion </h2>

Z shell (and oh-my-zsh in our case) is really good at predicting what you want to do on the command line. The most obvious example of this is its auto-completion capabilities. 

It's most frequently seen when you enter enter commands like the `cd` command. 

If you follow `cd` up with a press of your `SPACE` key, and then `TAB` key. You will see an interactive list of directories to choose from:

![Nitrous IDE Z Shell Auto-completion Image](http://i.imgur.com/OdPHXa6.png)

Pressing `TAB` again cycles through the choices, with `ENTER` confirming the selection:

![Nitrous IDE Z Shell Auto-completion Image](http://i.imgur.com/LhKR3JI.png)

This auto-completion works exactly the same for other common shell commands that require a file, directory, or path:

![Nitrous IDE Z Shell Auto-completion Image](http://i.imgur.com/0Jui4sV.png)

It integrates into commands themselves too. 

Like with `apt-get` the project's package manager:

![Nitrous IDE Z Shell Auto-completion Image](http://i.imgur.com/Hl495DX.png)

Hitting your `TAB` key suggests in the same fashion as before some of the possible packages you might be looking for.

On a more basic level related commands or shell functions you might be trying to enter are suggested also:

![Nitrous IDE Z Shell Auto-completion Image](http://i.imgur.com/WnhKJ1Q.png)


<h2 id=”4”> 4 - Event Designation </h2>

*Event designators* reference commands we have entered at some point in the past. This could be the last command, the command we entered three entries ago, or however far back beyond that into the shell's history we can go. 

The character that triggers this feature is the `!` character. 

So to show the **last** command we entered type out two `!` characters and press the `TAB` key:

```command
!!
```

Your terminal now recreates and shows the previous command entered.

To find the command we entered **three** commands ago, use a `!` with a `-` followed by the number `3` and press `TAB` again:

```command
!-3
```

The prompt displays the command from three entries ago! 

It's possible to replace this number in the last command for however many commands back you like. 

```command
!-8
```

We can also use event designation to refer back to past **arguments** of old commands. 

Enter in and send through this example command, then examine the output:

```command
ls zsh_nitrous_test/project_one/css/style/homepage.css
```

Now imagine we want to see the same output again, but with the different `ls -l` format. 

To do this type the next command out; followed by the `TAB` key as done previously. 

```command
ls -l !!1
```

The file path from the last command is automatically added in for us. 

This works as the command refers back to the first previous argument; the path to our `homepage.css file` .

To use arguments from older commands the syntax is slightly different.

Replace the second exclamation mark with a `-` and a command history number (e.g.  `3`) followed by a `:` then an argument number again (`1`) : 

```command
ls -l !-5:1
```

> **Note:** if you get a "no such word in event" message here it means there is no first argument for the older command you are targeting. 

The `:` here is necessary to split up the values and stop the shell from going thirty-one commands back. 

More information on Event Designation is listed at:

> [Zsh - 14.1.2 Event Designators](http://zsh.sourceforge.net/Doc/Release/Expansion.html#Event-Designators)

<h2 id=”5”> 5 - Glob Operators</h2>
*Globbing* refers to short command line based expressions that let us create custom patterns to target files and directories with. Operators are the symbols used during the globbing. 

The most basic and common example of this is with the asterisk wildcard operator `*` . 

There are instances of this in use by the test script that created our false directories e.g.

``` ~/.zsh-nitrous-test-script.sh
for directory in zsh_nitrous_test/*/html/*; do
```

The `*` matches **any** and **all** potential values in a command or command sequence, for example:

```command
ls zsh_nitrous_test/project_one/*/*/*.html   
```

The output shows all `.html` files in `project_one` without us having to fill in any sort of full path or real directory names. 

Other glob operators like `[]` let us be more specific. Targeting file names that **begin** with certain letters or numbers:

```command
ls -l zsh_nitrous_test/*/*/*/[ab]*.*
```

Which shows the "about" files:

```
-rw-rw-r-- 1 nitrous nitrous 2048 Nov 24 14:46 zsh_nitrous_test/project_one/css/min/about.css
-rw-rw-r-- 1 nitrous nitrous 2048 Nov 24 14:46 zsh_nitrous_test/project_one/css/style/about.css
-rw-rw-r-- 1 nitrous nitrous 2048 Nov 24 14:46 zsh_nitrous_test/project_one/html/main/about.html
-rw-rw-r-- 1 nitrous nitrous 2048 Nov 24 14:46 zsh_nitrous_test/project_one/html/public/about.html
-rw-rw-r-- 1 nitrous nitrous 2048 Nov 24 14:46 zsh_nitrous_test/project_two/css/min/about.css
-rw-rw-r-- 1 nitrous nitrous 2048 Nov 24 14:46 zsh_nitrous_test/project_two/css/style/about.css
-rw-rw-r-- 1 nitrous nitrous 2048 Nov 24 14:46 zsh_nitrous_test/project_two/html/main/about.html
-rw-rw-r-- 1 nitrous nitrous 2048 Nov 24 14:46 zsh_nitrous_test/project_two/html/public/about.html
```

This can be expanded further to include **multiple** strings at the beginning of file names. 

The pipe `|` character enclosed in brackets `()` separating the two strings are the operators for this:

```command
ls -l zsh_nitrous_test/*/*/*/(ab|he)*.*
```

Outputting:

```
-rw-rw-r-- 1 nitrous nitrous 2048 Nov 24 14:46 zsh_nitrous_test/project_one/css/min/about.css
-rw-rw-r-- 1 nitrous nitrous 2048 Nov 24 14:46 zsh_nitrous_test/project_one/css/style/about.css
-rw-rw-r-- 1 nitrous nitrous 2048 Nov 24 14:46 zsh_nitrous_test/project_one/html/main/about.html
-rw-rw-r-- 1 nitrous nitrous 2048 Nov 24 14:46 zsh_nitrous_test/project_one/html/public/about.html
-rw-rw-r-- 1 nitrous nitrous    0 Nov 24 14:46 zsh_nitrous_test/project_one/js/lib/helper.js
-rw-rw-r-- 1 nitrous nitrous    0 Nov 24 14:46 zsh_nitrous_test/project_one/js/test/helper.js
-rw-rw-r-- 1 nitrous nitrous 2048 Nov 24 14:46 zsh_nitrous_test/project_two/css/min/about.css
-rw-rw-r-- 1 nitrous nitrous 2048 Nov 24 14:46 zsh_nitrous_test/project_two/css/style/about.css
-rw-rw-r-- 1 nitrous nitrous 2048 Nov 24 14:46 zsh_nitrous_test/project_two/html/main/about.html
-rw-rw-r-- 1 nitrous nitrous 2048 Nov 24 14:46 zsh_nitrous_test/project_two/html/public/about.html
-rw-rw-r-- 1 nitrous nitrous    0 Nov 24 14:46 zsh_nitrous_test/project_two/js/lib/helper.js
-rw-rw-r-- 1 nitrous nitrous    0 Nov 24 14:46 zsh_nitrous_test/project_two/js/test/helper.js
```

The possibilities with these operators are endless, but are best used when you have a large file-system and need to find or select very specific files for use inside of commands. 

All available operators are listed at:

>[Zsh - 14.8.1 Glob Operators](http://zsh.sourceforge.net/Doc/Release/Expansion.html#Glob-Operators)

 <h2 id=”6”> 6 - Glob Qualifiers</h2>
Qualifiers make globbing a bit more varied. it's the same concept as before but relates to file properties rather than just the names. Qualifiers appear in brackets most frequently as part of parameter.

To see only the empty files in project two of our test directories run:

```command
ls -l zsh_nitrous_test/project_two/*/*/*(L0)
```

Here the `L` indicates file size and `0` is the amount - aka empty files.

``` Output
-rw-rw-r-- 1 nitrous nitrous 0 Nov 24 14:46 zsh_nitrous_test/project_two/js/lib/helper.js
-rw-rw-r-- 1 nitrous nitrous 0 Nov 24 14:46 zsh_nitrous_test/project_two/js/lib/main.js
-rw-rw-r-- 1 nitrous nitrous 0 Nov 24 14:46 zsh_nitrous_test/project_two/js/test/helper.js
-rw-rw-r-- 1 nitrous nitrous 0 Nov 24 14:46 zsh_nitrous_test/project_two/js/test/main.js
```

To add more to this qualifier try out:

```command
ls -lh zsh_nitrous_test/project_two/*/*/*(LK+3)
```

The `k` indicates kilobytes, `+` means greater than, and `3` is the amount. So this shows us files greater than three kilobytes in size:

``` Output
-rw-rw-r-- 1 nitrous nitrous 4.0K Nov 24 14:46 zsh_nitrous_test/project_two/css/min/contact.css
-rw-rw-r-- 1 nitrous nitrous 4.0K Nov 24 14:46 zsh_nitrous_test/project_two/css/style/contact.css
-rw-rw-r-- 1 nitrous nitrous 4.0K Nov 24 14:46 zsh_nitrous_test/project_two/html/main/contact.html
-rw-rw-r-- 1 nitrous nitrous 4.0K Nov 24 14:46 zsh_nitrous_test/project_two/html/public/contact.html
```

There are lots of different qualifiers, not just ones related to size. 

Change the permissions on these files:

```command
chmod 777 zsh_nitrous_test/project_two/html/public/*
```

Enter this qualifier to see which files in our test directories have had dangerous user permissions set on them:

```command
ls -l zsh_nitrous_test/project_two/*/*/*(RWX)
```

`RWX` means full read, write, and execute permissions for their user group and other. Which is of course bad for security if this were files hosted on a real web server! 

``` Output
-rwxrwxrwx 1 nitrous nitrous 2048 Nov 24 14:46 zsh_nitrous_test/project_two/html/public/about.html
-rwxrwxrwx 1 nitrous nitrous 4096 Nov 24 14:46 zsh_nitrous_test/project_two/html/public/contact.html
-rwxrwxrwx 1 nitrous nitrous 1024 Nov 24 14:46 zsh_nitrous_test/project_two/html/public/homepage.html
```

All available qualifiers are listed at:

>[Zsh - 14.8.7 Glob Qualifiers](http://zsh.sourceforge.net/Doc/Release/Expansion.html#Glob-Qualifiers)

<h2 id=”7”> 7 - Modifiers</h2>
These change the normal expected end result of command sequences. They are not dissimilar in how they appear to the "Qualifiers" from before, as they are enclosed in brackets. 

The modifier to return file extensions is `:e` so try:

```command
print zsh_nitrous_test/project_one/js/lib/*(:e)
```

This returns two JavaScript file extensions - meaning we now know there are two `.js` files here as expected. 

`(:h)` is a modifier that alters the output to return the *parent* folder of the file:

```command
print -l zsh_nitrous_test/project_two/css/*/homepage.css(:h)
```

Telling us that there are two parent folders with the `homepage.css` file inside. 

```Output
zsh_nitrous_test/project_two/css/min
zsh_nitrous_test/project_two/css/style
```

Some modifiers such as this `:h`"head" modifier can be repeated to continue the pattern:

```command
print -l zsh_nitrous_test/project_two/css/style/homepage.css(:h:h)
```

Meaning the parent folder of the original parent folder (`style`) is returned with this repetition:

``` Output
zsh_nitrous_test/project_two/css
```

Lastly qualifiers and modifiers can be combined together, as they are not mutually exclusive:

```command
print -l zsh_nitrous_test/project_two/css/*/homepage.css([1]:h)
```

With this, the parent directory of the first file only (`min`) is retrieved. 

``` Output
zsh_nitrous_test/project_two/css/min
```

All further modifiers are listed at:

>[Zsh - 14.1.4 Modifiers](http://zsh.sourceforge.net/Doc/Release/Expansion.html#Modifiers)

<h2 id=”conclusion”>Conclusion</h2>
The real challenge here is in becoming familiar with all these different Z shell concepts and then finding practical use for them. The test script directories are valuable for practicing and applying some of this so continuing to experiment there is a good start. 

Other than that if you liked this article you may want to learn how to change the look and feel of the environment to further enhance your Z shell workflow. Which is covered in another Nitrous tutorial: 
>[Customising Z Shell (zsh) in Nitrous Projects](https://community.nitrous.io/tutorials/customising-z-shell-zsh-in-nitrous-projects)


