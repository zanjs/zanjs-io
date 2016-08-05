title: 'Create a character voting app using React, Node.js, MongoDB and Socket.IO'
date: 2015-11-21 11:38:11
tags:
	- javascript
	- react
	- nodejs
	- mongodb
	- socket.io
categories:
	- javascript
---

### Update Notice 2 (November 12, 2015)

Tutorial has been updated to use [Babel](https://medium.com/@malyw/how-to-update-babel-5-x-6-x-d828c230ec53) 6.0 and
 React [Router 1.0](https://github.com/rackt/react-router/releases/tag/v1.0.0). For detailed tutorial updates see November 12, 2015 notes below.

### Update Notice 1 (October 19, 2015)

Tutorial has been updated to use React 0.14 and React Router 1.0-rc3 that introduced breaking changes. 
For detailed tutorial updates see October 19, 2015 notes below.

## Overview


In this tutorial we are going to build a character voting app (inspired by Facemash and Hot or Not) 
for [EVE Online](http://www.eveonline.com/) - a massively multiplayer online game. 
Be sure to play this awesome soundtrack below to get yourself in the mood for this epicly long tutorial.


While listening to this soundtrack, imagine yourself mining asteroid belts in deep space while 
keeping a lookout for pirates on the radar, researching propulsion system blueprints at the station's facility, 
manufacturing spaceship components for capital ships, placing buy & sell orders on the entirely 
player-driven market where supply and demand govern the game economics, hauling trade goods from a remote solar system 
in a massive freighter, flying blazingly fast interceptors with a microwarpdrive or powerful battleships armored to 
the teeth, optimizing extraction efficiency of rare minerals from planets, 
or fighting large-scale battles with thousands of players from multiple alliances. That is *** EVE Online ***.



Each player in EVE Online has a 3D avatar representing their character. 
This app is designed for ranking those avatars. Anyway, your goal here is to learn about Node.js, 
React and Flux, not EVE Online. But I will say this: "Having an interesting tutorial project is just as important, 
if not more so, than the main subject of the tutorial". 
The only reason I built the original New Eden Faces app is to learn Backbone.js and the only reason I built the
 TV Show Tracker app is so that I could learn AngularJS. To me, either one of these projects is far more 
 interesting than a simple todo app that everyone seems to be using these days.
 
 
 One thing that I have learned — between screencasts, books and training videos, 
 nothing is more effective than building a small project that you are passionate about to learn a new technology.
 
 