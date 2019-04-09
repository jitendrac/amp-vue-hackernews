# AMP Hacker News client PWA
Hacker News client app built with Nuxt.js (and Vue.js) which generates fast and lightweight [AMP pages](https://amp.dev/) as a [Progressive Web Application](https://amp.dev/documentation/guides-and-tutorials/optimize-and-measure/amp_to_pwa.html)

The idea behind this project is to provide a developer friendly "recipe" to generate and serve a PWA with AMP using Javascript.

## Why Vue.js and Nuxt.js
> Simplicity is the key to brilliance

1. Because of Vue.js simple, intuitive and reliable template engine. 
2. Because of Nuxt.js presets and conventions, which let you stay focused on the most important thing: enjoy coding your app
3. Because javascript is the programming language I love and use every day

## Main AMP requirements
* Render all pages with [required markup](https://amp.dev/documentation/guides-and-tutorials/start/create/basic_markup.html)
* Loading stylesheet from an external file is not allowed
* Custom javascript is not allowed, only amp `custom-element` scripts can be used
* When is supposed to be so, render the right amp `custom-element` in place of native HTML elements
* amp `custom-elements` scripts must be imported only if they're used in page

## AMP PWA recipe - ingredient list
Is a Server Side Rendered Vue.js application in [universal mode](https://nuxtjs.org/guide/#server-rendered-universal-ssr-).

### [+] webpack bundles removal
Since custom javascript is not allowed, client side webpack bundles imports must be removed from DOM. Of course, would be better to avoid client side build at all but I didn't find the way to exclude this step. See `ampHtmlSanitizer.js` module which is responsible for building and expose page absolute URL.

### [+] remote data and vuex store
In server side rendering everything works as usual with any other Nuxt.js app. [Fetch method](https://nuxtjs.org/api/pages-fetch#the-fetch-method) is used to fire an async vuex action to call the APIs and gather request data in order to render the page. If you are familiar with Vue.js and Nuxt.js you should easily understand the request and render flow in SSR

### [+] client side interaction replaced with amp-bind and amp-state
Client side interaction can be done using amp-bind and amp-mustache-templates. In this app you can see them in action by navigating to any `/item/:id` route and clicking on "toggle replies" link: on link tap fires setState action, which toggles the underneath block visibility and amp-list gets remote data using a remote endpoint. Comment replies are shown using mustache templates under `components/amp-mustache-templates` folder by referencing their id (filename including extension)

[Canonical and amphtml page link meta tags](https://amp.dev/documentation/guides-and-tutorials/optimize-and-measure/discovery#linking-pages-with-link) are handled at layout level by using the `appConfigInjector.js` module

## Build Setup

``` bash
# install dependencies
$ yarn install

# serve with hot reload at localhost:8000
$ yarn run dev

# build for production and launch server
$ yarn run build
$ yarn start

# generate static project
$ yarn run generate
```

For detailed explanation on how to deal with Nuxt.js, checkout [Nuxt.js docs](https://nuxtjs.org)