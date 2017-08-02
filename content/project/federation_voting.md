+++
# Date this page was created.
date = "2017-06-28"

# Project title.
title = "Who Voted for Federation?"

# Project summary to display on homepage.
summary = "Understanding who voted for the Federation of Australia, and why the outcome of several critical elections changed, allows us to better appreciate why voters decided to add a level of government when only a couple of generations earlier they insisted on political independence from Britain."

# Optional image to display on homepage (relative to `static/img/` folder).
image_preview = "_DSC4116.jpg"

# Tags: can be used for filtering projects.
# Example: `tags = ["machine-learning", "deep-learning"]`
tags = ["current"]

# Optional external URL for project (replaces project detail page).
external_link = ""

# Does the project detail page use math formatting?
math = false

# Optional featured image (relative to `static/img/` folder).
[header]
image = "_DSC4116.jpg"
#caption = "A galah :bird:"

+++

The Federation of Australia culminated on 1 January 1901 when six former British colonies created the Commonwealth of Australia. A hundred years later, advertising at the time of the Centenary of Federation made much of the fact that Australia was formed as an outcome of a vote not a war. But it took many elections for this federation to occur. Understanding who voted for the Federation of Australia, and why the outcome of several critical elections changed, allows us to better appreciate why voters decided to add a level of government when only a couple of generations earlier they insisted on political independence from Britain. 

In this note we: 

1. introduce the datasets that exist to examine these events;
2. detail the steps that have been taken to tidy these datasets so that usual statistical analysis can be conducted; and 
3. describe the current and future tasks.

This note complements the note written by Tim Hatton that provides more detail about the background and specifics of the Federation elections. A paper will bring these notes together along with statistical analysis that is yet-to-completed. As far as we have been able to tell, there has been little analysis of the reasons for Federation from a quantitative perspective and we are able to do this because of our new datasets.

Current/next steps are:

1. Continue tidying the elections data, which means both improving the geocodes of the booths and assigning those boths to census districts.
2. Obtain and tidy required census data.
3. Bring these datasets together and conduct analysis.