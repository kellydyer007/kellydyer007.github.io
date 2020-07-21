## Introduction

  Every year I take a trip with my boyfriend and his family to Pensacola Beach in the heat of the Summer. Soak up some sun and enjoy everyones company. The past few years I have gone with, I find my self asking when the tiny and thriving beach city will cease to exist due to sea level rise. As I was taking a Summer class this year, I decided to answer the question that has been brewing for my final data science lifecycle project, in CMSC 320 Intro to Data Science at University of Maryland, and expand upon it. 

  I am going to take you through the data science lifecycle and my steps to answering the question of what year Pensacola, and later other cities, will begin to flood due purely to the rise in sea level.

### Data Collection

  The first step for any data science problem is determining the data you need and gathering it. Because I want to know when a landmass will flood due to rises in the oceans, I looked for sea level measurements over the past years. I was able to find data collected by four satillites, TPOEX/POSIDEON, Jason-1, Jason-2, and Jason-3, funded by the National Oceanic and Atmospheric Administration the National Oceanic and Atmospheric Administration (NOAA) containing data from 1992 to 2020. There are two different sea level datasets available at their website, https://www.star.nesdis.noaa.gov/socd/lsa/SeaLevelRise/LSA_SLR_timeseries.php. One set has seasonal indicators included, things such as el nino's and la nina's where the sea levels rises in waves based on what occured the previous year.
  
  When answering the question of unexpected sea level rise, we want to look only at the unusual increase over time. Included seasonal trends in my data would produce inaccurate and skewed results. Due to this, I decided to use the other dataset NOAA has collected, the seasonal affects removed data. This data is seperated into csv files of each ocean based on the picture below. Each file has data for how many millimeters the ocean rose at the time of observement. 

![globe](https://user-images.githubusercontent.com/66328517/88014096-13b5de00-caec-11ea-8ce0-b342623ddbee.png)

To calculate when Pensacola will flood, we need to look at the Gulf of mexico data. I used pandas to read in the Gulf csv file along with other oceans I would need for a later analysis (Global trends, Atlantic, Pacific, Sea of Japan, Carribean, Indonesia, North Atlantic, North Pacific, and South China Sea). I repeated this piece of code for each csv file to get everything in a data frame matched with the year the data was obtained. 
```markdown
#collects all data and merges it into one dataframe
data=pd.read_csv("slr_sla_gbl_free_txj1j2_90.csv", sep=',',skiprows=5)
data.fillna(0,inplace=True)
data['global']=data['TOPEX/Poseidon']+data['Jason-1']+data['Jason-2']+data['Jason-3']
data.drop(['TOPEX/Poseidon','Jason-1','Jason-2','Jason-3'], axis=1, inplace=True)
data['year']=round(data['year'],2)
#repeated for every ocean of interest
atl_data=pd.read_csv("slr_sla_atl_free_txj1j2_90.csv", sep=',',skiprows=5)
atl_data.fillna(0,inplace=True)
data['atlantic']=atl_data['TOPEX/Poseidon']+atl_data['Jason-1']+atl_data['Jason-2']+atl_data['Jason-3']


# Header 1
## Header 2
### Hi

- Bulleted
- List

1. Numbered
2. List

**Bold** and _Italic_ and `Code` text

```
My dataframe is filled with everything needed and looks as follows:
![image](https://user-images.githubusercontent.com/66328517/88014651-7491e600-caed-11ea-89d6-baf3462519bc.png)

### Data Tidying
  As you can see in my dataframe, the year variable contains a decimal point that represents when the measurement was made. The decimal is not easy on the eyes and hard to understand when beginning to look at the data. So, I decided to seperate out the year into quarters based on this decimal point. This seperates the year into quarters similar to the fiscal year, [0,.25)=Q1 [.25,.5)=Q2 [.5,.75)=Q3 [.75,1)=Q4.] Creating a new variable made grouping and understanding the year eaiser. 

### Jekyll Themes

Your Pages site will use the layout and styles from the Jekyll theme you have selected in your [repository settings](https://github.com/kellydyer007/kellydyer007.github.io/settings). The name of this theme is saved in the Jekyll `_config.yml` configuration file.

### Support or Contact

Having trouble with Pages? Check out our [documentation](https://help.github.com/categories/github-pages-basics/) or [contact support](https://github.com/contact) and we’ll help you sort it out.
