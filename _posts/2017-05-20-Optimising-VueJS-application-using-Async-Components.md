---
  layout: post
  title: Optimising VueJS application using Async Components
  tags: 
  categories: 
---


I have been trying my hand at [VueJS](https://www.haskell.org/) lately with some projects, and one in particular is an open source community website that has been getting a lot of traffic. Before I start, this project uses Laravel as the backend framework and [Firebase](https://firebase.com) for database and authentication.

My initial implementation involved loading all components in the app boostrap file which looked like the code snippet below

{% highlight javascript linenos %}

const App           = require('./components/App.vue');
const HomePage      = require('./components/site/HomePage.vue');
const AboutPage     = require('./components/site/AboutPage.vue');
const MembersPage   = require('./components/site/MembersPage.vue');
const TeamPage      = require('./components/site/TeamPage.vue');

{% endhighlight %}

After gulp did it things (bundling, minifiying, uglifying etc), I got an app bundle size of `1.1mb` uncompressed and `312kb` compressed, which if you are on a system with super fast internet is not that bad, but on mobile devices and slow internet is really painful.

I had already used Async modules in Angular 2 with route lazy loading, and was looking for a similiar technique in Vue. 

Turns out a similiar effect can be achieved using Vue's Async component and [Webpack's](https://webpack.js.org/) dynamic require.

The snippet below shows the updated app bootstrap using webpack's dynamic require.

{% highlight javascript linenos %}
const App           = require('./components/App.vue');
const HomePage      = (resolve, reject) => {
  require.ensure(['./components/site/HomePage.vue'], () => {
    resolve(require('./components/site/HomePage.vue'));
  });
};
const AboutPage     = (resolve, reject) => {
  require.ensure(['./components/site/AboutPage.vue'], () => {
    resolve(require('./components/site/AboutPage.vue'));
  });
};
const MembersPage   = (resolve, reject) => {
  require.ensure(['./components/site/MembersPage.vue'], () => {
    resolve(require('./components/site/MembersPage.vue'));
  });
};
const TeamPage      = (resolve, reject) => {
  require.ensure(['./components/site/TeamPage.vue'], () => {
    resolve(require('./components/site/TeamPage.vue'));
  });
};

{% endhighlight %}

We can see that we end up writing more code to require each component, but right now that is the price we have to pay for Async loading of components.

Incase you are wondering if we got any significant benefit switching to Async component loading, the answer is `YES` by a factor of more than 2 in terms of bundle size.

<!-- ![without async](/assets/img/async-loading-nor.png). ![with async](/assets/img/async-loading.png). -->

<div class="img-side">
<img width="150" height="150" src="/assets/img/async-loading-nor.png">
</div>
<div class="img-side">
<img width="150" height="150" src="/assets/img/async-loading.png">
</div>

From the image above we went from `489kb` compressed js bundle to  `160kb`.