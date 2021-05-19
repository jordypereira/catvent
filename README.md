# catvent
> 2 Dec 2018 

> An advent calendar with cats.  
Add dangerous or irrelevant cat pictures to [the blacklist](/static/blacklist.json)

## About
This was my project to try out Nuxt.  

You can flip over a card with a random cat picture/gif every day, until the 25th.  
The cats are chosen from a random set depending on the day you click on.  

I save which cat pictures you've clicked on with 1 long string in your localStorage.  
That way I limit the amount of data to the bare minimum. 

When you visit the page, all the images get preloaded and saved in the same string. 
That way, flipping the images becomes an instant experience. 

Big shout out to [The Cat Api](https://thecatapi.com)

## Setup

copy .env.example to .env and fill in an api key from [the cat api](https://thecatapi.com/)  
fill in an google analytics key or remove the line in the nuxt.config.js

## Features

+ Load in a different set of random kitties per day.
+ Preload all images.
+ Save clicked images in 1 string in Local Storage.
+ The Cat Face is drawn with CSS.
+ CSS hover animation.
