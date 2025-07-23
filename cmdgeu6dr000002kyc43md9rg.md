---
title: "Printing Greenhopper Cards From Jira"
datePublished: Mon Mar 08 2010 22:28:32 GMT+0000 (Coordinated Universal Time)
cuid: cmdgeu6dr000002kyc43md9rg
slug: printing-greenhopper-cards-from-jira-723bad03f958

---

In an attempt to keep a better historical log of our work, [my company](http://locamoda.com/) switched from a whiteboard and 3x5 cards to [Greenhopper](https://studio.plugins.atlassian.com/wiki/display/JGH/GreenHopper) on top of [JIRA](http://www.atlassian.com/software/jira/). However, we wanted to keep the whiteboard for ambient information and as a location for morning scrum. Greenhopper doesn’t support a way of printing 3x5 cards so what follows is a very convoluted set of instructions that have gotten up 80% of the way there.

#### Greasemonkey

Greenhopper has a way of printing story cards but the only available layout is a two column setup that doen’t handle page breaks nicely causing cards to be cut between pages. The following greasemonkey script will force a page break after each card.

%[https://gist.github.com/965ccc8cf76f23b01f6f71dabadd83b9]

Include this script on the following url pattern:

[http://jira.mycompany.com/secure/Print.jspa\*](http://jira.locamoda.com/secure/Print.jspa*)

#### Printing in Firefox

Ideally you could print directly from firefox to the 3x5 card but with my setup (FF 3.6 on OS X 10.6) Firefox wont accept 3x5 as a valid page size. To get around this create a 4.5 x 7.5 paper size by going to File -> Page Setup and select Manage Custom Sizes… under Paper Size. With that paper size selected, navigate to the Planning Board view in Greenhopper and click the printer icon next to your current sprint. Turn off all the headers and footers and click preview, not print.

#### Printing in Preview

In preview you are able to create a 3x5 paper size. Choose Print from the File menu and you will see a Paper Size drop down in the print dialog. Select Manage Custom Sizes…, just like in the Page Setup dialog in Firefox, and create a paper size of 3x5. With that paper size selected click the scale radio button and scale up to 125%. Finally, load your printer with 3x5 cards and click print. You should end up with prints that fill most of a 3x5 card.