# Ghost-FormMailer
This is a fork of the famous [Ghost](https://ghost.org) blogging software, which enables the use of Ghost internal NodeMailer module and mail settings to send contact forms without an third party. If you are looking for the original repositiory click [here.](http://github.com/TryGhost/Ghost/)

It is based on the tutorial [Ghost - Contact form using Ghost's Nodemailer](http://tarikk.com/ghost-blog-contact-form-using-ghosts-nodemailer/) by [tariknz](https://github.com/tariknz), but added some AJAX functionality.

To get it work you will have to configure your **config.js** right, especially the mail object!

## What’s the Difference?
Out of the box Ghost doesn't provied a contact form. In the documentation you'll find ways to embed third party services for contact forms, but in my opinion, this doesn't make any sense, especially when you already have Node Mailer built in.

To achive this several changes in the core and in the theme were necessary. Some depend on each other, so please **read the following section carefully.**

## Code Changes
This section covers the changes in the core section of Ghost. For the necessary changes in the theme please head over to [Casper-FormMailer repository.](https://github.com/PDXIII/Casper-FormMailer)

The changes are quiet simple and very straight forward, but before that you'll need to pay attention to your **config.js** and the right declaration of the **mail object.**

### Config.js Especially the Mail Object
Like in any other Ghost installation you'll need to configure the **mail object.** It should look similar to this:

```
mail: {
    transport: 'SMTP',
    from: 'Ghost Blog Admin <admin@your-ghost-blog-domain.tld>',
    options: {
        host: 'your-ghost-blog-domain.tld', // hostname
        secure: true,                       // use SSL
        port: 587,                          // port for secure SMTP
        auth: {
            user: 'admin@your-ghost-blog-domain.tld',   // username
            pass: 'Y0ur-T0t477y-5ecure-Pa55w0rd'        // password
       }
    }
},
```

This is not only improtant for the contact form, this is responsible for delivering a new login for your Ghost Blog, if you forgot your password. Like mentioned before, this is basic stuff! Now let's go on to the real code changes.

### The Frontend Controller
The first document changed is the frontend controller index.js, which is located under `core/server/controllers/frontend/index.js`. First of all Node Mailer needed to be referenced in the variables:

```
var _           = require('lodash'),
    api         = require('../api'),
    rss         = require('../data/xml/rss'),
    path        = require('path'),
    config      = require('../config'),
    errors      = require('../errors'),
    filters     = require('../filters'),
    Promise     = require('bluebird'),
    template    = require('../helpers/template'),
    routeMatch  = require('path-match')(),

    // costum code for adding a contact form with node mailer part 1
    // this requires node mailer
    mailer      = require('../../mail'), // add node mailer
    // end of custom code part 1

    frontendControllers,
    staticPostPermalink = routeMatch('/:slug/:edit?');
```

Easy, init mate? Let's go on! This document needed another change: a new method for summiting the form! This should be the last function in the **frontendControllers object**:

```
frontendControllers = {
    // a lot of methods are written here
    // and finally at the bottom comes our custom method

    // custom code for adding a contact form with nodemailer part 2
    // action for posting the contact form
    submitContactForm: function (req, res) {

        // debug output
        // console.log(req.query);

        var mailOptions = {
            // the req.query must be in synch with
            // the contact form

            // sender address
            from: req.query.email,

            // email receiver (add your email here)
            // you can use the variable
            // config.mail.options.auth.user
            // which keeps the email address you definded
            // in the config.js
            // or you add any other email address
            to: config.mail.options.auth.user,

            // subject
            subject: req.query.subject,

            // email message body
            html: req.query.messageBody + ' - From: ' + req.query.name
        };

        //sending the email
        mailer.send(mailOptions).then(function (data) {
            //this is the response to the end user
            //should ideally redirect or return a view
            res.status(200);
            res.send('OK');

        }).error(function (error){

            //response for error
            res.status(500);
            res.send('ERROR');

        });
    }
    // end of custom code part 2
};
```

**Attention:** This works only with the contact form of the [Casper-FormMailer](https://github.com/PDXIII/Casper-FormMailer) theme. If you want your contact form to be different, you need to make according changes in this section!

That's for the frontend controller index.js. Let's go on!

### The Frontend Route
There must be a route where the form can be posted. this needs to be declared in this file `core/server/routes/frontend.js` in the **frontendRoutes object**:

```
frontendRoutes = function (middleware) {
    var router = express.Router(),
        subdir = config.paths.subdir,
        routeKeywords = config.routeKeywords;

    // a lot of routes are written here
    // and finally at the bottom you find the new route

    // custom code for adding a contact form with nodemailer
    // if you want to pass arguments to the function
    // it must be router.get instead of tariknz’ router.post
    router.get('/mail', frontend.submitContactForm);
    // end of custom code

    return router;
};
```

So, what's the idea behind this? In n00b speak it is like that: When you hit the submit button your contact form is posted to the route `/mail`. The server listens to this route, and when something is posted to it, it takes the values and executes the method `submitContactForm` which is declared in the **frontend controller**.

### The Theme
To get the whole thing running, the basic theme neede some changes, too, which are covered in the [Casper-FormMailer repository](https://github.com/PDXIII/Casper-FormMailer). So please head over!

## Quick Start Install from Git
Make sure you've installed Node.js - We recommend the latest **Node v0.10.x** release.

Ghost is also compatible with **Node v0.12** and **io.js v1.2**, but please note that these versions are more likely to run into installation problems.

Install Node.js.

```bash
# Node v0.10.x - recommended
# Node v0.12.x and v4.2.x - supported
#
# Choose wisely
```

Clone this repository

```bash
git clone git@github.com:PDXIII/Ghost-FormMailer.git
cd Ghost-FormMailer
```

Install grunt. No prizes here.

```bash
npm install -g grunt-cli
```

Install Ghost.

```bash
npm install
```

Build the things!

```bash
grunt init
```

Minify that shit for production?

```bash
grunt prod
```

Start your engines.

```bash
npm start

## running production? Add --production
```

## Copyright & License
Copyright (c) 2016 PDXIII aka Peter Sekan - Released under the [MIT license](LICENSE).
