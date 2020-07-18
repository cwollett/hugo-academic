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

I was able to grab enough NCES data to be able to look at all completions at any post-secondary institution within the United States in the past year. These were further broken down by program "code" and award level. One of the headaches when working with workforce data, though, is educational programs are given different code names than occupations. To put this into context: your major in college likely will not be the same as your job title once you hit the job market. Thankfully, there are crosswalks to try to alleviate this issue. That will come in handy later.

Before I start digging around the NCES data, I head over to Emsi. They have a report that allows me to look at the number of job postings in a region. They've already done the site-scrubbing and data-cleaning on numerous job board website, not to mention then grouping the data by occupation code. All in all, this will save oodles of time on my end. I pull down a report of all job postings in my region, exclude things like retail and restaurants, and sort it in descending order. Number one was Heavy and Tractor-Trailer Truck Drivers. As I look down the list I see similar items: Light Truck Drivers, Supervisors of Material Moving Workers, Driver/Sales Workers, etc. Clearly this truck driving occupation has some gaps in our area.

Now that I have an idea of what is demanded I want to know if our region is currently capable of *supplying* those workers. If not, there would be data to take to training centers to work on developing new programs in order to close the gap. But first: the NCES data. Python, pandas especially, makes this a breeze. I create a list of the occupation codes corresponding to truck drivers, create a corresponding list of program codes in Python using the crosswalk mentioned earlier, and create a table view listing the institutions that have awarded credentials in those programs in the past year.

## Visualizing the Result
ArcGIS Pro is an incredible tool for geospatial analysis. It allows joining of spreadsheets to location layers which allowed the image at the top of this post to be created. 
Mapping the job postings show that there is a massive need for truck drivers in our eastern-most county. That one county blows all others out of the water. Though, I do know that Jefferson County is currently a huge area for economic development. Within the past year [JSW Steel](https://wtov9.com/news/local/mingo-junction-celebrates-jsw-steel-after-years-without-industry) has moved in and there have been on-going talks about [an ethane cracker nearby](https://www.heraldstaronline.com/news/local-news/2019/07/jobsohio-awards-30m-grant-for-preparation-of-proposed-cracker-site/). Not to mention the fact that the shale underlying that region is part of the [most productive in the nation](https://www.eia.gov/todayinenergy/detail.php?id=33512).

Overall, the image above shows that there are institutions in neighboring areas that are producing far more truck drivers than we are. Since each of those areas have major metropolitan areas, they're going to have far more job opportunities period. It will be difficult to pull a worker from that market as their rate of pay is typically higher. In order to combat this particular issue, our employers will either need to focus on attracting talent from the areas producing it, or we need to work with local educational institutions within the region to offer programs to produce truck drivers closer to home.
