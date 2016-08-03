#Newsletter tutorial for ProcessWire

This is a tutorial for a basic newsletter setup which covers the basics of:

- creating newsletters
- sending tests
- sending out the newsletter

It doesn't cover the process of collecting and managing subscribers.

**The tutorial was created using**

- [ProcessWire 3.0.28](http://processwire.com/)
- module [Admin Custom Pages](http://modules.processwire.com/modules/process-admin-custom-pages/) for adding custom menu item to the admin
- [Emogrifier](https://github.com/jjriv/emogrifier) to inline CSS into newsletter body

While this newsletter setup is fully functional, for production use it is strongly recommended to use a 3rd party service for sending emails (eg. MailGun through WireMailMailgun module).


## Install ProcessWire and modules

1. Download ProcessWire and upload to your server.

2. Upload and extract [ProcessWire Newsletter Tutorial](site-pwnl.zip) site profile to your PW install root on server.

1. Install ProcessWire (visit your site online to start) and select the "ProcessWire Newsletter Tutorial" site profile from the available site profiles dropdown.

1. (optional) Install [Admin Custom Pages](http://modules.processwire.com/modules/process-admin-custom-pages/) module

1. (optional) Install WireMailMailgun module

After install, all templates, fields and contents will be installed to your site. You can continue browsing the admin to see how it works, or continue this tutorial for the details.

Note that if you get the error message saying "FieldtypeAdminCustomPagesSelect does not exist", just go to the Modules section and click on the "Refresh" button.


## Set up templates

You'll need the following templates. As for now, just create these files in "/site/templates/" folder.

- Article (name "article", file "article.php"): template for articles that you can reference in the newsletter
- Newsletter (name "newsletter", file "newsletter.php"): this is the template for the newsletters
- Subscriber (name "subscriber"): template for newsletter subscribers. No template file needed because they are not meant to be viewable.

We will add fields to them later, having only field "Title" is OK for now.

There's also the default "basic-page" template that comes with ProcessWire. We'll use it for parent pages for articles, newsletters and subscribers.


## Create pages

Having all templates set up it's time to create these pages under "Home". Use template "basic-page" for all.

- Articles
- Newsletters
- Subscribers

### (optional) Add navigation item for newsletters for easy access

We will also add a main navigation item called "Newsletters" to the admin with the help of module Admin Custom Pages.
To achieve this, you'll need to create a new template file "_newsletter.php" (see its content on GitHub).

After that create a new page called "Newsletters" under the "Admin" page.
Set its template to "admin", and set its Process to "ProcessAdminCustomPages".
Then select "_newsletter.php" from the "Template" select box that appears.

If all went well the new navigation item should appear - if not, check line number 4 and set a correct path to your Newsletters page. However, it won't show any newsletters as there's none yet.


## Create fields

Create these fields and assign to the corresponding templates.

### Fields for template "article":

- Title (name "title", built-in, type: Text)
- Excerpt (name "excerpt", type: Text) - this will contain the intro of the main content
- Featured image (name "featured_image", type: Image) - this will be the main image of the article


### Fields for template "newsletter":

- Title (name "title", built-in, type: Text)
- Excerpt (name "excerpt", type: Text) - this will contain the intro of the main content
- Featured image (name "featured_image", type: Image) - this will be the main image of the article
- Body (name "body", built-in, type: Textarea with CKEditor) - the main content of the newsletter
- Article reference (name: "article_ref", type: Page) - select box to choose articles to include in the newsletter
- Newsletter test email (name: "newsletter_test_email", type: Text) - field to enter email address(es) to send test (preview) emails before send

Field "article_reference" needs to be set to "Multiple pages (PageArray)" on the "Details" tab.
On the "Input" tab set "Parent of selectable Page(s)" to "Articles" page, and set "Template of selectable pages" to "article".
Set "Input field type" to "AsmSelect".


### Fields for template "subscriber":

- Title (name title, built-in, type: Text) - this will contain the full name of a subscriber
- Email (name email_subscriber, type: Email) - field to hold the subscriber's email address


### Fields for template "basic-page":

- Title (name title, built-in, type: Text)


## Add some content

When done, add a few dummy articles under page "Articles" using the "article" template.
Enter title, excerpt, add dummy body content and upload a featured_image.

We'll also need some subscribers too so add some under "Subscribers".


## Template files

We'll keep the default content of basic-page.php with a small modification of echoing $page->body in it.

As for article.php, we will just use the same as in basic-page.php, so here we only include that file. The same goes for home.php, just in case someone reads it.

### newsletter.php

This is where a newsletter is displayed or sent out (when called from AJAX).
Because it's viewable as a page, you can preview the newsletter and link to this page in the newsletter for those who prefer reading in browser.

This file sets up variables for file "_newsletter-content.php" which is responsible for displaying the newsletter.

Make sure to examine this file and modify hardcoded values like contact data, facebook, etc. 
Normally you would need to move these information to a central location to make it easier to maintain.

To make the admin page of a newsletter fully functional we'll need a hook to add some extra fields in "/site/ready.php".

This is where "/site/temlates/scripts/newsletter.js" is added to the admin, which is needed to do AJAX (send tests and send out newsletters).
It also sets the "View" link in the admin to open the newsletter in a panel that slides in from the left, so you can preview the newsletter without having to leave the admin editor. However, this preview is updated only if the newsletter was saved.

### _newsletter-content.php

This file is called by newsletter.php and shows the newsletter's content. 

It's the html boilerplate of the newsletter, and uses "/site/templates/styles/newsletter.css" for styling.
You can modify both of them if you need custom markup or style.

Note that all CSS is inlined in both regular page view and when sending out newsletters, to make them identical in both cases.


## Create your first newsletter

Adding newsletters is as easy as adding a new page under "Newsletters" using the "newsletter" template.

Available fields and how they are used:

- Title: newsletter subject
- Excerpt: this will be the so-called preheader text
- Featured image: main image of the newsletter
- Body content: the main content - you can add images, their source will be set to absolute path on send
- Articles (optional): if used, selected articles' featured_image, excerpt will be added to the newsletter, plus a "Read more" button that links to the corresponding article page. You can modify the order of the articles by dragging them up or down.


## Sending newsletters

### Test send

You can send a test email by entering an email address to the "Newsletter test email" field and clicking on the "Send test" button.
If you would like to send tests for multiple addresses, separate them by a comma.

Note that you need to save the newsletter first to be able to send a test email.


### Send newsletter for subscribers

At the bottom there's a "Send newsletter" button. It's available only if there is at least one active (published) subscriber.

Clicking on it a confirmation dialog will come up. Selecting "Yes" will start sending the emails.

Note that no queuing is used so if you have a large number of subscribers I recommend using a 3rd party service.
[WireMailMailgun](https://github.com/plauclair/WireMailMailgun) is a module to integrate the MailGun service and works out of the box with this newsletter setup.


## Final thoughts

This tutorial is very basic but I hope it will get you started. Please share your comments if you find something to improve.
