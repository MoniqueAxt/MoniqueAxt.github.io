---
#
# Use the widgets beneath and the content will be
# inserted automagically in the webpage. To make
# this work, you have to use › layout: frontpage
#
layout: frontpage
header:
  image_fullwidth: green-250767_1280.jpg
widget1:
  title: "Projects"
  url: '/blog/'
  image: widget-1-302x182.jpg
  text: '..'
widget2:
  title: "About"
  url: '/info/'
  image: widget-coding-glasses.jpg
  text: 'How did a South African working in natal fitness find her way to coding?'
widget3:
  title: "Github profile"
  url: 'https://github.com/MoniqueAxt'
  image: widget-github-303x182.jpg
  text: '..'
#
# Use the call for action to show a button on the frontpage
#
# To make internal links, just use a permalink like this
# url: /getting-started/
#
# To style the button in different colors, use no value
# to use the main color or success, alert or secondary.
# To change colors see sass/_01_settings_colors.scss
#
#callforaction:
  #  url: https://tinyletter.com/feeling-responsive
  #  text: Inform me about new updates and features ›
  #  style: alert
permalink: /index.html
#
# This is a nasty hack to make the navigation highlight
# this page as active in the topbar navigation
#
homepage: true
---
