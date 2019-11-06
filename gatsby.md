---
description: >-
  Gatsby is a free and open source framework based on React that helps
  developers build blazing fast websites and apps
---

# Gatsby

## Getting started

```
npm install -g gatsby-cli

gatsby new gatsby-site
cd gatsby-site
gatsby develop
```

## Creating a blog

```
gatsby new blog https://github.com/alxshelepenok/gatsby-starter-lumen
```

### Deploy to Github Pages

* Ensure that your `package.json` file correctly reflects where this repo lives
* Change the `pathPrefix` in your `config.js`
* Run the standard deploy command

```
npm run deploy
```

