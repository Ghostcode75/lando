---
title: An example guide
metaTitle: An example guide | Lando
description: A longer and SEO dense description
date: 2020-01-20T18:57:47.435Z
original: stgg
repo: stgg

author:
  name: Mike Pirog
  pic: https://www.gravatar.com/avatar/dc1322b3ddd0ef682862d7f281c821bb
  link: https://twitter.com/pirogcommamike

feed:
  enable: true
  author:
    - name: Mike Pirog
      email: alliance@lando.dev
      link: https://twitter.com/pirogcommamike
  contributor:
    - name: Mike Pirog
      email: alliance@lando.dev
      link: https://twitter.com/pirogcommamike
---

# An example guide

<GuideHeader name="Mike Pirog" pic="https://www.gravatar.com/avatar/dc1322b3ddd0ef682862d7f281c821bb" link="https://twitter.com/pirogcommamike" />

Here is some demo content to help get you started. We are using it primarily to...

* Give you a decent starting structure for your guide
* Showcase some helpful markdown elements you can use
* Showcase the _kinds_ of things guides should have

Feel free to use none, some or all of this!

::: warning THIS IS DEMO CONTENT!
Note that the below is DEMO CONTENT and exists only to illustrate a possible way to structure a guide as well as the markdown components that can be used, including this warning dialog!
:::

## Proxying the SOLR 7 admin interface

By default Lando does not expose the solr admin interface to the outside world however you can tell Lando to expose it and even give it a nice `*.lndo.site` URL.

::: danger ONLY FOR RECENT VERSIONS OF SOLR
The solr web UI works differently for earlier versions of solr so this _may_ or _may not_ work for those earlier versions.
:::

### Exposing the UI

The key setting to activate is the `portforward` option which comes with our `solr` service. This will tell Lando to both expose the service to the outside world AND to assign a `localhost` address to it.

```yaml
name: lando-solr
services:
  index:
    type: solr:7
    portforward: true
```

Make sure you rebuild your app so the new config will take.

```bash
lando rebuild -y
```

You should see the `localhost` URL when the rebuild completes. :100: :guitar:


### Assigning a proxy route

```yaml
name: lando-solr
proxy:
  index:
    - admin-solr.lndo.site:8983
services:
  index:
    type: solr:7
    portforward: true
```

::: tip CUSTOM PROXY PORT
Note the `8983` at the end of `admin-solr.lndo.site`. This is needed when the web service is not running on the default `80` or `443` ports. **It does not mean that the service is accessible at `https://admin-solr.lndo.site:8983`.** It means that `https://admin-solr.lndo.site` routes to port `8983` on the `index` service.
:::

Again make sure you rebuild your app so the new config will take.

```bash
lando rebuild -y
```

You should see the `localhost` _and_ proxy URLs when the rebuild completes. :star:

<GuideFooter original="stgg" repo="sggg"/>
<Newsletter />
