---
layout: page
title: About
permalink: /about/
---

 <img src="../images/me-jan2019-sm.png" alt="ProfilePic" width="150px" style="border-radius: 50%;">

Hi, I'm Andrew. I'm a recent graduate of the Information and Computer Systems program at Camosun College in Victoria, BC, and I'm currently completing a Software Engineering internship at [Change.org](https://change.org). I'm passionate about working with modern web technologies to craft great user experiences.

My intent with this site is to feature my personal and professional software development projects and to give me a place to reflect on what I've been learning. Check out the [projects page]( {{ "./projects" | relative_url }} ) to see some demos of my latest side projects. My GitHub account with all the source code can be found [here](https://github.com/a-bishop).

## Contact

Have questions about my portfolio or want to know more about me? Please don't hesitate to get in touch :)

<form action="https://getform.io/f/ab7dccdb-e06b-49f5-8433-2d2bc2eaa19c" method="POST" id="usrform">
    <div class="form-row">
      <div class="form-group col-md-6">
        <label for="formName">Name *</label>
        <input id="formName" class="form-control" type="text" name="name" placeholder="Enter name" required>
      </div>
      <div class="form-group col-md-6">
        <label for="formEmail">Email *</label>
        <input id="formEmail" class="form-control" type="email" name="email" placeholder="Enter email" required>
      </div>
    </div>
    <div class="form-group">
      <label for="formMsg">Send a message</label>
      <textarea id="formMsg" class="form-control" name="message" placeholder="Enter message"></textarea>
    </div>
    <div class="form-group">
      <div class="custom-control custom-checkbox">
        <input type="hidden" name="resume" value="no">
        <input type="checkbox" class="custom-control-input" id="resumeCheck" name="resume" value="yes" checked>
        <label class="custom-control-label" for="resumeCheck">Request CV</label>
      </div>
    </div>
    <button type="submit" class="btn btn-primary">Send</button>
</form>
