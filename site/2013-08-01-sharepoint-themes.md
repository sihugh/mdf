---
layout: post
title: Don't Use OOB SharePoint Themes
categories: sharepoint themes
---

MOSS 2007 ships a number of default themes. There are a number of issues with these regarding enabling caching for CSS and images used in the themes, supporting the modification of these artefacts without requiring the theme to be updated, but also around the validity of the CSS itself.

I will cover the caching and updating points in another post (although the key elements behind this are covered in [Heather Solomon's post](http://www.heathersolomon.com/blog/archive/2008/01/30/SharePoint-2007-Design-Tip-Import-your-CSS-for-SharePoint-Themes.aspx "Heather Solomon's post"). This post flags up the issue of broken CSS one of the side effects of which is extra HTTP requests which will never be served.

I'm going to cut to the chase and advise that you do not permit the out of the box themes to be used. (Not that anyone is still deploying new 2007 installations are they?) I’m going to pick on a few specific problems, but they are indicative of MOSS themes as a whole.

### Not Cacheable ###

Check out Fiddler when you make a request to a theme enabled site. Every request brings down a raft of images associated with the theme. Why no browser caching?
The theme files (theme.css, mossextension.css and all the associated images) are served with a Cache-Control header set to "private,max-age=0". This means that every time you request those resources your browser will head all the way back to the server to check whether they've been updated.

### Stored In The Content Database ###

When you apply a theme to a SharePoint site, it grabs all the styles and images in the folder that defines the theme and stores them in the site (i.e. in the content database) under the "_themes" folder.

This has the side effect of making it difficult to make any changes to themes which have been applied, but also means that there is a lot of database access just to serve out a set of static image and css files.

### Broken Links - Bad CSS and Missing Images ###

The Simple theme contains two references to 'http://localhost/topnavselected_simple.gif'. This will never be resolved which results in 404 errors when requesting pages with that theme applied.

{% gist 4121350 %}

The Verdant theme has links to the 'siteTitleBKGD_vintage.jpg' image. Sadly, there’s a gif, but no jpg by that name in this theme.

{% gist 4121601 %}

There are further problems than this, but if you've got performance issues on your MOSS farm and you have themes enabled, consider what impact they have.

I've looked at the theme definitions on 2010 and they seem similarly afflicted.


**Next post: How to fix your themes!**
