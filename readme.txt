=== (MB) YouTube Widget ===
Contributors: Mechabyte
Tags: youtube, videos, recent videos, subscribe, social, widget
Requires at least: 3.0
Tested up to: 3.6
Stable tag: 1.05
License: GPLv2 or later
License URI: http://www.gnu.org/licenses/gpl-2.0.html

The YouTube videos widget lets you quickly and easily display your most recent YouTube videos in your blog's sidebar.

== Description ==

The YouTube videos widget lets you quickly and easily display your most recent YouTube videos in your blog's sidebar.

### Features 

* The YouTube videos widget is fully-based on the WordPress [Plugin API](http://codex.wordpress.org/Plugin_API).
* Uses hooks and filters to allow for maximum hackability by theme designers and developers.
* Organized straightforwardly so that it's easy to modify and style.

### Usage

The YouTube videos widget is ready to use as-is, although you can easily customize it to your liking with a bit of code.

You can also insert a plain list of your recent videos into posts and pages with the shortcode `mechabyte_youtube`. For example, to load **5** videos from **freddiew** that **open in new tabs**, you'd use `[mechabyte_youtube username="freddiew" videos="5" tab="true"]`.

###Custom Styles

The YouTube videos widget loads it styles by hooking into Wordpress' `wp_enqueue_scripts` action. To remove the default styling you need to remove the our `enqueue_scripts` function from the hook.
        
        remove_action( 'wp_enqueue_scripts', array( 'mechabyteYouTube', 'enqueue_scripts' ) );
  
###Custom Output

The end user has two options of displaying their YouTube videos: in plain or decorated lists. When modifying the output of the plugin you must use one of two filters that this plugin uses: `mbYT_construct_plain` and `mbYT_construct_decorated`. Before you add your own functions to either filter, you must remove the default ones that are used automatically: `mbYT_construct_plain_default` and `mbYT_construct_decorated_default`.

        remove_filter( 'mbYT_construct_plain', array( 'mechabyteYouTube', 'mbYT_construct_plain_default'), 10, 3  );
        remove_filter( 'mbYT_construct_decorated', array( 'mechabyteYouTube', 'mbYT_construct_decorated_default'), 10, 3  );
        
Now it's time to get creative. When creating a function that will loop through the videos you have complete control over how you want to display the content. Just keep in mind that in the end, you should be outputting `<li>` elements. When filters are being applied to the YouTube content, there are three arguments that are being passed through: `$youtube_videos` (`array`, the array of videos), `$number` (`integer`, the number of videos the user wishes to display), and `$tab` (`boolean`, a true/false value of whether or not the user wants to open video links in a new tab).
Keep in mind that you have access to the following pieces of information when creating your video loop:

        $item['title'] // Video title
        $item['videoID'] // Video ID
        $item['viewCount'] // Video view count
        $item['published'] // Video publish date -- UNIX
        $item['duration'] // Video duration in hh:mm:ss
        $item['numLikes'] // Video likes count
        $item['link'] // Video link
        $item['image']['default'] // 'Default' thumbnail (low quality)
        $item['image']['mqdefault'] // Medium quality thumbnail
        $item['image']['hqdefault'] // High quality thumbnail
        
Here's an example of some code that adds a custom function to our `mb_construct_plain` filter (assuming we've already removed the default functions). Keep in mind the `3` at the end. That lets Wordpress expect our three arguments that will be passed through. Check out Wordpress' [add_filter()](http://codex.wordpress.org/Function_Reference/add_filter) page for more info.

        add_filter( 'mb_construct_plain', 'construct_plain_example', 10, 3 );
        
        function construct_plain_example( $youtube_videos, $number, $tab ) {
            $output = '';
            foreach( $youtube_videos as $youtube_video ) {
                // If we've reached the user's display limit, end the loop
                if( $i == $number )
                  break;
                $output .= '<li>';
                $output .= '<a href="' . . '"';
                // If the user has selected to open videos in a new tab, specify the link target
                if($tab) {
                  $output .= ' target="_blank"';
                }
                $output .= '>';
                $output .= $youtube_video['title'];
                $output .= '</a>';
                $output .= '</li>';
                
                $i++;
              }
            return $output;
        }
        
Pretty basic, but it allows you to see a general example of how to loop through the objects and return the modified data.
The above code will produce a list element like this one:      

        <li><a href="http://www.youtube.com/watch?v=3L-rrkyvApU" target="_blank">Real Life Portal Gun</a></li>

== Installation ==

1. Copy the `mb-youtube` directory into your `wp-content/plugins` directory
1. Navigate to the "Plugins" dashboard page
1. Locate the menu item that reads "(MB) YouTube Widget"
1. Click on "Activate"

== Frequently asked questions ==

= How can I make this plugin match my Wordpress theme? =

Easy! If you check out the 'Description' tab, there's documentation on how to use the plugin's style hooks & filters to modify the output. If you have trouble, try contacting your theme's developer and ask them to include some styling rules in the theme's `functions.php` file.

== Screenshots ==

1. Widget customization `/assets/screenshot-1.png`
2. Widget front-end display as a decorated list `/assets/screenshot-2.png`
3. Widget front-end display as a plain list `/assets/screenshot-3.png`

== Changelog ==

1.05 - Updated plugin with `mechabyte_youtube` shortcode to display videos in posts and pages

1.01 - Updated the video data array to store the date as Unix time

1.0 - Initial release

== Upgrade notice ==
