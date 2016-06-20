---
layout: post
title:  "Folium and the Partisan Voting Index"
date:   2016-06-18 0:10:00 -0500
---
This is the first in a series of at least two posts that serve the following
three purposes:

1. I want to learn how to use Python data analysis tools, in the interest of
becoming less of a monolingual programmer who only knows web dev.
2. I want to learn to use mapping tools that are more powerful than CartoDB in
certain respects.
3. I want to learn how state partisan voting patterns change over time.

#### What are we looking at?

[introduce topic here]

The Cook Partisan Voting Index (Cook PVI) is a measure of the political lean of
a US state or congressional district – i.e., its degree of "red," "blue," or
"swing" behavior. The index is calculated by comparing the Democratic or
Republican candidate's share of the two-party vote in that state with their
national share of the two-party vote. For example, if a Democratic candidate
wins 51% of the vote in Ohio and 53% of the vote nationally, Ohio has a PVI of
R+2. Although the Democrat won, Ohio was still more Republican than the country
as a whole.

The Cook PVI is calculated by averaging the results of the last two presidential
elections. That's not useful to us, so we'll be looking at the PVI for individual
elections. Someone at Daily Kos has created a [spreadsheet of state-by-state results and PVIs for every presidential election from 1828 to 2012](http://www.dailykos.com/story/2014/8/20/1323254/-New-Spreadsheet-with-1828-2012-Presidential-Results-PVI-by-State-Neatly-Colored).
I extracted the PVIs and put them in [another spreadsheet](https://docs.google.com/spreadsheets/d/1aHoBXaxyg6nlLO9odKhlVAGaNA_24iKGKFnIiSZ09PA/edit).
This will be our dataset. (Take a look – it's interesting.)
