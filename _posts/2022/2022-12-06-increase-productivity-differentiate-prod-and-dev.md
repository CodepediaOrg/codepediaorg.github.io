---
layout: post
title: Visually differentiate environments for better programming efficiency
description: "Present a simple trick to differentiate environments for better efficiency when programming"
author: ama
permalink: /ama/visually-differentiate-environments-for-better-efficiency
published: true
categories: [article]
tags: [codever, productivity, environment]
---

## Going "green"

As mentioned [How I manage my dev bookmarks to save time and nerves](https://dev.to/ama/how-i-manage-my-dev-bookmarks-and-save-time-and-nerves-56ae)
I use heavily [Codever](https://www.codever.dev) to manage my bookmarks and code snippets, so there is always a browser
tab open with the website. Lately I've also been developing quite a bunch of [new features](https://github.com/CodeverDotDev/codever/tags) on the project.
Lots of testing happens in the browser, and until now there was no quick way to differentiate between the production and development version,
unless I look at the URL in the browser's navigation bar of course.

Because I am always looking for things to make me life easier, I've remembered of an old trick to help me much easier differentiate between the two versions:
- change the **color** of the **navigation bar**
- change the **favicon**

Both are now <span style="color:green"><b>green</b></span> for the dev environment 🤓🟢

See in the pictures below how easy it's to recognise the versions without looking the url

{% include image.html url="/images/posts/2022-12-06-increase-productivity-visually-differentiate-environments/recognise-dev-when-in-prod.jpeg" description="Recognise dev tab when in production" %}
{% include image.html url="/images/posts/2022-12-06-increase-productivity-visually-differentiate-environments/recognise-prod-when-in-dev.jpeg" description="Recognise prod tab when in development" %}


## Code changes

### Favicon
The code changes were minor. First change the `favicon` `href` attribute **when not production**

```typescript
export class AppComponent implements OnInit {
    favIcon: HTMLLinkElement = document.querySelector('#favicon');
    readonly environment = environment;

    ngOnInit(): void {
        if (environment.production === false) {
            this.favIcon.href = 'assets/logo/logo-green.svg';
        }
    }
}
```

> See [Configure and use environment specific values in Angular and html template](https://www.codever.dev/snippets/6389997bb160cb1fab2430fe/details) to see how to work with Angular environments


### Navigation bar color

Then set the navigation's bar color to <span style="color:green"><b>green</b></span> also based
on **not production** condition

```html
<nav class="navbar navbar-expand-lg navbar-dark bg-dark fixed-top shadow"
     [ngClass]="{'navbar-dev' : environment.production === false}" >
```

The `navbar-dev` css class

```css
.navbar-dev {
  background-color: darkgreen !important;
}
```

> See [Conditional css class in angular html element ](https://www.codever.dev/snippets/638daa94d00726522c6cdd7c/details)
> on how to change a class in Angular dynamically


## Multiple dev/testing environments

Of course if you have **more dev/test environments** like **integration** or **preprod**, consider differentiating them as well,
 your developers will thank you.

You won't believe how these little tweaks can improve your development experience 💪
