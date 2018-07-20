---
title: "Routine: July 21st"
date: "2018-07-20T17:36:22.858Z"
layout: post
draft: true
path: "/posts/routine-2018-07-21"
tags:
- "routine"

description: "vue, pwa and rust"
---


## vue and pwa, rust at first try
### vue-cli 3.0 and pwa
> [Overview | Vue CLI 3](https://cli.vuejs.org/guide/)

vue-cli 3 is fantastic! It provides a scaleable scaffold and newly-added useful commands such as `vue ui`. 

Besides `vue init` now we can use `vue create` to init a project with option and use `vue add` to easily extend the project thanks for plugins like `@vue/pwa` and `vuetify`.

You can easily upgrade your app to pwa without side effects by plugin `@vue/pwa`.

- Based on [register-service-worker](https://www.npmjs.com/package/register-service-worker), this plugin do encapsulation and provides a few simple and clear lifecycle (ready, cached, updated, offline, error) for you to custom  behavior for your code.
- Remember to prevent caching for your server or your service worker will update after 24 hour in default: [pwa/prevent_caching.md at development · vuejs-templates/pwa · GitHub](https://github.com/vuejs-templates/pwa/blob/development/docs/prevent_caching.md)
- Remember to verify your [baseUrl in vue.config.js](https://cli.vuejs.org/config/#baseurl) and src in manifest.json to make static file serve properly.

Will try some other pwa related api in future, maybe push at first.

### Rust
Rust have an excellent support for js and webassembly. Besides, the syntax is more familiar to me than c++ and cargo is a very nice package manager. So I decide to spend a few time on it (mostly because of webassembly).  