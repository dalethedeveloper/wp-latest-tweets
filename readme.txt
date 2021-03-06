=== Plugin Name ===
Contributors: timwhitlock
Donate link: http://timwhitlock.info/donate-to-a-project/
Tags: twitter, tweets, oauth, api, rest, api, widget, sidebar
Requires at least: 3.5.1
Tested up to: 3.5.1
Stable tag: 1.0.6
License: GPLv2 or later
License URI: http://www.gnu.org/licenses/gpl-2.0.html

Latest Tweets widget compatible with the new Twitter API 1.1

== Description ==

Connect your Twitter account to this plugin and the widget will display your latest tweets on your site.

This plugin is compatible with the new **Twitter API 1.1** and provides full **OAuth** authentication via the Wordpress admin area.
 

Built by [timwhitlock](https://twitter.com/timwhitlock)

The underlying Twitter API library is [available on Github](https://github.com/timwhitlock/wp-twitter-api)

See also [Latest Vines](http://wordpress.org/extend/plugins/latest-vines-widget/)


== Installation ==

1. Unzip all files to the `/wp-content/plugins/` directory
2. Log into Wordpress admin and activate the 'Latest Tweets' plugin through the 'Plugins' menu

Once the plugin is installed and enabled you can bind it to a Twitter account as follows:

3. Register a Twitter application at https://dev.twitter.com/apps
4. Note the Consumer key and Consumer secret under OAuth settings
5. Log into Wordpress admin and go to Settings > Twitter API
6. Enter the consumer key and secret and click 'Save settings'
7. Click the 'Connect to Twitter' button and follow the prompts.

Once your site is authenticated you can configure the widget as follows:

8. Log into Wordpress admin and go to Appearance > Widgets
9. Drag 'Latest Tweets' from 'Available widgets' to where you want it. e.g. Main Sidebar
10. Optionally configure the widget title and number of tweets to display.

== Frequently Asked Questions ==

= How can I style the widget? =

See the 'Other Notes' tab for theming information.

= Why do I have to register my own Twitter app? =

Because I'm proving code, not a service. If I set up a Twitter app for this plugin I'd be responsible for every person who uses it. 
If Twitter closed my account or revoked my keys every instance of this plugin would break. Twitter also place limits on the number of users that can connect to a single app.

= How I do know what my OAuth settings are? =

These details are available in the [Twitter dashboard](https://dev.twitter.com/apps)

= What do I put in the third and fourth fields? =

Once you've populated the first two fields, just click the *Connect* button and follow the prompts.

= I get SSL certificate errors =

You can disable SSL verification of twitter.com by adding this to your theme functions.php:  
`add_filter('https_ssl_verify', '__return_false');`  
Do so at your own risk.


== Changelog ==

= 1.0.6 =
* Enabled translations and added pt_BR
* Switched dates to use i18n date formatter

= 1.0.5 =
* Moved widget title outside latest-tweets wrapper
* Using WordPress 'transient' cache when APC not available 

= 1.0.4 =
* Library update fixes dates for old PHP versions 

= 1.0.3 =
* Added theme filters
* Added configs for showing replies and RTs

= 1.0.2 =
* Fixed hook for PHP < 5.3

= 1.0.1 =
* First public release

== Theming ==

For starters you can alter some of the HTML using built-in WordPress features.  
See [Widget Filters](http://codex.wordpress.org/Plugin_API/Filter_Reference#Widgets)
and [Widgetizing Themes](http://codex.wordpress.org/Widgetizing_Themes)

**CSS**

This plugin contains no default CSS. That's deliberate, so you can style it how you want.

Tweets are rendered as a list which has various hooks you can use. Here's a rough template:

    .latest-tweets {
        /* style tweet list wrapper */
    }
    .latest-tweets h3 {
        /* style whatever you did with the header */
    }
    .latest-tweets ul { 
        /* style tweet list*/
    }
    .latest-tweets li {
       /* style tweet item */
    }
    .latest-tweets .tweet-text {
       /* style main tweet text */
    }
    .latest-tweets .tweet-text a {
       /* style links, hashtags and mentions */
    }
    .latest-tweets .tweet-details {
      /* style datetime and link under tweet */
    }


**Custom HTML**

If you want to override the default markup of the tweets, the following filters are also available:

* Add a header between the widget title and the tweets with `latest_tweets_render_before`
* Perform your own rendering of the timestamp with `latest_tweets_render_date`
* Render plain tweet text to your own HTML with `latest_tweets_render_text`
* Render each composite tweet with `latest_tweets_render_tweet`
* Override the unordered list for tweets with `latest_tweets_render_list` 
* Add a footer before the end of the widget with `latest_tweets_render_after`

Here's an **example** of using some of the above in your theme's functions.php file:

    add_filter('latest_tweets_render_date', function( $created_at ){
        $date = DateTime::createFromFormat('D M d H:i:s O Y', $created_at );
        return $date->format('d M h:ia');
    }, 10 , 1 );
    
    add_filter('latest_tweets_render_text', function( $text ){
        return $text; // <- will use default
    }, 10 , 1 );
    
    add_filter('latest_tweets_render_tweet', function( $html, $date, $link ){
        return '<p class="my-tweet">'.$html.'</p><p class="my-date"><a href="'.$link.'">'.$date.'</a></p>';
    }, 10, 3 );
    
    add_filter('latest_tweets_render_after', function(){
        return '<footer><a href="https://twitter.com/me">More from me</a></footer>';
    }, 10, 0 );


== Credits ==

Screenshot taken with permission from http://stayingalivefoundation.org/blog

== Notes ==

Be aware of [Twitter's display requirements](https://dev.twitter.com/terms/display-requirements) when rendering tweets on your website.

