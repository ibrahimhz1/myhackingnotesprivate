---
title: Untitled
date: 2026-06-12
tags:
  - 
publish: true
draft: false
description: ""
---
This lab contains a DOM-based cross-site scripting vulnerability in the search query tracking functionality. It uses the JavaScript `document.write` function, which writes data out to the page. The `document.write` function is called with data from `location.search`, which you can control using the website URL.

To solve this lab, perform a cross-site scripting attack that calls the `alert` function.

Explore the application and understand it

**Basic understanding**: 

---
Type of Application: Blogging Platform
Users can add comments for the posts
The homepage have search feature (to search the blog posts)

---

