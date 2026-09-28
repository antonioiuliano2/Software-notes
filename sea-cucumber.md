---
description: Event Display
---

# Sea Cucumber

## Introduction

The Event Display is available here: [https://github.com/ShipSoft/sea-cucumber](https://github.com/ShipSoft/sea-cucumber). The README is very clear, so I will provide here just a few notes:

### Front-ends

There are two front-ends:

* REve, classical ROOT display;
* Web-data, very intituive and ideal for web-hosting.&#x20;

Currently web-data **does not support** TGeoCompositeShapes (Subtractions and Intersection in the new GeoModel format). They are skipped completely in the mesh conversion.



## Hosting

I am currently hosting my SND event display in my github-pages. Now i can simply edit my github repository: github.com/antonioiuliano2/antonioiuliano2.github.io/

But to build it initially I used this clear guide [https://www.npmjs.com/package/gh-pages](https://www.npmjs.com/package/gh-pages), the script I use is [https://github.com/antonioiuliano2/macros-ship/blob/master/geometry/publish\_ghpages.js](https://github.com/antonioiuliano2/macros-ship/blob/master/geometry/publish_ghpages.js)

Simply launch it with `node publish_ghpages.js`

