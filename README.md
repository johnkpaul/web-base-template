# jQuery.com jquery-wp-content

This is a set of plugins, themes, and configuration files for jQuery's website infrastructure, which is powered by WordPress. It is designed as a custom content directory. So think of `jquery-wp-content` as your `wp-content` directory.

## Prerequisites 

This install guide assumes you already have certain prerequisites already configured within your environment.

* Apache
* Mysql
* PHP

## Installation

1. Configure your local webserver with a virtual host that covers the relevant jQuery domains, such as `*.jquery.com` and `*.jqueryui.com`, all pointing to the same root. For example, in Apache:

    ```
    <VirtualHost *:80>
    ServerName local.jquery.com
    ServerAlias *.jquery.com *.jqueryui.com *.jquery.org *.qunitjs.com *.sizzlejs.com *.jquerymobile.com
    DocumentRoot "/srv/www/jquery"
      <Directory /srv/www/jquery>
         Options All
         AllowOverride All
         Order allow,deny
         Allow from all
      </Directory>
    </VirtualHost>
    ```

    You do not need to configure your `/etc/hosts` file for `local.*` because `jquery.com`'s DNS handles this for you.

1. Place the WordPress core files in the document root you chose. (Don't install it.) You can do this any number of ways:

    <ul>
      <li>Download the latest version from http://wordpress.org/latest.zip</li>
      <li>Check out the latest tag from http://core.svn.wordpress.org/tags/</li>
      <li>Clone the official WordPress Github mirror at http://github.com/wordpress/wordpress/</li>
    </ul>

1. Clone `jquery-wp-content` into place, so you have a file tree that looks like this:

    ```
    index.php
    jquery-wp-content/
    license.txt
    readme.html
    wp-activate.php
    ...
    ```

1. Copy `jquery-wp-content/wp-config-sample.php` and move it up one directory, to `wp-config.php`. Fill in your database credentials.

1. Create an .htaccess file with the following content into that same document root:

    ```
    RewriteEngine On
    RewriteBase /
    RewriteRule ^index\.php$ - [L]

    RewriteRule ^resources/?$ index.php [L]
    RewriteRule ^resources/(.+) gw-resources/%{HTTP_HOST}/$1 [L]

    # Add a trailing slash to the wp-admin of a subsite.
    RewriteRule ^([_0-9a-zA-Z\.-]+/)?wp-admin$ $1wp-admin/ [R=301,L]

    RewriteCond %{REQUEST_FILENAME} -f [OR]
    RewriteCond %{REQUEST_FILENAME} -d
    RewriteRule ^ - [L]

    # Handle wp-admin, wp-includes, and root PHP files for subsites.
    RewriteRule  ^[_0-9a-zA-Z\.-]+/((wp-admin|wp-includes).*) $1 [L]
    RewriteRule  ^[_0-9a-zA-Z\.-]+/(.*\.php)$ $1 [L]

    RewriteRule . index.php [L]
    ```
1. Make sure that you have assigned your WordPress files and directories the correct permissions.  
For example, if your WordPress files are in the directory ```wordpress```, and you are running Apache under Mac OS X with the ```_www``` user:
    ```
    sudo chown -R _www wordpress
    sudo chmod -R g+w wordpress
    ```

1. Go to `http://local.jquery.com` and walk through the standard WordPress installation. `jquery-wp-content` includes a special install script that will initialize the entire network.

1. Be sure to have node >= 0.8 installed on your system.  Some sites, such as download.jqueryui.com, require that version or greater.

## Auto-Updates
Changes pushed to master will be pulled onto the stage domain.

## Copyright

Copyright 2012 jQuery Foundation and other contributors. All rights reserved.

The jquery-wp-content repository contains themes for rendering all jQuery Foundation web sites.

### What is licensed

The contents of these web sites are all available under terms of the MIT license ( http://jquery.org/license ).

Special exception: Code samples are given away for you to freely use, for any purpose. For code samples in API sites
and Learn articles (unlike the source code of jQuery projects) you don't even have to say where you got the code from.
Just use it.

The PHP files in the jquery-wp-content repository are a derivative work of WordPress, and available under the
terms of the GPL license ( http://codex.wordpress.org/License )

### What is not licensed

The theme, design, layout, look-and-feel of the jquery-wp-content repository, including all html, css, images, and
icons, is not licensed for use. Not by the MIT license or any other license. It is copyrighted. You don't have
permission to use it in any way for any purpose, commercial or otherwise. If you have questions about this, please
ask a member of the jQuery Content Team.
