# **Shopware with DDEV**

## Live Training

<br>
<div class="logo-row">
<img src="images/ddev-logo.svg" alt="DDEV Logo" class="ddev-logo">
<img src="images/shopware-logo-brand-blue.svg" alt="Shopware Logo">
</div>
<br>

Benny Poensgen (@vanWittlaer)

Note:
Speaker notes go here.

---

## Agenda

1. What (and who) is Shopware?
2. Getting started & Tooling
3. A simple setup
4. Shopware Configuration
5. Media Proxy
6. A more complex setup

Note:
Speaker notes go here.

---

## What (and who) is Shopware?

* A headless e-commerce platform
* Open source (MIT) (well, mostly 😉)
* <a href="https://github.com/shopware/shopware" target="_blank">https://github.com/shopware/shopware</a>
* 3.4k stars, 1.2k forks, 600 contributors

--

## Who is Shopware AG?
<!-- .slide: class="dense" -->

<img src="images/Schoeppingen.png" width="200" alt="Schöppingen" data-preview-image data-preview-fit="contain" style="float: right; margin: 0 0 0.5em 1em; cursor: zoom-in;">

* Founded 2000 by brothers Sebastian and Stefan Hamann
* Based in Schöppingen (a rather small community in the north west of Germany)
* Runs a subsidiary Shopware US Inc., NYC
* Privately owned with PayPal holding some 40 %<sup>*</sup>
* Named a 'Visionary' in the Gartner Magic Quadrant for Digital Commerce (2023, 2025)
* Revenue (2025) some 75 million € (62 % Germany, 15 % EU, 23 % other)<sup>**</sup>
* Some 330 employees (2025)

<small>(*) Source: [Michele Mattei on LinkedIn](https://www.linkedin.com/posts/michele-mattei_paypal-shopware-share-7386365016003739648-xojn/) 
<br>
(**) Source: Annual Report 2025</small>

--

## Why I recommend it

* Modern architecture and technology stack
* Extremely scalable (100 k€ to 10s of million € GMV)
* Very easily customizable (Twig template inheritance, event system)
* Customizations (if done properly) do not break updates
* Compliance with legal requirements through regular updates (EU Garan label)

--

## Shopware Community

<img src="images/shopware-discord.png" width="640">

<center><a href="https://discord.com/invite/shopware" target="_blank">https://discord.com/invite/shopware</a></center>

<center>
Find me there: <a href="https://discord.com/users/bennypoensgen" target="_blank">@bennypoensgen</a>
</center>

--

## Architecture Basics
<!-- .slide: class="dense" -->

* Note the non-standard versioning: 6.7.15.1 - "Shopware 6" with 7.15.1 designating the major, minor and patch version
* Based on Symfony 7.4, makes use of message queues, mailer, caching, flysystem etc.
* Homegrown data access layer ('DAL' replaces Doctrine ORM)
* A Shopware 6 setup can be rather complex (with 2 or more Redis instances, RabbitMQ, OpenSearch, S3)
* Simplest setup: a web server and MariaDB, all other services provided by Doctrine and local filesystem

--

## Community Edition, Paid Plans

* Community Edition available via GitHub
* Paid plans on three levels (Rise, Evolve, Beyond) adding features (like B2B) and support
* Register as a 'technical partner' to gain access to paid plan features for plugin and app development

--

## Apps and Plugins

* Plugin/app (a.k.a. extensions) store <a href="https://store.shopware.com" target="_blank">https://store.shopware.com</a>
* 'Plugins' run in the Shopware web application (self-hosted or PaaS)
* 'Apps' run application logic on their own servers, connect via webhooks (targeting SaaS instances)
* Simplest apps overwrite Twig templates and SCSS only, do not even require a server

---

## Getting started & Tooling

* Use <code>shopware6</code> as DDEV project type
* Start with either DDEV quickstart guide or use my less-than-5-minutes install (see below)
<br>
<small>
<a href="https://docs.ddev.com/en/stable/users/quickstart/#shopware" target="_blank">https://docs.ddev.com/en/stable/users/quickstart/#shopware</a>
<br><br>
<a href="https://notebook.vanwittlaer.de/ddev-for-shopware/less-than-5-minutes-install-with-ddev-and-symfony-flex" target="_blank">https://notebook.vanwittlaer.de/ddev-for-shopware/less-than-5-minutes-install-with-ddev-and-symfony-flex</a>
</small>

--

## IDE integration

* PHPStorm: Shopware 6 Toolbox
* VSCode: Shopware Language Server
* Symfony Services, Twig Templates, Twig Block Versioning, Icons, Snippets (Translations), Routes, Feature Flags, and more

--

## shopware-cli

* <a href="https://developer.shopware.com/docs/products/tools/cli/" target="_blank">shopware-cli</a> is Shopware's Swiss Army knife
  * supports the development of plugins and apps (and more)
  * comes bundled with DDEV
* Use with <code>ddev shopware-cli ...</code> or in the web container - do not use it natively on your host
* Admin and storefront watchers: <code>ddev admin watch</code> and <code>ddev storefront-watch</code>

---

## A simple setup

<center>
<br>
<a href="https://github.com/vanWittlaer/sissy-demo" target="_blank">https://github.com/vanWittlaer/sissy-demo</a>
<br><br>
A simple setup with just the database and mailpit, demonstrating the use of DDEV and a simple Shopware setup based on shopware/docker
</center>

---

## Shopware Configuration

* A combination of .env variables and files
* .env files per environment (.env.dev etc., note .env.local - local is not an environment)
* Symfony-style configuration files in `config/packages/<env>`

---

## Media Proxy
<!-- .slide: class="media-proxy" -->

* A shop can easily have tens of GB of media files
* Don't waste your laptop's resources - use a media proxy

```nginx
# .ddev/nginx/media-proxy.conf

set $media_proxy_url "https://media.example.com/public";

location @mediaserver {
    resolver 1.1.1.1 ipv6=off;
    proxy_ssl_server_name on;
    proxy_pass $media_proxy_url$request_uri;
}

location ^~ /media/ {
    access_log off;
    expires max;
    try_files $uri $uri/ @mediaserver;
    break;
}

location ^~ /thumbnail/ {
    access_log off;
    expires max;
    try_files $uri $uri/ @mediaserver;
    break;
}
```

Similar setups for Apache or other webservers are possible.

---

## A more complex setup

<center>
<br>
<a href="https://github.com/vanWittlaer/swoofy" target="_blank">https://github.com/vanWittlaer/swoofy</a>
<br><br>
A more complex setup with Redis, RabbitMQ and Elasticsearch, demonstrating the use of DDEV and a Coolify controlled server setup
</center>

---

# Thank You!
<center>
Questions?
<br><br>
Benny Poensgen (@vanWittlaer)<br>
<a href="https://www.poensgen.de" target="_blank">https://www.poensgen.de</a><br>
Email: benjamin@poensgen.de
<br><br>
or get in touch via the Shopware Discord server: @bennypoensgen
</center>
