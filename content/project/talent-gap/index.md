---
title: Talent Gap Analysis
summary: Economic Development Woes — Part 1.
# tags: 
# date: "2016-04-27T00:00:00Z"

# Optional external URL for project (replaces project detail page).
external_link: ""

image:
  caption: Truck Driver Completions and Postings near Ohio
  focal_point: Smart

links:
#- icon: external-link-alt
#  icon_pack: fas
#  name: Udacity Course
#  url: https://www.udacity.com/course/intro-to-machine-learning--ud120
url_code: ""
url_pdf: ""
url_slides: ""
url_video: ""

# Slides (optional).
#   Associate this project with Markdown slides.
#   Simply enter your slide deck's filename without extension.
#   E.g. `slides = "example-slides"` references `content/slides/example-slides.md`.
#   Otherwise, set `slides = ""`.
slides: ""
---

## Problem

After my first few months with a regional economic development (ED) organization, it was clear that the issue facing those in the ED world was far larger than just "landing the next big business." In order to make that happen, there are numerous other factors in play: sites and buildings, utilities, incentives, and workforce.

Workforce is an issue that seems to plague both business retention as well as business expansion. A business is not going to set up shop in an area where they will not be able to attract a full workforce. In addition, it is difficult for smaller businesses to expand, or sometimes even stay afloat, if their workforce is insufficient.

## Investigation

After hearing about workforce being a significant issue for many of the businesses within my region, I decided to take a closer look into where the gap may be. My work gave me access to [Emsi](https://www.economicmodeling.com/), a Labor Market Analytics service. That coupled with a few downloads from the [National Center for Education Statistics (NCES)](https://nces.ed.gov/) and I was ready for some investigation.

A few steps through the NCES website allows users to grab completion data for all accredited post-secondary institutions in the United States. It's a little hefty, but allows a "database" of sorts to be set up locally using Excel or, in my case, Python. If you load that CSV into a dataframe in a Jupyter notebook, you can do endless SQL-like queries to do some great investigation into what programs are available nearby and how many graduates are produced. *And* if you add in a crosswalk between education programs and occupations, the exploration gets even more fun. You can then explore what kind of jobs for which you are over- and under-supplied. For economic development, this helps you figure out which companies would be good targets because your region will be able to supply the workforce. On the other hand, it can also tell you the areas you may need to talk to local institutions about to help start a talent pipeline to attract a different set of businesses.

For this particular analysis, I wanted to include a demand figure to compare with the supply from the NCES data. I used job postings data from Emsi in this case. They've already done the site-scrubbing and data-cleaning that will save oodles of time on my end. Number one on the list was Heavy and Tractor-Trailer Truck Drivers. As I look down the list I see similar items: Light Truck Drivers, Supervisors of Material Moving Workers, Driver/Sales Workers, etc. The high demand over time makes me curious as to whether there is a gap in supply for my area.

## Visualizing the Data

ArcGIS Pro is an incredible tool for geospatial analysis. It allows joining of spreadsheets to location layers which allowed the image at the top of this post to be created. Mapping the job postings show that there is a massive need for truck drivers in our eastern-most county. That one county blows all others out of the water.
{{% alert note %}}
As a note, I do know that Jefferson County is a bit of an outlier for economic development. Within the past year [JSW Steel](https://wtov9.com/news/local/mingo-junction-celebrates-jsw-steel-after-years-without-industry) has moved in and there have been on-going talks about [an ethane cracker nearby](https://www.heraldstaronline.com/news/local-news/2019/07/jobsohio-awards-30m-grant-for-preparation-of-proposed-cracker-site/). Not to mention the fact that the shale underlying that region is part of the [most productive in the nation](https://www.eia.gov/todayinenergy/detail.php?id=33512).
{{% /alert %}}

Overall, the posts header image shows that there are institutions in neighboring areas that are producing far more truck drivers than we are. Since each of those areas have major metropolitan areas, they're going to have far more job opportunities period. It will be difficult to pull a worker from that market as their rate of pay is typically higher. In order to combat this particular issue, our employers will either need to focus on attracting talent from the areas producing it, or we need to work with local educational institutions within the region to offer programs to produce truck drivers closer to home. In any case, this gives our team some further background into what is happening for this particular occupation as well as a process for us to investigate other talent pipelines in the region.
