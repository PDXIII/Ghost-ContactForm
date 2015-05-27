# Ghost-ContactFrom

This is a fork of the famous [Ghost](https://ghost.org) blogging software, which enables the use of Ghost internal Node Mailer module and mail settings to send contact forms without an third party. If you are looking for the original repositiory click [here.](http://github.com/TryGhost/Ghost/)

It is based on the tutorial [Ghost - Contact form using Ghost's Nodemailer](http://tarikk.com/ghost-blog-contact-form-using-ghosts-nodemailer/) by tarikk (sorry, don’t know his GitHub user name), but adds some AJAX functionality.

In addition you’ll need to install the [Casper-ContactForm theme](https://github.com/PDXIII/Casper-ContactForm) in your `/content/themes/` directory to see and use the contact form.

To get it work you will also configure your __config.js__ right, especially the mail options!

# Quick Start Install from Git

Make sure you've installed Node.js - We recommend the latest **Node v0.10.x** release.

Ghost is also compatible with **Node v0.12** and **io.js v1.2**, but please note that these versions are more likely to run into installation problems.

Install Node.js.

```bash
# Node v0.10.x - full support
# Node v0.12.x and io.js v1.2 - partial support
#
# Choose wisely
```

Clone this repository

```bash
git clone git@github.com:PDXIII/Ghost-ContactForm.git
cd Ghost-ContactForm
```

Install grunt. No prizes here.

```bash
npm install -g grunt-cli
```

Install Ghost. If you're running locally, use [master](https://github.com/TryGhost/Ghost/tree/master). For production, use [stable](https://github.com/TryGhost/Ghost/tree/stable). :no_entry_sign::rocket::microscope:

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


# Copyright & License

Copyright (c) 2015 PDXIII aka Peter Sekan - Released under the [MIT license](LICENSE).
