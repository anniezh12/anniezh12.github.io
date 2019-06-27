---
layout: post
title:      "Setting up environment for a React App"
date:       2019-06-27 18:52:13 -0400
permalink:  setting_up_environment_for_a_react_app
---



Setting up Git bash
--------------

Git for Windows provides a BASH emulation used to run Git from the command line. In order to download Gitbash visit
https://gitforwindows.org/ and download it.

![](https://imgur.com/uO7ynxA.jpg)

Simply proceed with the default setting until the final installation. Once installed Gitbash can be opened as shown in the following image.

![](https://imgur.com/VcRL31y.jpg)

## Checking Node and NPM 

In order to create a React app using create-react-app command first we need to have both nodejs and npm. 
We can see if the node and npm exist by simply using following linux commands in our bash terminal

```node -v```
v10.13.0  // showing that we have a node version

```npm -v```
6.4.1           // showing we have npm installed

In case if we dont have any of them we can install them using "$ apt-get install node" or "$ sudo apt-get install node"

> Note: $ is a shorthand for terminal and  is not the part of command
 
 NPM (Node Package Manager), has frequent updates and should be updated on a regular basis using following command.
 
 ```$ npm install npm@latest -g```
 
 See following image for visual representation.
 
 ![](https://imgur.com/hBdZGrD.jpg)
 
 
### Creating our first React App
 
 Now we can easily create our React app using following command
 
 ```$ npx create-react-app weather-app```
 
 A new React app weather-app will be created which can be started by first changing directory to it and then using ```$ npm start```
 
 ![](https://imgur.com/XJQ01yP.jpg)
 
 A new react app will open in the browser with the default React Application page.
 
 ![](https://imgur.com/rE8eW15.jpg)
 
###  Opening  React App in a Text Editor
 
 We can use any text editor to make changes to a react app but I will use Sublime Text here. Simply navigate to this newly created app and we can see the structure of the React app here. 
 
 ![](https://imgur.com/TEw4uuN.jpg)
 
 As we can see index.js in the above image which is the first file that is executed when we start a react app. 
 Here it renders App component in the ```root``` div, which is located in index.html. 
 
 ![](https://imgur.com/QVvnnhG.jpg)
 
 
 Moreover, node_modules folder contain all the modules that can be imported in a React application. Two of such modules are imported by default and can be seen in index.js as 'react' and 'react-dom'.
 
 
 
 




