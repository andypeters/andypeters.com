---
layout: post
title: Setup Bridgetown to use TailwindCSS
published: true
date:   2020-10-10 10:00:00 -0500
date_short: Oct 10
date_long: Oct 10, 2020
description: Use TailwindCSS with Bridgetown to create lovely static sites.
excerpt: Hey there!  I used Bridgetown and TailwindCSS to build this site you are looking at.  Here's a little bit about how I setup TailwindCSS with Bridgetown to create this little ole website.  Hope it helps you out...
---

I've been using the Ruby powered static site generator:  [Bridgetown](https://bridgetownrb.com) for a lot of things lately.  If you don't already know, here is the Bridgetown tagline:

> A Webpack-aware, Ruby-powered static site generator
for the modern Jamstack era.

I'm a Ruby on Rails fan and, [TailwindCSS](https://tailwindcss.com) believer and I'm all in on [TailwindUI](https://tailwindui.com).  So naturally, I wanted to use this stack to build my site.  [Andrew Mason](https://dev.to/andrewmcodes) had a great writeup to setup Bridgetown with TailwindCSS, but for the life of me I couldn't get TailwindCSS to build with those instructions.  After getting things to work on my end, I think the difference was in the PostCSS and/or the `index.scss` file but I'm still not 100%.  🤷🏻‍♂️ Here is his detailed write up:


Since everything is working just fine today, I thought I'd write up  my steps.  This is assuming you have a Bridgetown site created and ready to go:

*Note this is with [Bridgetown 0.17.1] (https://github.com/bridgetownrb/bridgetown/releases/tag/v0.17.1)*

### 1. yarn add ...:

`yarn add -D postcss autoprefixer postcss-import postcss-loader tailwindcss`

It's my understanding that `postcss` is a dependency of `postcss-import` and `postcss-loader`. So I didn't need to call it out here, but I did anyway for verbosity when I was trying to get things working.

### 2. Create `tailwind.config.js`

Easiest way to do this:  `npx tailwindcss init`

```javascript
module.exports = {
  future: {
    // removeDeprecatedGapUtilities: true,
    // purgeLayersByDefault: true,
  },
  purge: [],
  theme: {
    extend: {},
  },
  variants: {},
  plugins: [],
}
```

### 3. Create `postcss.config.js`

Here is a copy of my `postcss.config.js` file.  It was actually taken from an old Jekyll site doing the same thing.  Seems to work.  🤷🏻‍♂️

```javascript
const purgecss = require("@fullhuman/postcss-purgecss")({
  content: ["./src/**/*.html", "./src/**/*.md", "./src/**/*.liquid", "./frontend/**/*.js", "./src/_data/**/*.yml"],

  defaultExtractor: content => content.match(/[\w-/.:]+(?<!:)/g) || []
})

module.exports = {
  plugins: [
    require("postcss-import", {
      path: "frontend/styles",
      plugins: []
    }),
    require("tailwindcss"),
    require("autoprefixer"),
    ...(process.env.NODE_ENV == "production" ? [purgecss] : [])
  ]
}
```

### 4. Update `webpack.config.js` to use postcss.

There is only one change I did and that was to add `"postcss-loader"` on line 56.  Here is a full copy of the file for reference:

```javascript
const path = require("path");
const MiniCssExtractPlugin = require("mini-css-extract-plugin");
const ManifestPlugin = require("webpack-manifest-plugin");

module.exports = {
  entry: "./frontend/javascript/index.js",
  devtool: "source-map",
  // Set some or all of these to true if you want more verbose logging:
  stats: {
    modules: false,
    builtAt: false,
    timings: false,
    children: false,
  },
  output: {
    path: path.resolve(__dirname, "output", "_bridgetown", "static", "js"),
    filename: "all.[contenthash].js",
  },
  resolve: {
    extensions: [".js", ".jsx"],
  },
  plugins: [
    new MiniCssExtractPlugin({
      filename: "../css/all.[contenthash].css",
    }),
    new ManifestPlugin({
      fileName: path.resolve(__dirname, ".bridgetown-webpack", "manifest.json"),
    }),
  ],
  module: {
    rules: [
      {
        test: /\.(js|jsx)/,
        use: {
          loader: "babel-loader",
          options: {
            presets: ["@babel/preset-env"],
            plugins: [
              ["@babel/plugin-proposal-decorators", { "legacy": true }],
              ["@babel/plugin-proposal-class-properties", { "loose" : true }],
              [
                "@babel/plugin-transform-runtime",
                {
                  helpers: false,
                },
              ],
            ],
          },
        },
      },
      {
        test: /\.(s[ac]|c)ss$/,
        use: [
          MiniCssExtractPlugin.loader,
          "css-loader",
          "postcss-loader",
          {
            loader: "sass-loader",
            options: {
              sassOptions: {
                includePaths: [
                  path.resolve(__dirname, "src/_components")
                ],
              },
            },
          },
        ],
      },
      {
        test: /\.woff2?$|\.ttf$|\.eot$|\.svg$/,
        loader: "file-loader",
        options: {
          outputPath: "../fonts",
          publicPath: "../fonts",
        },
      },
    ],
  },
};

```

### 5. Update the main stylesheet

`frontend/styles/index.scss`

```css
@import "tailwindcss/base";
@import "tailwindcss/components";
@import "tailwindcss/utilities";
```

**UPDATE:**. I also had used the `@tailwind` in the config and it worked fine.  I switched to the `@import` as I think that is the correct way when working with `postcss-import` (as detailed on the Tailwind site).

More [details here](https://tailwindcss.com/docs/installation#add-tailwind-to-your-css)

### 6. Start the server

`yarn start`.

🚀

### 6.5:  If you are using TailwindUI, then simply follow the instructions on the site documentation.  You'll just be adding the `tailwindui` plugin as the instructions indicate.

That's it.  Enjoy!

*Oh, just incase here is Bridgetown on Github:*
