---
layout: post
title: AJAX - Getting Data without Page Refresh
thumbnail: "assets/img/thumbnails/Ajax.jpg"
tags: [Ajax]
---

## Background

Have been working on both back-end and front-end projects for years, more and more circumstances do I face getting data from and posting data to the server. On traditional Django applications, I send data from a server to a page by rendering a HTML template, in a Django View. This is so convenient that I can develop a prototype in a very short amount of time. However, there are four issues when a project getting more complex:

- Back-end and front-end are **highly coupled**;
- Templates are rendered at the server side, **adding load** to the server. Remember, computing power of a server, e.g., AWS EC2, Azure cognitive services, is expensive for SMEs (small and medium enterprises);
- If pulling massive data from different Django models and rendering on a webpage, users will experience **very slow page loading**;
- Whenever getting data from and post data to a server, I encountered **page refresh**.

To resolve these issues, `Ajax` and `Django REST Framework (DRF)` are adopted to save the world. **Ajax** makes page refresh irrelevant. **DRF** decouples back-end and front-end. The decoupling makes web development an easier and much enjoyable process. Back-end server provides and processes data only, no page rendering involved. Front-end UI consumes and sends data only. In this way, back-end developers could focus on designing RESTful APIs while front-end developers can enjoy any framework they like, e.g., `React`, `Vue`, and `Angular`, for the development.

However, DRF is beyond the topic of this blog. Let's go back to three mostly used (personally) ways to make Ajax calls, to get data.

### 1. jQuery `$.ajax`

Many of us are already familiar with jQuery. `$.ajax` is a quick and intuitive solution to make AJAX calls. And, the API is just simple:

```js
loadDataFromServer: function() {
    $.ajax({
      method: 'GET',
      url: 'my_url_endpoint',  // A RESTful URI
      dataType: 'json',
      cache: false,
      success: function(result,status,xhr) {
        // process the result
      },
      error: function(xhr,status,error) {
        // process error
      }
    });
  }
```

This is pretty much everything we need. It is recommended if you are already using jQuery library in your front-end development. Of course, there are two short-handed versions: `$.get()` and `$.post()` at your disposal. Please refer to [jQuery AJAX Methods](https://www.w3schools.com/jquery/jquery_ref_ajax.asp) for other useful methods.

### 2. Fetch API

**Fetch API** is a new, simple and standardized API that aims to unify fetching across the web and replace `XMLHttpRequest`. The new API provides a more powerful and flexible feature set. It has a [polyfill](https://github.com/github/fetch) for older browsers and should be used in modern web apps. If you are making API calls in Node.js you should also check out [node-fetch](https://github.com/bitinn/node-fetch) which brings `fetch` to Node.js.

Here is what the modified API call looks like:

```js
loadDataFromServer: function() {
    fetch('my_url_endpoint').then(function(response){
        // process the response
    }).catch(function(error) {
	    // process error
    });
}
```

Please refer to [Mozilla - Fetch_API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API) for more information.

### 3. Axios

[Axios](https://github.com/axios/axios) is a **promise based** HTTP client for Node.js and browser. Like `fetch`, it can work on both client and server. It is officially recommended for `Vue.js`.

Here is how you make an API call using Axios:

```js
loadDataFromServer: function() {
    axios.get('my_url_endpoint').then(function(response){
      // process the response

    }).catch(function(error){
      // process the error

    });
}
```

Refer to its GitHub page for many other useful features.

## Conclusion

If you are using jQuery as a library in your font-end code, just use `$.ajax` to get the job done.

If you are using modern vanilla JavaScript (ES6/ES2015+) or `React`, `Fetch API (fetch)` is a good option.

If you are using `Vue.js` like I do, use `axios`.

However, there is not an arbitrary choice. You can use `fetch` or `axios` for either `Vue.js` or `React`, but avoid `$.ajax`.

## References

- [5 best libraries for making AJAX calls in React](https://hashnode.com/post/5-best-libraries-for-making-ajax-calls-in-react-cis8x5f7k0jl7th53z68s41k1)
- [jQuery AJAX Methods](https://www.w3schools.com/jquery/jquery_ref_ajax.asp)
- [Mozilla - Fetch_API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API)
- [Axios](https://github.com/axios/axios)
