---
layout: post
title:  "How to string keyins together or create keyin scripts"
date: 2006-09-02
categories: MicroStation 键入
tags: MicroStation 脚本 key-in
excerpt: MicroStation键入命令技巧及创建命令脚本文件。
author: 测量老覃
---
* content
{:toc}

**Original Article Date: January 2001**

**Updated:  August 2006**

Originally titled The Power of Keyins - Part 2 and published in the July 2000 issue of the MicroStation Manager Magazine, this tutorial covers how to create script files to streamline your workflows.

The ability to string keyins together is often referred to as a keyin script or action string. Whatever terminology you use, it's all a fairly easy to understand. In addition, by understanding the various options and components of action strings, you'lll be able to create very powerful scipts to streamline your workflow and minimize repetitive tasks. Let's get started!

### Action Strings
Whatever MicroStation does when you activate a tool, menu item or function key can be referred to as an Action String. In other words, there is always some kind of keyin that executes a set of instructions when one of these items is selected. A great example of an action string is the F8 function key which toggles the display of construction elements on or off with this keyin: set construct toggle; selview all; update all where...

set construct toggle Is the actual command to turn the construction elements on or off.
selview all Instructs MicroStation to apply the keyin to all views.
update all Updates all the views.

![](/img/2022/2022-09-10-15-50-40.png)

### Action Types
There are a variety of action types associated with tools, view controls, menu items and function keys. Note that some of these pre-date the current version of MicroStation and are shown with a (*).

Action Type | Syntax | Description
--- | --- | ---
Command Entry Keyin | E,[keyin] | Whatever keyin is specified, it is always activated regardless of the state of the current tool.
Terminated Keyin | T,[keyin] |	A terminated keyin is one that prompts the user for additional information such as the radius of a circle, text, or an answer to a yes/no question.
Unterminated Key-in | K,[keyin]	| An un-terminated string of characters waits for the user to finish the string.
Print Message | M,ER[message] | Prints the message on the left side of the status bar.
Print Message | M,ST[message] | Prints the message on the right side of the status bar in MicroStation /J and in the Message Center in MicroStation V8.
Print Message | M,CF[message] | Prints the message as the left side a MicroStation prompt.
Print Message | M,PR[message] | Prints the message as the right side of a MicroStation prompt.
Place Active Cell - Absolute | C,[cellname] | Sets the Active Cell to the specified cellname and activates the Place Cell Absolute.
Place Active Cell - Relative | R,[cellname] | Sets the Active Cell to the specified cellname and activates the Place Cell Relative keyin.
Tutorial (*) | D,[tutorial] | Activates the specified tutorial.
User command (*) | U,[ucm_num] | Activates user command defined in the user command index file for [ucm_num]
Primitive (*) | P,[primitive] | Activates keyin by its IGDS primitive name

_(*) MicroStation Pre-V8_

A couple of things to point out with Action Types:
If no type is specified, then MicroStation assumes that it will be a command entry keyin (E).

For some terrific examples of action strings, take a look at the sample tablet menu in this file: ...\workspace\system\menus\dgn\v8menu.dgn. You'll want to navigate to the Command model and view the Command Text level.

![](/img/2022/2022-09-10-15-51-02.png)

### Action Type Options
In addition to the Action Types, there are several options which can be used following the E, T or K action types. These options can be positioned anywhere in a multiple action string and must be followed with a semi-colon.

Option | Description
--- | ---
/ | A forward slash will cause MicroStation to wait for user input such as entering a value.
/d | Causes MicroStation to wait for a data point.
/k | Makes MicroStation wait for a keyin
% | This is the same as the forward slash except that MicroStation won't display its normal prompts.
 

### Stringing Multiple Keyins Together
There are two ways of stringing keyins together. The first is to simply keyin them all in on one line and separate each keyin with a semi-colon. This is typically referred to as an action string. The other is to use a script file which is described below. Seriously, that's all there is to it.
Take this for example, this action string sets up your attributes and then selects the correct drawing tool.
CO=3;LV=12;LC=2;WT=4;PLACE SMARTLINE

**Where:**
```
CO=3 Sets the Active Colour to number 3
LV=12 Sets the Active Level to 12
LC=2 Sets the Active Line Style to 2
WT=4 Sets the Active Weight to 4
PLACE SMARTLINE Activates the Place Smartline tool
```

And this example shows the use of the various action types. After setting up the active attributes, it places a circle with a 12 inch radius at a user-defined point:

```
CO=7;LV=23;LC=0;WT=1;E,Place Circle Radius;T,12;M,Place Manhole;%d;null
```

**Where:**
```
CO=7 Sets the Active Colour to number 7
LV=23 Sets the Active Level to 23
LC=0 Sets the Active Line Style to 0
WT=1 Sets the Active Weight to 1
E,Place Circle radius A command keyin which activates the Place Circle Radius tool
T,12 A terminated keyin that enters a radius of 12
M,Place Manhole Displays a message in the status bar which reads “Place Manhole”
%d MicroStation waits for a data point which defines the location of the manhole.
NULL After the arc has been placed, MicroStation executes the NULL key-in so that no tool or view control is active.
```

### Script Files
Simply put, a script file is a list of MicroStation keyins stored in an external text file such as the following two examples. Note that in this case, you can enter each keyin on a separate line rather than stringing them all together on one line as described above.

![](/img/2022/2022-09-10-15-51-25.png)

Script files can be executed a number of different ways:

Run from the key-in window by keying in @script_file. For example, the keyin: @c:/temp/my_commands.txt will immediatly kick off the list of MicroStation commands found in the file c:\temp\my_commands.txt. Note, be certain to include the full path if the file is not in the current working directory.

Run from a function key by simply editing a function key to execute the keyin: @script_file as described above.

Run the script file at the startup of MircroStation by appending the -s[script_file] command line switch as shown below.

![](/img/2022/2022-09-10-15-51-49.png)
 
Finally, you can run a script against multiple files by utilizing MicroStation V8's Batch Process Utility. This utility lets your create and run a script that can be executed on either individual files or entire directories full of files. In this case, the script file is referred to as a command file...All in all it's just the same thing.

![](/img/2022/2022-09-10-15-52-05.png)

Whatever your need is for stringing multiple keyins together, there's no doubt that utilizing script files will allow you to improve upon your workflows. Have fun!

[AskInga Article #37](https://communities.bentley.com/products/microstation/w/askinga/389/how-to-string-keyins-together-or-create-keyin-scripts)