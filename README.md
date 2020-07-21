## Introduction

  Every year I take a trip with my boyfriend and his family to Pensacola Beach in the heat of the Summer. Soak up some sun and enjoy everyones company. The past few years I have gone with, I find my self asking when the tiny and thriving beach city will cease to exist due to sea level rise. As I was taking a Summer class this year, I decided to answer the question that has been brewing for my final data science lifecycle project, in CMSC 320 Intro to Data Science at University of Maryland, and expand upon it. 

  I am going to take you through the data science lifecycle and my steps to answering the question of what year Pensacola, and later other cities, will begin to flood due purely to the rise in sea level.

### Data Collection

  The first step for any data science problem is determining the data you need and gathering it. Because I want to know when a landmass will flood due to rises in the oceans, I looked for sea level measurements over the past years. I was able to find data collected by four satillites, TPOEX/POSIDEON, Jason-1, Jason-2, and Jason-3, funded by the National Oceanic and Atmospheric Administration the National Oceanic and Atmospheric Administration (NOAA) containing data from 1992 to 2020. There are two different sea level datasets available at their website, WWW.WHEREIGOTMYDATA.COM. One set has seasonal indicators included, things such as el nino's and la nina's where the sea levels rises in waves based on what occured the previous year.
  
  When answering the question of unexpected sea level rise, we want to look only at the unusual increase over time. Included seasonal trends in my data would produce inaccurate and skewed results. Due to this, I decided to use the other dataset NOAA has collected, the seasonal affects removed data. This data is seperated into csv files of each ocean based on the picture below. Each file has data for how many millimeters the ocean rose at the time of observement. 

```markdown
Syntax highlighted code block

# Header 1
## Header 2
### Hi

- Bulleted
- List

1. Numbered
2. List

**Bold** and _Italic_ and `Code` text

[Link](url) and ![Image](src)
```

For more details see [GitHub Flavored Markdown](https://guides.github.com/features/mastering-markdown/).

### Jekyll Themes

Your Pages site will use the layout and styles from the Jekyll theme you have selected in your [repository settings](https://github.com/kellydyer007/kellydyer007.github.io/settings). The name of this theme is saved in the Jekyll `_config.yml` configuration file.

### Support or Contact

Having trouble with Pages? Check out our [documentation](https://help.github.com/categories/github-pages-basics/) or [contact support](https://github.com/contact) and we’ll help you sort it out.
