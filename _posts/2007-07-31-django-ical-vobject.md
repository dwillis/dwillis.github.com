---
layout: post
title: Django, iCal and vObject
---

For [one of our Django applications at work](http://web.archive.org/web/20071229080437/http://projects.washingtonpost.com/2008-presidential-candidates/tracker/) we received a request to add iCal feeds to accompany the RSS feeds available for each candidate’s page (example [here](http://web.archive.org/web/20071225222926/http://projects.washingtonpost.com/2008-presidential-candidates/tracker/candidates/hillary-clinton/)). I first thought about doing this using a hard-coded template, [the way described in March on boomby.com](http://web.archive.org/web/20071225222926/http://boomby.com/?p=4). So I did and it worked - the directions are great. Since I happened to be at OSCON, I showed it to [Jacob Kaplan-Moss](http://www.jacobian.org/), who immediately recommended that I check out [vObject](http://vobject.skyhouseconsulting.com/), a Python library for generating iCal files.

Why use vObject? Well, for one, it eliminates the need for a template file, and the [iCal spec](http://www.ietf.org/rfc/rfc2445.txt) being a slightly cranky thing, that’s for the better. How cranky? Well, it doesn’t like carriage returns in template, for one thing. It also means that Django treats iCal feeds much the same as RSS feeds in terms of generation. So using vObject means you set up the file in the view, call the serialize() function to fill out the standard stuff and return an HttpResponse, setting the mimetype to text/calendar.

Seems pretty simple? It is, with one exception. Internet Explorer (of course). To ensure that Outlook correctly handles the iCal files, you need to add a few lines to the calendar object and to each event. Specifically, you need to do the following inside your view:


    cal = vobject.iCalendar()
    cal.add('method').value = 'PUBLISH'  # IE/Outlook needs this
    for event in event_list:
        vevent = cal.add('vevent')
        ... # add your event details
    icalstream = cal.serialize()
    response = HttpResponse(icalstream, mimetype='text/calendar')
    response['Filename'] = 'filename.ics'  # IE needs this
    response['Content-Disposition'] = 'attachment; filename=filename.ics'

Outlook seems to require three datetime fields for each event: DTSTART, DTEND and DTSTAMP (even if you don’t have values for all of them). Other consumers of .ics files do not, and what works on Windows with Firefox and Outlook may not with IE and Outlook. But it does work if you follow the steps, and vObject makes it much easier.

The only other thing you need is to set up the url in Django’s urlconf and have it call the view. No template needed! Here’s a sample iCal file listing [Mike Huckabee’s upcoming visits](http://web.archive.org/web/20071225222926/http://projects.washingtonpost.com/2008-presidential-candidates/tracker/ical/candidates/mike-huckabee/).

Thanks to Jacob, Malcolm Tredinnick and Simon Willison for their advice on this. Any mistakes are mine, however, and comments or suggestions are welcome.