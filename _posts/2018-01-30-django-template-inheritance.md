---
layout: post
title: Django Template Inheritance and Webpage Structure
thumbnail: "assets/img/thumbnails/django-website-structre.png"
tags: [Django, Python]
---

Django templates are very powerful for speeding up a website prototype, e.g., template literals, template filters, and template inheritance. Especially, the template inheritance, makes it much easier to build a multi-page web application with minimal code repetition.

I personal have developed the following webpage structure based on the template inheritance as a best practice.

1.  First of all, the overall and simplified web application can be decomposed into primitive blocks as the following figure shows.

![webpage structure](../../../assets/img/thumbnails/django-website-structre.png)

Namely, `MAIN_HEADER`, `SIDEBAR`, `Breadcrumb`, `message`, `BODY_CONTENT` and `FOOTER`. Where, `MAIN_HEADER` includes children blocks `LOGO`, `MESSAGES (hub)` and `Account_bar`.

2.  With this design pattern, the HTML markup blocks can be configured as a `base.html` as following

![webpage structure](../../../assets/img/thumbnails/template-inheritance.png)

It acts as an abstract base structure of the website.

3.  I added another layer to wrap up a `site_base.html` to extend the `base.html` as the webpage base for all pages. This one includes static file blocks. Pay attention to the colored items.

![webpage structure](../../../assets/img/thumbnails/site-base.png)

4.  For an actual page, e.g. `home.html` or `about.html`, it extents the `site_base.html` with block implementations.

In this way, a multi-page website can be effectively developed.
