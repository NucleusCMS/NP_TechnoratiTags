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
  - add <%TechnoratiTags(localcloud)%> into your skin, likely on the sidebar. This list all tags in the current blog and link to a tag search page to show all posts on a tag.
  - create a new skin call "tags" from admin menu, you can clone your existing main skin if you want the same look for the local tag search result. Actually you only need the main index skin part.
  - modify the new skin's main index, replace <%blog(default/index,10)%> (or <%Showblog()%> & etc) with <%TechnoratiTags(tagsearch)%>. This is the search result part that show all posts for a tag
  - replace the sidebar (if you have any w/ the localcloud skinVar) with <%TechnoratiTags(localcloud)%>
  - create a tags.php in you blog root directory (same place where index.php is) with this [[technoratitagstags.php|content]]. You might need to change the blog URL from blog setting to remove the "/index.php" suffix.

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
