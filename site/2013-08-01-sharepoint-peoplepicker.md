---
layout: post
title: SharePoint Peoplepicker Angst
categories: sharepoint peoplepicker
---

I’ve recently been trying to run down some issues around service accounts gaining access to site collections in one of our web apps.  We’ve set the [siteuseraccountdirectorypath](http://technet.microsoft.com/en-us/library/cc263328%28v=office.12%29.aspx) and that is working as expected and users under that OU are resolved correctly.

Unfortunately, some (but not all) of the service accounts that reside in the OUs specified in the [peoplepicker-serviceaccountdirectorypaths](http://technet.microsoft.com/en-us/library/cc263012%28v=office.12%29.aspx) property were getting the ‘The user does not exist or is not unique’ error when trying to access the site and could not resolve via the people picker in the site.

## TL;DR – What fixed the problem? ##

- Make sure the service account directory paths property is correct
- Make sure the service account is a farm administrator.
    - Make sure there are no ‘Deny’ policies in the Policy for Web Application page interfering with this account.

- **If the user still does not resolve in the people picker on the site, remove it from any web app policies it is defined in and re-add it.** (This was our issue)

- Get the service account to try to access the site again.
    - Ours was a search indexing service account so we restarted the crawler.
- The user should now be able to get access (and should now resolve in the people picker).

## Investigation ##
The problem was that the service account did not have access, but as we could not log in as the account to try things, we were trying to fix the “would not resolve in people picker” issue first to see if it was the same issue.

Using [NetMon](http://www.microsoft.com/en-us/download/details.aspx?id=4865) I could see that People picker only does an LDAP query under the [siteuseraccountdirectorypath](http://technet.microsoft.com/en-us/library/cc263328%28v=office.12%29.aspx) base OU.  This means that the issue was not to do with the connection to AD from the site collection as it was never actually querying AD at a location where service accounts were.

The [Technet reference](http://technet.microsoft.com/en-us/library/cc263012%28v=office.12%29.aspx) does indicate that SharePoint has to know about the account already (emphasis mine):

> Enables a **farm administrator** to manage the site collection that has a specific organizational unit (OU) setting as defined in the Setsiteuseraccountdirectorypath setting

The service accounts that appeared in the people picker selector were present in the User Information List in the root web of the site collection.

I set up a test user with no access to the site and even though he got the Access Denied page when browsing to the site, the user account was added to the User Information List at first browse.  This implied that it was something different than just lack of permissions.
Eventually we determined that the accounts that were not being resolved were all in an OU which had recently been added to the [peoplepicker-serviceaccountdirectorypaths](http://technet.microsoft.com/en-us/library/cc263012%28v=office.12%29.aspx) property.  They had been in the web application policies list since the web app had been set up, but this had obviously not been working.

Removing and adding the service account in the [Policy for Web Application page](http://technet.microsoft.com/en-us/library/cc262617%28v=office.12%29.aspx) seemed like a bit of a clutching at straws tactic, but after I’d restarted the crawl service everything sprang into life.
