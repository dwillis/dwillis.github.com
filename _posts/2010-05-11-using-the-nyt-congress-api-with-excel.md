---
layout: post
title: Using the NYT Congress API with ... Excel?
---

It's true that Excel has been a decreasing part of my toolkit for several years now, and that I never quite had the love for it that I do for various database managers. But I'm guessing that's the exception, not the rule, in the broader journalism community. So when it came time to propose a [lightning talk](http://web.archive.org/web/20111221230824/http://data.nicar.org/node/3766) for the 2010 CAR Conference last week, I chose to pull out the ol' spreadsheet and show how you could get started with the [NYT's Congress API](http://developer.nytimes.com/docs/congress_api) with a familiar tool.

To do this, I had to not only drag out Excel but also do it on Windows, since [Excel's Web Query feature](http://www.mrexcel.com/tip103.shtml) isn't available on the Mac. (You could also do this, albeit in a slightly different manner, using OpenOffice and Google Spreadsheets.) Here's how it works using Excel.

First, you'll need an API key. To get one, go to [The Times Developer Network](http://developer.nytimes.com/) and [register](http://developer.nytimes.com/apps/register) (note: you'll need to be a registered user of nytimes.com first).

![API Key Signup](/static/images/key_signup.png)

You're registering an "application", and then you can add specific API keys to that account. Let's add one for the Congress API. The key itself is a longish string of letters and numbers that gets appended to every API request URL, including the ones we'll make from Excel. Let's copy the API key so we can easily grab it (note that this particular key has been disabled, so using it won't work).

![API key](/static/images/api_key.png)

Let's find an API call that we can use be looking at the [Congress API's documentation](http://developer.nytimes.com/docs/congress_api). Let's pick the ["members leaving office" response](http://developer.nytimes.com/docs/congress_api#h3-members-leaving), otherwise known as the casualty list. All that's required is the chamber ('house' or 'senate') and the congress (currently only the 111th-forward is supported). If we choose the House, the URL [will look like this](http://api.nytimes.com/svc/politics/v3/us/legislative/congress/111/house/members/leaving?api-key=YOUR-API-KEY), except that you'll need to specify your Congress API Key.

The version number should be "v3″ and you don't need to specify a format after leaving (xml is the default). You should quickly get an xml file that looks roughly like this:

![The response](/static/images/request.png)

To get that xml into Excel, we're going to use Excel's Import Data feature. I'm not one of those cool kids who has Excel 2007 at their fingertips, so I'm going to use Excel 2002. Import Data can be found at Data -> Get External Data -> Import Data.

![Import part 1](/static/images/import_data.png)

Then change the file type to xml and paste the full API url into the box just above the file type.

![Import part 2](/static/images/import_data2.png)

It works for local files and Web urls. Then click on "Open" to start the process. The import process consists of Excel asking you where to put the file. Just click "OK" and you should soon see something like this:

![Results](/static/images/results.png)

The header row in row 2 isn't perfect, but it should suffice. You probably don't need the copyright statement in column A. But now you've got a way to pull data into Excel from an API! If you have questions or comments, please don't hesitate to post them below. If you're having issues with the API, the [forum](http://developer.nytimes.com/forum) is the best place to head.