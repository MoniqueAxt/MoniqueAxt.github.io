---
layout: page-fullwidth
title:  "Blackjack game in C#"
subheadline:  "Projects"
teaser: "Love to gamble?"
breadcrumb: true
categories:
    - portfolio
header:
   image_fullwidth: header_blackjack.jpg
   title: "<br> monique axt <br> software engineer"
image:
    thumb: "csharp_Logo.jpg"
gallery:
    - image_url: gallery-BJ-Main_screen_1.jpg
      caption: Main screen (game not started)
    - image_url: gallery-BJ-Main_screen_2.jpg
      caption: Main screen (game in play)
    - image_url: gallery-BJ-Game_settings.jpg
      caption: Game settings
    - image_url: gallery-BJ-database.jpg
      caption: Database view
---
<a name="top"></a>

<div class="row">
<div class="medium-4 medium-push-8 columns" markdown="1">
<div class="panel radius" markdown="1">
**Table of Contents**
{: #toc }
*  TOC
{:toc}
</div>
</div><!-- /.medium-4.columns -->
<div class="medium-8 medium-pull-4 columns" markdown="1">

A Blackjack game to idle your time away. Play as a human yourself, or set up a game of bots and see who wins.
Written with C# and .NET to play around with this specific programming language, learn about turn-based game logic and work with a database.


## Source code

<img src="{{ site.urlimg }}/GitHub-Mark-32px.png" alt="Github repo">
<a href="https://github.com/MoniqueAxt/Blackjack" target="_blank">Github repo link</a>

<img src="{{ site.urlimg }}/Blackjack_Demo_short2.gif" alt="Github repo">

## Design
The WPF application is designed with separation of logic and presentation in mind, via implementation of a three-layer architecture: presentation layer, business logic layer, and data access layer. Delegates are used together with events to create an event-driven application.

## Game
In addition to making the application modular and maintainable, the separation of layers was important to allow interchangeability of the GUI for the game in the future. Inspiration for handling the running of the game was taken from [roguelike games](https://en.wikipedia.org/wiki/Roguelike), using a turn-based game loop.

## Data logging
Game events are logged to a text file and to output. (Previous version: serialization was used to persist data in binary and XML formats, enabling the loading and saving of games from file).

## Database
A database manager class using LINQ in conjuction with a DbContext class implements SQL database functionality to allow the saving, deleting, editing and searching of games stored locally. 

## Misc.
A simple timer and animation was used to shift colours in the banner of the main game window.

## Further development
 - loading of games from the database
 - saving mid-game (involves storing moves yet to be made but already queued)
 - implementation of betting on each round (the actual fun part of playing)

## Gallery
{% include gallery %}


