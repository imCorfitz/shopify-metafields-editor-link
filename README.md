# Generate Shopify Metafields Editor Link

This repo generates a link needed to access custom metafields for products in your Shopify Store without the need for a plugin or app.

## Use locally

Simply clone the repo, install dependencies `yarn` or `npm install` and run the dev server `yarn dev` or `npm run dev`.

_Live Example: https://shopify-metafields-editor-link.vercel.app_

### How to use?

After you have added your custom metafields, you can simply include them in your theme code in liquid as following:

```shell
{{ product.metafields.global.field_name }}
```

This solution has a few limitations, as you are limited to only using global metafields, and you can't nest metafield objects.

This tool is public repo available at https://github.com/imCorfitz/shopify-metafields-editor-link

### Why?

Was watching "Code with Chris" and his video on [Custom Fields in Shopify](https://www.youtube.com/watch?v=UbwhADWZzvQ), and thought it would be good to have such tool available to simplify access to metafields in the Shopify Admin.

#### Have fun! 🤓 🥃
