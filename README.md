NP_TechnoratiTags
===============================

This plugin provides a general tagging system with Technorati and del.icio.us tags support. A list of tags is added to the end of the post by default.

Overview
-------------------------------

Support for:

* adding, updating, deleting posts
* viewing posts, post date statistics
* adding, deleting, renaming tags
* adding, updating, deleting bundles
* returing last update date
* returning error numbers and messages
* caching and automatic protection against throttling of API
* HTTP requests through CURL or fopen (automatic selection from supported methods)
* Backup and restore to/from MySql (implemented in example files)

Download
-------------------------------

https://github.com/NucleusCMS/NP_TechnoratiTags

Support
-------------------------------

http://nucleuscms.org/forum/viewtopic.php?t=15457

Sample
-------------------------------

http://edmondhui.homeip.net/blog/

Skinvar & Template var
-------------------------------

This plugin provides both themplate var and skin var.

The template var `<%TechnoratiTags%>` can be used to allow user to control where tags are link to.

syntax: `<%TechnoratiTags([rss|dtag|ltag])%>` for an item.

By default, `<%TechnoratiTags%>` display tags and links to Technorati. `<%TechnoratiTags(rss)%>` is used in RSS feed to add <category> tag. `<%TechnoratiTags(dtag)%>` display tags and links to del.icio.us. `<%TechnoratiTags(ltag)%>` display tags and links to local tagcloud display (see below for setup)

The skin var is used to add tagcloud and display list of posts for a tag.

syntax: `<%TechnoratiTags([tagsearch|cloud|localcloud|dcloud], [pop|alp], x, [current|all|y])%>`

To display the list of posts to a tag: `<%TechnoratiTags(tagsearch)%>`
To display tagcloud: `<%TechnoratiTags([cloud|localcloud|dcloud], [pop|alp], x, [current|all|y])%>`

###param 1
* **cloud** - display tag cloud that links to Technorati
* **localcloud** - display tag cloud that links to local posts
* **dcloud** - display tag cloud that links to del.icio.us

###param 2
* **pop** - sort tags by popularity, from most to less
* **alp** - sort tags by alphabet

###param 3
* ***num*** - max number of tags to display, **-1** for all tags (note, this may override by the tag % to show plugin option)

###param 4
* **current** - show tags from current blog only
* **all** - show tags from all blog
* ***num*** - show tags from a particular blog, ***num*** is blogid

Tagging System
-------------------------------

As of v0.8.2, this plugin has evolved into a functioning tagging system. To implement the tagging system for Nucleus using this plugin, the following steps are needed:
* add `<%TechnoratiTags(localcloud)%>` into your skin, likely on the sidebar. This list all tags in the current blog and link to a tag search page to show all posts on a tag.
* create a new skin call "tags" from admin menu, you can clone your existing main skin if you want the same look for the local tag search result. Actually you only need the main index skin part.
* modify the new skin's main index, replace `<%blog(default/index,10)%>` (or `<%Showblog()%>` & etc) with `<%TechnoratiTags(tagsearch)%>`. This is the search result part that show all posts for a tag
* replace the sidebar (if you have any w/ the localcloud skinVar) with `<%TechnoratiTags(localcloud)%>`
* create a tags.php in you blog root directory (same place where index.php is). You might need to change the blog URL from blog setting to remove the "/index.php" suffix.

````
<?php
$CONF = array();
$CONF['Self'] = '.';

include('./config.php');

selectSkin('tags'); // change this if your tag skin is not named tags....
selector();
?>
````

If you are using FancyURL, rename the tags.php file to "tags" and add the following to the .htaccess:
````
<FilesMatch "^tags$">
    ForceType application/x-httpd-php
</FilesMatch>
````

If you are using NP_FancierURL2, you need to replace the following line in .htaccess
````
RewriteRule ^(.*)$ index.php?virtualpath=$1 [L]
````
with
````
RewriteRule ^((item|blog|member|archives|archive|category).*)$ index.php?virtualpath=$1 [L,QSA]
RewriteRule ^((tags).*)$ tags.php?virtualpath=$1 [L,QSA] 
````
Each of the above should be on a single line.


To customize the tagcloud's looks add the following to skin's CSS file:
````
.tinyT { font-size:12px;}
.smallT { font-size:16px;}
.mediumT { font-size:18px;}
.largeT { font-size:20px; font-weight: bold;}
````


del.icio.us Support
-------------------------------
New to v0.9.0, del.icio.us is supported, the following is possible:
* show tags and link to del.icio.us
* tag a new blog post to del.icio.us

To implement the support:
-------------------------------
1. go to plugin option set "Add post to each tag in del.icio.us? (user need to set his/her login & password from member setting)" to yes
2. go to member setting and set the user and password to add post to your del.icio.us account
3. To show the tag @ del.icio.us, change templete var to `<%TechnoratiTags(dcloud)%>`
4. To show a tagcloud of blog's post to del.icio.us, change skin var for the tag cloud to `<%TechnoratiTags(dcloud,...)%>`

