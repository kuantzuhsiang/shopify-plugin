#Shopify Integration

##Shopify Theme Modification

To actually start serving your content with Dexecure, you’ll need to make two changes to your Shopify theme.

You can access Shopify’s theme file editor by going to

`Online store > Themes > Customize theme > Theme options > Edit HTML/CSS`

###settings_schema.json

The first change lets you configure your Dexecure installation from inside Shopify’s theme settings. Copy the code below to the bottom of your theme’s `Config/settings_schema.json` file, inside the outermost pair of square brackets (`[` `]`). Make sure to add a comma (`,`) to the file following the `}` preceeding where you paste in this code.

```
{
  "name": "dexecure",
  "settings": [
    {
      "type": "checkbox",
      "id": "enableDexecure",
      "label": "Enable Dexecure"
    },
    {
      "type": "text",
      "id": "dexecureUrl",
      "label": "Dexecure subdomain",
      "info": "The subdomain you are given on the Dexecure platform. Example: `https:\/\/xxx.cloudfront.net`."
    },
    {
      "type": "text",
      "id": "dexecureShopifyUrl",
      "label": "Shopify CDN domain",
      "default": "\/\/cdn.shopify.com",
      "info": "Don't change this unless you have a proxy in place. Not sure? Leave it as is."
    }
  ]
}
```

###dexecure.liquid

The second file adds a new Liquid tag that helps you generate Dexecure URLs. This time, create a new file in the Snippets directory of your theme named `dexecure.liquid`. We have committed the file into this repository, you can find it's contents [here](https://raw.githubusercontent.com/Dexter-JS/shopify-plugin/master/dexecure.liquid?token=AAvS63ervggjpTvYrz2yIi295AyBhNLPks5Xvt7HwA%3D%3D#). Copy that code into the `dexecure.liquid` file that you just created and save.

##Enabling the Dexecure Integration

Now that you’ve set up your Shopify theme to work with Dexecure, you can enable Dexecure in your theme settings. Head to the `Online store > Themes > Customize`. Once there, you’ll see a `Theme settings` option in the left-hand sidebar. Click on this. Head to the `dexecure` tab to configure your Dexecure setup. It should look something like the following:

```
Enable Dexecure: Make sure this is checked.
Dexecure subdomain: You would get this from Dexecure. It would be something like https://xxx.cloudfront.net
Shopify CDN domain: You’ll most likely not want to change this from the default value of //cdn.shopify.com.
```

After you’ve set everything up, click `Save`, and you can move on!

##Migrating Your Theme Images to Dexecure

At this point, you’re ready to change your existing theme’s assets to use Dexecure. The exact process will vary depending on your theme, but the basic idea won’t change. Just find places in your theme where existing assets are being output, and replace them with the Dexecure Liquid tag you just created. Here’s an example:

####Before:
```
{{ product.featured_image | img_url:'grande' }}
```
####After:
```
{% assign feat_img_url = product.featured_image | img_url:'master' %}
{% include 'dexecure' src:feat_img_url %}
```
####Before:
```
{{ 'collection5-3.png' | asset_url | img_tag }}
```
####After:
```
{% assign full_url_for_dexecure = 'collection10-1.png' | asset_url %}
{% capture modified_dexecure_url %}
{% include 'dexecure' src:full_url_for_dexecure %}
{% endcapture %}
{{modified_dexecure_url | strip | img_tag}}
```
####Note:
It’s a good idea to always use the master variant of Shopify’s images, and let Dexecure handle the resizing. That way, you’ll always get the best quality image possible.
