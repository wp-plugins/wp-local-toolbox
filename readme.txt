=== WP Local Toolbox ===
Contributors: joeguilmette,jb510
Tags: admin,administration,responsive,dashboard,notification,simple, develop, developer, developing, development
Tested up to: 4.2
Stable tag: 1.2
License: GPL v2+
License URI: https://www.gnu.org/licenses/gpl-2.0.html

A simple plugin to help manage development over local, staging and production servers.

== Description ==
Through constants defined in wp-config, you can disable plugins, disable the  loading of external files, set search engine visibility, display or hide the admin bar, display the server name and change the color of the admin bar, or literally anything else you can think of. All without touching the database, so you can push and pull without worrying.

This is an invaluable tool if you often work in production, staging, and local servers at the same time.

For support, pull requests, and discussion: https://github.com/joeguilmette/wp-local-toolbox

= Constants =

* **WPLT_SERVER**: The name of your server environment. It will be displayed in the admin bar at browser widths greater than 1030px. If left undefined, the plugin will make no changes to the admin bar. 

	If not defined as `PRODUCTION` or `LIVE`, the plugin will enable 'Discourage search engines from indexing this site' to prevent your development and staging servers from being indexed. This option is not stored in the database, so your production server will still look to the actual setting on the Reading page.

* **WPLT_COLOR**: Determines the color of the admin bar. You can set this to any CSS color. If left undefined, will use the following defaults: 
	
	* PRODUCTION / LIVE: red
	* STAGING / TESTING: orange
	* LOCAL / DEV: green

* **WPLT_ADMINBAR**: Show or hide the admin bar on the frontend. `FALSE` will force it to be hidden, `TRUE` will force it to display, `ALWAYS` will display the admin bar even when logged out. These settings will override the 'Show toolbar' setting in the 'Users > Your Profile' options panel and `add_filter('show_admin_bar', '__return_false');` in functions.php, but doesn't attempt to overcome any CSS based hiding of the admin bar.

* **WPLT_DISABLED_PLUGINS**: An array of plugins to disable. This does not store any data in the database, so plugins that are manually deactivated or activated will stay so when undefined in this constant. **Note: In order for this to function properly, WP Local Toolbox must be installed as an mu-plugin. You can read more about mu-plugins here: https://codex.wordpress.org/Must_Use_Plugins**. We're investigating ways to avoid this requirement; if you have any ideas we'd love to hear it!

* **WPLT_NOTIFY**: Define this constant as the email address where you'd like to be notified of post updates. You can specify either an email address or a Slack Incoming WebHook URL. You can set up a Slack Incoming WebHook URL here: https://my.slack.com/services/new/incoming-webhook/

This is helpful in production to see if a client has submitted a new post, or in development to see if data is being added to the staging environment so you don't accidentally overwrite new posts when pushing databases around.

* **WPLT_NOTIFY_CHANNEL**: If WPLT_NOTIFY is set to a Slack Incoming WebHook URL, you can specify the channel that the notification will be posted to. If left unset, it will post to the default channel specified in Slack's Incoming WebHooks settings page.

* **WPLT_DISABLE_ATTACHMENT_NOTIFY**: If set, this will disable notifications for attachments.

* **WPLT_AIRPLANE**: Control loading of external files when developing locally. WP loads certain external files (fonts, gravatar, etc) and makes external HTTP calls. This isn't usually an issue, unless you're working in an evironment without a web connection. This plugin removes / unhooks those actions to reduce load time and avoid errors due to missing files.

	On and Off: Can be toggled from the admin bar by clicking 'Airplane Mode'. In the admin bar a ✗ or ✓ will indicate if Airplane Mode is enabled or disabled. 

= Example wp-config.php Entries =

`
// set server environment to 'LOCAL'
define('WPLT_SERVER', 'local');

// set admin bar color to #800080
define('WPLT_COLOR', 'purple');

// show the admin bar even when logged out
define('WPLT_ADMINBAR', 'always');

// deactivate a set of plugins
define('WPLT_DISABLED_PLUGINS', serialize(
	array(
		'w3-total-cache/w3-total-cache.php',
		'updraftplus/updraftplus.php',
		'nginx-helper/nginx-helper.php',
		'wpremote/plugin.php',
		'wordpress-https/wordpress-https.php',
	)
));

// enable the Airplane Mode toggle
define('WPLT_AIRPLANE', 'true');

// send an email to someone@somewhere.com 
// whenever any post or page is updated
define('WPLT_NOTIFY','someone@somewhere.com')

// or, send a notification to a Slack channel
define('WPLT_NOTIFY', 'https://hooks.slack.com/services/etc');
define('WPLT_NOTIFY_CHANNEL','#channel');
`

= Modification =

You can add code that will be executed depending on server name by modifying the following in wp-local-toolbox.php.

I'd love a pull request if you come up with something useful.

`
if (strtoupper(WPLT_SERVER) != 'LIVE' && strtoupper(WPLT_SERVER) != 'PRODUCTION') {
	// Everything except PRODUCTION/LIVE SERVER

	// Hide from robots
	add_filter( 'pre_option_blog_public', '__return_zero' );

} else {
	// PRODUCTION/LIVE SERVER

}
`

= Notes =

As a special thank you, this plugin will remove the ridiculous `Howdy, ` that is prepended to the username in the admin bar.

You're welcome.

= Credit =

* Plugin disabling from [Mark Jaquith](https://twitter.com/markjaquith): https://gist.github.com/markjaquith/1044546

	* Using this fork from [Andrey Savchenko](https://twitter.com/rarst): https://gist.github.com/Rarst/4402927

* Airplane Mode from [Andrew Norcross](https://twitter.com/norcross): https://github.com/norcross/airplane-mode

* Always showing the admin bar from [Jeff Star](https://twitter.com/perishable): http://digwp.com/2011/04/admin-bar-tricks/

* A healthy refactoring from [Jon Brown](https://twitter.com/jb510) of [9seeds](http://9seeds.com/)

== Installation ==
After installation, you must define constants in the wp-config.php file.
