---
layout: post
title: Django Template Inheritance and Webpage Structure
tags: [Django, Python]
---

Django templates are very powerful for speed up a website prototype, e.g., template literals, template filters, template inheritance. Especially, the template inheritance, makes it much easy to build a multi-page web application with minimal code repetition.

I personal have developed the following webpage structure based on the template inheritance. It is considered the best practice.

First of all, the overall and simplified web application can be decomposed into primitive blocks as the following figure shows.

![webpage structure](../../../assets/img/thumbnails/django-website-structre.png)

Namely, `MAIN_HEADER`, `SIDEBAR`, `Breadcrumb`, `message`, `BODY_CONTENT` and `FOOTER`. Where, `MAIN_HEADER` includes children blocks `LOGO`, `MESSAGES (hub)` and `Account_bar`.

With this design pattern, the HTML markup blocks can be configured as a `base.html` as following

![webpage structure](../../../assets/img/thumbnails/template-inheritance.png)

It acts as an abstract base structure of the website.

Normally, I will add another layer to wrap up a `site_base.html` to extend the `base.html` as the real webpage base. This one includes static file blocks. Pay attention to the colored items.

![webpage structure](../../../assets/img/thumbnails/site-base.png)

For an actual page, it extents the `site_base.html` with block implementations.

In this way, a multi-page website can be effectively developed.
