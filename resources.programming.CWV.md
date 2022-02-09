---
id: efff1c7e-c48d-4dc3-84d7-d7de64cfb8da
title: CWV
desc: ''
updated: 1625244786402
created: 1625244134970
---

# Atrybuty linka

## Preload

```html
<link rel="preload"> 
```
**Używać, gdy potrzebujemy jakiegoś zasobu natychmiast.**

Tells the browser to download and cache a resource (like a script or a stylesheet) as soon as possible. It’s helpful when you need that resource a few seconds after loading the page, and you want to speed it up.

The browser doesn’t do anything with the resource after downloading it. Scripts aren’t executed, stylesheets aren’t applied. It’s just cached – so that when something else needs it, it’s available immediately.

**Don’t overuse it.** Preloading everything won’t magically speed up the site – instead, it will likely prevent the browser from scheduling everything smartly.


## Prefetch

```html
<link rel="prefetch"> 
```
**Używać, dla zasobów pochodzących z innych stron.**

Asks the browser to download and cache a resource (like, a script or a stylesheet) in the background. The download happens with a low priority, so it doesn’t interfere with more important resources. It’s helpful when you know you’ll need that resource on a subsequent page, and you want to cache it ahead of time.

The browser doesn’t do anything with the resource after downloading it. Scripts aren’t executed, stylesheets aren’t applied. It’s just cached – so that when something else needs it, it’s available immediately.

**Use it for resources from other pages.**

## Preconnect

```html
<link rel="preconnect">
```
**Używać w przypadku, gdy ważne zasoby jak style lub skrypty zaciągamy z third-party domains**

 asks the browser to perform a connection to a domain in advance. It’s helpful when you know you’ll download something from that domain soon, but you don’t know what exactly, and you want to speed up the initial connection.

A browser has to set up a connection when it retrieves something from a new third-party domain. (A third-party domain is a domain that’s different from the one your app is hosted on.) This may happen when a site uses a font from Google Fonts, loads React from a CDN, or requests a JSON response from an API server.

**Use it for domains you’ll need shortly.** <link rel="preconnect" /> will help you when you have an important style, script, or image on a third-party domain, but you don’t know the resource URL yet.

Source:https://3perf.com/blog/link-rels/