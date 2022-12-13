---
layout              : page
title               : "Contact"
meta_title          : "Contact and use our contact form"
subheadline         : "Get in touch"
#subheadline         : "Contact Form"
#teaser              : "Get in touch with me"
permalink           : "/contact/"
header:
    image_fullwidth: header-contact.jpg
    title: "<br> monique axt <br> software engineer"
---
{{ site.email_title | default: "Email" }}: <a href="mailto:{{ site.email }}" target="_blank">{{ site.email | escape }}</a>

Email me directly or use the form below.

<form accept-charset="UTF-8" action="https://getform.io/f/aab5ebb1-09eb-4849-9b3e-3d39720ecf78" method="POST" enctype="multipart/form-data" target="_blank">
          <div class="form-group">
            <label for="inputEmail" required="required">Your email address</label>
            <input type="email" name="email" class="form-control" id="inputEmail" aria-describedby="emailHelp" placeholder="Enter email">
          </div>
          <div class="form-group">
            <label for="inputName">Your name</label>
            <input type="text" name="name" class="form-control" id="**exampleInputName**" placeholder="Enter your name" required="required">
          </div>
          <div class="form-group">
            <label for="inputMessage">Message</label>
            <input type="text" name="message" class="form-control" id="inputMessage" placeholder="Enter your message" required="required">
          </div>
          <input type="hidden" name="_gotcha" style="display:none !important">
          <button type="submit" class="btn btn-primary">Submit</button>
        </form>
