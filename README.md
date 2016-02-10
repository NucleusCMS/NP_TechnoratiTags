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

###param1
* **cloud** - display tag cloud that links to Technorati
* **localcloud** - display tag cloud that links to local posts
* **dcloud** - display tag cloud that links to del.icio.us

###param2
* **pop** - sort tags by popularity, from most to less
* **alp** - sort tags by alphabet

###param3
* ***num*** - max number of tags to display, **-1** for all tags (note, this may override by the tag % to show plugin option)

###param4
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

License
-------------------------------

Software License Agreement (BSD License)

Copyright (C) 2005-2006, Edward Eliot.
All rights reserved.
   
Redistribution and use in source and binary forms, with or without
modification, are permitted provided that the following conditions are met:

* Redistributions of source code must retain the above copyright
  notice, this list of conditions and the following disclaimer.
* Redistributions in binary form must reproduce the above copyright
  notice, this list of conditions and the following disclaimer in the
  documentation and/or other materials provided with the distribution.
* Neither the name of Edward Eliot nor the names of its contributors 
  may be used to endorse or promote products derived from this software 
  without specific prior written permission of Edward Eliot.

THIS SOFTWARE IS PROVIDED BY THE COPYRIGHT HOLDER AND CONTRIBUTORS "AS IS" AND ANY
EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE IMPLIED
WARRANTIES OF MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE ARE
DISCLAIMED. IN NO EVENT SHALL THE COPYRIGHT OWNER OR CONTRIBUTORS BE LIABLE FOR ANY
DIRECT, INDIRECT, INCIDENTAL, SPECIAL, EXEMPLARY, OR CONSEQUENTIAL DAMAGES
(INCLUDING, BUT NOT LIMITED TO, PROCUREMENT OF SUBSTITUTE GOODS OR SERVICES;
LOSS OF USE, DATA, OR PROFITS; OR BUSINESS INTERRUPTION) HOWEVER CAUSED AND
ON ANY THEORY OF LIABILITY, WHETHER IN CONTRACT, STRICT LIABILITY, OR TORT
(INCLUDING NEGLIGENCE OR OTHERWISE) ARISING IN ANY WAY OUT OF THE USE OF THIS
SOFTWARE, EVEN IF ADVISED OF THE POSSIBILITY OF SUCH DAMAGE.

Change Log (started 11/11/2006)
-------------------------------

* V0.5 - 1st public release
* V0.6 - fixed disappearring + in add item
* V0.7.1 - added tag cloud
* V0.8.2 - added tagging system
* V0.8.3 - added new parameters to control tag sorting and max tags to display
* V0.8.4 - fix missing formating bug
* V0.8.5 - add CSS 
* V0.8.6 - use sql_query 
* V0.8.7 - minor fix on getTableList()
* V0.9.0
 * allow per blog tag cloud - current, all, by blogid
 * show tag count
 * option for tag search title
 * outbound tag support to del.icio.us
* V0.9.1 - fixed broken url when insert tags at the end of post
* V0.9.2
 * skip tag update to del.icio.us if there is no user/password set
 * fix tag cloud to ensure it displays according to PlusSwitch option
 * rename templete var dcloud switch to dtag
* V0.9.3
 * fixed UTF-8 multi-bytes encoding problem wih tag search
 * FancyURL support (Thanks Shi!)
 * show popular tag only option
* V0.9.4
 * tagcloud idle display without tag select
 * ltag templateVar switch to show local tag
* v0.9.5
 * fix tagsearch result double http link bug
 * error checking for missing blog object in doSkinVar()
 * port NP_AutoComplete by anand to NP_TechnoratiTags, allow tag auto completion
* v0.9.6
 * optimize auto complete init
 * change list of tag by date decrement
 * fix tag cloud display of draft on search and cloud
 * fix add/delete post incorrect URL
 * fix top tags striping bug (thx Rico)
 * support for multi-blog setup 
* 13/01/2007:
 * Modified cache to make it username specific
* 11/11/2006:
 * Added support for notes field in results returned
 * Added LastErrorString() method
 * Replaced LastError() method with LastErrorNo() but kept original as alias for backwards compatibility
 * Added examples for exporting/importing posts to/from MySql (see examples folder)
 * Added example to print a simple table of results (see examples folder)

Public Methods
-------------------------------

* LastErrorNo()
* LastError (alias for LastErrorNo())
* LastErrorString()
* GetLastUpdate()
* GetAllTags()
* RenameTag()
* GetPosts()
* GetRecentPosts()
* GetAllPosts()
* GetDates()
* AddPost()
* DeletePost()
* GetAllBundles()
* AddBundle()
* DeleteBundle()

Full documentation for these methods to follow. For now see examples and source for parameters and usage.
