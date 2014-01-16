---
layout: post
title: Uri.TryCreate Is Not Reliable on .NET 3.5
categories: .net3.5 bug
---

I ran into this issue while running through a load of SharePoint pages extracting URLs for a migration project:

[http://stackoverflow.com/questions/2814951/system-uriformatexception-invalid-uri-the-hostname-could-not-be-parsed](http://stackoverflow.com/questions/2814951/system-uriformatexception-invalid-uri-the-hostname-could-not-be-parsed)

Essentially, you cannot just use Uri.TryCreate to 100% determine whether a supplied string is a valid Uri.

In my case, I was able to do some simple pre-processing to exclude "mailto:" types.

[http://connect.microsoft.com/VisualStudio/feedback/details/475897/uri-trycreate-throws-uriformatexception](http://connect.microsoft.com/VisualStudio/feedback/details/475897/uri-trycreate-throws-uriformatexception)