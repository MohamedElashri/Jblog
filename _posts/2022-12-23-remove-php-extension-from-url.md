---
layout: post
title: "Remove PHP extension from URL"
categories: [PHP, Web Development, Server, .htaccess]
keywords: "PHP, Web Development, Server, .htaccess"
comments: true
---

I always found the `.php` at the end of some website pages I visit (and when I used wordpress) very unattractive and annoying. I wanted to remove it. I found a way to do it using `.htaccess` file. I will show you how to do it. If you want to remove the `.php`, `.html`, or `.htm` extensions from your website's URLs, you can do so by using the `.htaccess` file. This file is a configuration file that allows you to control various aspects of your website's behavior, including how URLs are displayed to visitors. We will rely on the `.htaccess` ability to rewrite URLs to remove the extensions from the URLs.

The first thing is to make sure that your website is hosted on a server that supports the use of `.htaccess` files. Not all servers support this file, so check with your hosting provider if you're not sure. There are many servers that do not support `.htaccess` files, so if you're using one of these, you'll need to find another way to remove the extensions from your URLs. One famous example of the server that supports `.htaccess` files is Apache.

We create the `.htaccess` file in the root directory which is usually the same directory as your index.php or index.html file. You can create the file using any text editor. If you're using a Mac, you can use TextEdit. If you're using Windows, you can use Notepad. If you're using Linux, you can use any text editor you like. You can also use an FTP client to create the file. If you're using FileZilla, you can right-click on the root directory and select New File. Then, you can name the file `.htaccess` and save it.

P.S: I don't recommend using FTP protocol to create the `.htaccess` file. It's better to use SFTP or SSH protocol. I know that this might not be possible for some people on some shared hosting plans, but if you have the option, I recommend using SFTP or SSH protocol.

Now, we need to add the following code to the `.htaccess` file:

```
RewriteEngine On
RewriteCond %{REQUEST_FILENAME} !-d
RewriteCond %{REQUEST_FILENAME}\.php -f
RewriteRule ^(.*)$ $1.php

RewriteCond %{REQUEST_FILENAME} !-d
RewriteCond %{REQUEST_FILENAME}\.html -f
RewriteRule ^(.*)$ $1.html

RewriteCond %{REQUEST_FILENAME} !-d
RewriteCond %{REQUEST_FILENAME}\.htm -f
RewriteRule ^(.*)$ $1.htm
```

This code tells the server to rewrite any URL that ends in .php, .html, or .htm to the same URL without the extension. For example, if a visitor requests the URL `www.melashri.net/about.php`, the server will rewrite the URL to `www.melashri.net/about` and serve the content from the `about.php` file.

If you're using WordPress, you can add this code to the `.htaccess` file in the root directory of your WordPress installation. If you're using another CMS, you can add this code to the `.htaccess` file in the root directory of your website. Save your .htaccess file and upload it to your server. Your changes should take effect immediately, and you should now be able to access your website's pages without the extensions.


It's worth noting that removing the extensions from your URLs can make it harder for search engines to understand the content of your pages. You may want to consider using other methods, such as using descriptive URLs or using the rel="canonical" tag, to help search engines understand the content of your pages. For example if you have a page with the URL `www.melashri.net/about.php`, you can use the rel="canonical" tag to tell search engines that the page with the URL `www.melashri.net/about` is the canonical version of the page. This will help search engines understand that the page with the URL `www.melashri.net/about` is the same page as the page with the URL `www.melashri.net/about.php`. The code for the rel="canonical" tag is as follows:

```
<link rel="canonical" href="https://www.melashri.net/about" />
```

You can add this code to the head section of your page. You can also add this code to the `.htaccess` file. If you're using WordPress, you can add this code to the `functions.php` file in your theme's directory. If you're using another CMS, you can add this code to the `header.php` file in your theme's directory. You can also add this code to the `index.php` file in your theme's directory. If you're using a static website (like me), you can add this code to the `index.html` file in your website's root directory.



