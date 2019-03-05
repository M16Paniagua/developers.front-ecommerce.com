---
id: front-commerce-js
title: .front-commerce.js
---

The `.front-commerce.js` configuration file at the root of a Front-Commerce project is the main entrypoint defining how an application behaves.

From this definition, Front-Commerce will initialize and register the different pieces composing your application.

## `name`

The application name.

```js
module.exports = {
  name: "ACME shop",
  // […]
};
```

This name is used for _Developer Experience_ only and is not aimed at appearing in the theme or any website user message.

It is for instance used as title in the [Storybook design system](/docs/essentials/add-component-to-storybook.html).

## `url`

The project URL.

```js
module.exports = {
  // […]
  url: "https://github.com/acme/shop/",
  // […]
};
```

This name is used for _Developer Experience_ only and is not aimed at appearing in the theme or any website user message. You may thus use it to redirect to your staging instance, or project management tool for instance.

It is for instance used as link target for the name in the [Storybook design system](/docs/essentials/add-component-to-storybook.html).

<blockquote class="note">
  Use [the `FRONT_COMMERCE_URL` environment variable](/docs/reference/environment-variables.html#Host) to configure the URL used to access to your application.
</blockquote>

## `modules`

A list of path to Front-Commerce modules to register in your application.

```js
module.exports = {
  // […]
  modules: ["./acme-common", "./src", "./winter", "./christmas"]
  // […]
};
```

Each module can contain client code to [extend the theme](/docs/essentials/extend-the-theme.html), and server code to [add custom server endpoints](/docs/advanced/server/add-http-endpoint.html).

<blockquote class="note">
Front-Commerce modules are registered in the order they appear in this list.
**The default Front-Commerce theme will always be registered first and must not appear in this list.**
</blockquote>

In the above example, a `component/Foo` component would be resolved according to theme overrides in the following order (the first existing one would be used):

1. `./christmas/component/Foo`
1. `./winter/component/Foo`
1. `./src/component/Foo`
1. `./acme-common/component/Foo`
1. `front-commerce/src/web/theme/component/Foo`


## `serverModules`

A list of objects describing GraphQL modules to register in your application.
GraphQL modules allows you to [extend the GraphQL schema](/docs/essentials/extend-the-graphql-schema.html).

```js
module.exports = {
  // […]
  serverModules: [
    { name: "FrontCommerceCore", path: "server/modules/front-commerce-core" },
    { name: "Magento2", path: "server/modules/magento2" }
  ],
  // […]
};
```

Each object must be composed of the properties below:

* `name`: unique key that must be unique across the `serverModules` list.
  It is a temporary name that is used during a code generation step and has no other usage in the application.
  _Do not worry about it, it will be deprecated in a near future: see [#179](https://gitlab.com/front-commerce/front-commerce/issues/179) for further information._

* `path`: must be a path to the [GraphQL module definition file](/docs/reference/graphql-module-definition.html)

<blockquote class="info">
  GraphQL modules will be loaded according to the [`dependencies` declared in their respective definitions](/docs/reference/graphql-module-definition.html#dependencies-optional).
  The order in the `serverModules` list should **NOT** be relied on for dependency management.
</blockquote>

As of the latest release, Front-Commerce’s provides the following GraphQL modules (or [meta-modules](/docs/advanced/graphql/meta-modules.html)):

* `server/modules/front-commerce-core`: core features of Front-Commerce.
* `server/modules/magento2` _(meta module)_: Magento2 GraphQL modules to [use Front-Commerce with all Magento2 supported features](/docs/magento2/overview.html).
  Register single sub-modules explicitely if you only need a subset of features.
* [Front-Commerce’s embedded payment](/docs/advanced/checkout/overview.html) modules:
  * `server/modules/payment-ogone`: Ogone payment platform.
  * `server/modules/payment-paypal`: PayPal payment platform.
  * `server/modules/payment-payzen`: PayZen payment platform.

<!-- TODO Add links to each embedded payment documentation page when available -->

## `styleguidePaths`

_Default value: `[ /.*.story.js$/ ]`_

A list of regexes that match the Stoybook stories you want to use in your styleguide, so you can [only display relevant stories in your Design System](/docs/essentials/add-component-to-storybook.html#Display-only-the-relevant-stories-to-your-Design-System).


```js
module.exports = {
  // […]
  styleguidePaths: [
    /.?\/components\/atoms\/Button\/.*.story.js$/,
    /.?\/components\/atoms\/Typography\/.*.story.js$/,
    /.?\/components\/molecules\/.*.story.js$/,
    /.?\/(pages|modules)\/.*.story.js$/,
  ],
  // […]
};
```

<blockquote class="info">
Stories matching this pattern will be fetched across the `web/theme` folder of every [Front-Commerce `modules` defined](#modules) in the `.front-commerce.js` file.
</blockquote>