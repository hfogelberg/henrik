---
title: "Setting Up a Mean Machine"
date: 2014-04-19T17:59:24Z
draft: false
---
# Setting up a MEAN machine
I always find it a pleasant experience to set up a development machine from scratch -to just get things the way you want it. At the back of your  mind you always know of things that you did wrong and it can be a pain to remove all the junk and clear out the trial and error settings.

So this post is a description of what I did to set up an Node, Angular, Mongo development environment on my new Mac. The list is long!

## 1. Terminal
 As pretty much all the installation will be done from the terminal I began by installing the terminal I prefer which is [iTerm2](https://www.iterm2.com/).

As a default iTerm2 comes with a plain black and white theme, but I’m hooked on Solarized. If you have the same inclination as I do, you can import that theme. The setup is a bit quirky but works
1. Download [Solarized](https://github.com/altercation/solarized/tree/master/iterm2-colors-solarized).
2. Go to Preferences and select Profiles. Click on the Colors tab. On that tab choose Load presets and select import in the drop down menu. Navigate to your Downloads folder and go to the iterm2 folder in the Solarized downloads. Select either the dark or light theme. 
3. Restart iTerm2. Now you would expect to sees Solarized. Wrong. You again need to go to Prefereneces -> Profiles ->Colors and select Load Presets. This time Solarized should be in the drop down menu.

## 2. XCode command line tools
I don’t plan to be doing any iOS development at the moment, so installing the **huge** XCode set up is unnecessary. However the command line parts of XCode are required by Homebrew. At the terminal just run 
`xcode-select —install`

## 3. Homebrew
Homebrew is my preferred package manager on OSX. It's installed by running `ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"`.

## 4. Node
With Homebrew in place the rest is just a walk in the park. To install Node: `brew install node`.

To check that everything is Ok just run  `node` from the command line.

## 5. Mongodb
No points awarded for figuring out how to install Mongodb:
`brew install mongodb`.

You need to manually create the folder for the database files and set the permissions:
`$sudo mkdir -p /data/db`
`$sudo chown -R $USER /data/db`

## 6. Nodemon
Nodemon is an essential little tool, written by Remy Sharp which restarts the node server whenever a file is changed.

You install it with NPM: `npm installl -g nodemon`.

## 7. HTTPie
Strictly speaking [HTTTPie] doesn’t really have anything to do with Node development. However it is a really useful little tool to make HTTP calls from the command line with a readable syntax. It’s a nice alternative to Postman if you prefer using the command line.

You install with Homebrew: `brew install httpie`.

## 8. Github
There’s an excellent step by step [walktrough](https://help.github.com/articles/set-up-git/) on Github describing how to access Github from a new machine if you have an existing account.

## 9. Graphics
Many of the sites I work on allow users or admins to upload images. Pretty much all of them, come to think of it. I use the Node package [gm] for scaling images. Before installing the Node package you need GraphicsMagick and/or ImageMagick .

````
brew install graphicsmagick
brew install imagemagick
````

To use it in a Node application `npm install --save gm`.

## 11. Grunt
Grunt is a very useful task runner, used for automation. You get along without it, but it does make life a bit more comfortable. Also it’s required by Yeoman.

How do you install it? 

Wrong answer. This time it’s NPM: `npm install -g grunt-cli`.

## 12. Yeoman
Sometimes I use Yeoman to scaffold a new Node projects. Wrong, you don’t use Homebrew to install Yeoman. 

Just like Grunt NPM is used for the installation `npm install -g yo`.

