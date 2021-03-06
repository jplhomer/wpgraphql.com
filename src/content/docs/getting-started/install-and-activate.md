---
title: Install and Activate
description: Details on installing and activating the WPGraphQL WordPress plugin
---

> ### Beta Software Notice
> Until WPGraphQL hits a 1.0.0 release, it is still considered beta software. This doesn't mean that the plugin isn't 
> ready for use, it just means that there might still be bugs and that there might be breaking changes to the shape of 
> the API or internal functions as we work toward a stable release.
> 
> Don't hesitate to start using the plugin, but just be sure to follow along with releases and keep up to date with 
> conversations in Slack [join here](https://wpgql-slack.herokuapp.com/)
> 
> WPGraphQL is already in use in production on several sites, including [work.qz.com](http://work.qz.com), 
> [hopelabs.org](http://hopelab.org) and more.

### Recommended Version
For the most stable and performant experience, it’s recommended that you use the most recent version of the plugin. 
You can see the [latest releases here](https://github.com/wp-graphql/wp-graphql/releases).

Of course, as new features are in development, feel free to check out the latest develop branch or check out any other feature or release.

## Download / Clone Plugin

WPGraphQL is available on Github: [https://github.com/wp-graphql/wp-graphql](https://github.com/wp-graphql/wp-graphql)

You can download the plugin or clone the plugin from Github.

Add the downloaded/cloned plugin to your WordPress plugin directory. On a typical WordPress install, this is located at /wp-content/plugins

<Tip>To minimize risk of unintended behavior, it's best for your plugin directory to be "wp-graphql" and not something else, like "wp-graphql-master" or "wp-graphql-develop"</Tip>

## Activate the Plugin

Once the plugin is in the WordPress plugins directory, it can be activated by clicking “Activate” on the plugin screen, or via WP CLI wp plugin activate wp-graphql

### Verify the Endpoint Works
The most common use of WPGraphQL is as an API endpoint that can be accessed via HTTP requests (although it can be used without remote HTTP requests as well)

In order for the /graphql endpoint to work, you must have [pretty permalinks](#flush-permalinks) enabled and any permalink structure other than the default WordPress permalink structure.

Once the plugin is active, your site should have a `yoursite.com/graphql` endpoint and you the **expected response** is a JSON payload like so:

```json
{
    "errors": [
        {
            "message": "GraphQL requests must be a POST or GET Request with a valid query",
            "category": "user"
        }
    ]
}
```

<Warning>If things aren't working at this point, you may need to [flush permalinks](https://codex.wordpress.org/Function_Reference/flush_rewrite_rules)</Warning>

### Flush Permalinks

Activating the plugin should cause the permalinks to flush. Occasionally, this doesn’t work. If you have a permalink structure other than the default WordPress structure, and the plugin is active but nothing shows at your site’s /graphql endpoint, try to manually flush your permalinks by:

- From your WordPress dashboard, visit the **Settings > Permalinks** page. Just visiting the page should flush the permalinks

or

- Using WP CLI run wp rewrite flush

## Using GraphQL without remote HTTP requests to the /graphql Endpoint

WPGraphQL can be used from the context of WordPress PHP and doesn’t require HTTP requests to be used. You can completely remove the `/graphql` endpoint that the plugin provides so that the API is not available publicly in any way but still use GraphQL in your plugin and theme code by using:

```php
do_graphql_request( $request, $operation_name, $variables );

```

[Learn more about using WPGraphQL in PHP, without HTTP requests](http://wpgraphql.com/guides/graphql-in-php)
