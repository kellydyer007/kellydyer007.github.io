Created by Kelly Dyer for CMSC 320 Summer 1 2020 Professor: Kyungjin Yoo

An analysis of sea level numbers obtained from satillites to determine the year when specified cities will begin to flood.

Note: The uploaded slr files are the individual csv files for each ocean I added to my data frame

## Introduction

  Every year I take a trip with my boyfriend and his family to Pensacola Beach in the heat of the Summer. The past few years I have gone with, I find my self asking when the tiny and thriving beach city will cease to exist due to sea level rise. As I was taking a Summer class this year, I decided to answer the question that has been brewing in my mind for my final data science lifecycle project, for CMSC 320 Intro to Data Science at University of Maryland, and expand upon it. 

  I am going to take you through the data science lifecycle and my steps to answering the question of what year Pensacola beach, and later other cities, will begin to flood due purely to the rise in sea level.

### Data Collection

  The first step for any data science problem is determining the data you need and gathering it. Because I want to know when a landmass will flood due to rises in the oceans, I looked for sea level measurements over the past years. I was able to find data collected by four satillites, TPOEX/POSIDEON, Jason-1, Jason-2, and Jason-3, funded by the National Oceanic and Atmospheric Administration the National Oceanic and Atmospheric Administration (NOAA) containing data from 1992 to 2020. There are two different sea level datasets available at their website, https://www.star.nesdis.noaa.gov/socd/lsa/SeaLevelRise/LSA_SLR_timeseries.php. One set has seasonal indicators included, things such as el nino's and la nina's where the sea levels rises in waves based on what occured the previous year, and the other with these signals removed.
  
  When answering the question of unexpected sea level rise, we want to look only at the unusual increase over time. Including seasonal trends in my data would produce inaccurate and skewed results. Due to this, I decided to use the other dataset NOAA has collected, the seasonal affects removed data. This data is seperated into csv files of each ocean based on the picture below. Each file has data for how many millimeters the ocean rose at the time of observement. 

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
```
My dataframe is filled with everything needed and looks as follows:
![image](https://user-images.githubusercontent.com/66328517/88014651-7491e600-caed-11ea-89d6-baf3462519bc.png)

### Data Tidying
  As you can see in my dataframe, the year variable contains a decimal point that represents when the measurement was made. The decimal is not easy on the eyes and hard to understand when beginning to look at the data. So, I decided to seperate out the year into quarters based on this decimal point. This seperates the year into quarters similar to the fiscal year, [0,.25)=Q1 [.25,.5)=Q2 [.5,.75)=Q3 [.75,1)=Q4. Creating a new variable made grouping and understanding the year eaiser. Other than the year, the data was relatively clean and easy to understand. Below you can see the code I used to create the quarters variable and the final dataframe after tidying. 
```markdown
def quarter(y):
    flo=math.floor(y)
    if y-flo<.25:
        return "Q1"
    elif y-flo<.5:
        return "Q2"
    elif y-flo<.75:
        return "Q3"
    else:
        return "Q4"

for index, row in groups.iterrows():
    groups.at[int(index),'quart']=quarter(row['year'])
    ye=str(math.floor(row['year']))
    groups.at[int(index),'yearID']= ye+" "+str(row['quart'])
```
![image](https://user-images.githubusercontent.com/66328517/88097322-f887b480-cb65-11ea-84d1-2ba106fd1969.png)

### Data Processing
  Now that we have all our data looking pretty and ready to analyze, I created some regression lines superimposed over the actual data graph for the Gulf of Mexico and global trends. 
  ![image](https://user-images.githubusercontent.com/66328517/88097178-c8401600-cb65-11ea-9d46-3b9c5caf1d56.png)
  
  As you can see from the graph, the Gulf of Mexico (red dashed line) has a higher slope than the Atlantic ocean (green dashed line). Why is this the case when the oceans are connected? After doing some research, the difference is due to how oceans stores heat. The regional differences occur because of the variability of winds and ocean currents that affect how the oceans store heat. This article explains the regional differences well, https://www.climate.gov/news-features/understanding-climate/climate-change-global-sea-level. 

Now that we understand why regional differences happen, we can use the regression line to predict how far sea level will rise over a period of time. To do this, we need to take the integral of whatever ocean we are looking at. In relation to pensacola, we look at the Gulf of Mexico. I took the integral up until the year 3000 to see the trend over a long period of time. When our integration line intercepts the horizontal flood threshold line, the city in question will be flooded or begin to flood, depending on where the sea level measurement was taken in the city. 

  The Scipy integration method returns a list of points, so to determine an estimated yeat I needed to find the index with data closest to the flooding parameter and add it to 2021, the current year. I used this piece of code to find the predicted year based on the integration line:
  ```markdown
  #finds the intersection of the two lines
close=min(Y, key=lambda x:abs(x-5000))
print(Y.index(close)+2021)
  ```
  Based on the trends in sea level rise, if they continue in a linear fashion, Pensacola Beach will be flooded in the year 2058. This is only 37 years away, much sooner than I anticipated.
  
## Expanding the scope to multiple cities
  Now that we have estimated one city, we can use the same techniques to predict when other costal cities are going to be underwater. I choose some cities around the world that would be impacted by different oceans to see how that factors into the predicted flooding year. I picked 12 costal cities that will be displayed in the data frame below. This new data frame contains the city name, distance from current sea level in meters and millimeters, ocean the city is located on, data from our integration line, and the year cities will flood. The data for distance from current sea level was obtained from https://elevation.maplogs.com/poi/ for each city manually.
![image](https://user-images.githubusercontent.com/66328517/88105498-ffb4bf80-cb71-11ea-835e-e4928ca8402f.png)

### Exploration and Data visualization
  The first plot of interest is the plot of integration lines, located in the data column, for each city together in one graph to see how the slope of each line affects the flooding rate. 
  
  ![image](https://user-images.githubusercontent.com/66328517/88105776-7782ea00-cb72-11ea-8d65-64912b479851.png)
  
As you can see in this plot, the pink line representing Shanghai China, which is influenced by the China Sea, will flood faster than any of the other oceans, while the green line representing London, influenced by the North Sea, will flood the slowest over a long period of time. Looking far out into the future of these cities gives insight on which mainland is going to flood faster. So the mainland of China will flood faster than the mainland of, say, Australia. 
  We can use the integration lines similar to as we did in our Pensacola example and find the intersection of the integration with the flood parameter of each city. The next graph I created shows the predicted year of flooding plotted with the total sea level increase needed.
  
  ![image](https://user-images.githubusercontent.com/66328517/88106449-80c08680-cb73-11ea-8029-35580c580748.png)

This plot is the culmination plot of everything I have calculated. You can see the increase in the influencing ocean clearly when looking at Shanghai and London, as well as Pensacola and Mumbai. Looking at Shanghai and London first, you can see that Shanghai, 13 meters above sea level, will flood before London, 11 meters above sea level. Even though Shanghai is 2 meters above London, the heavy slope on the China Sea will impact the flooding year. Based on my predictions, Shanghai will flood 2 years earlier than London. Even though this flooding does not occure until 2088, you are still able to see the impact of the influencing ocean. This is most likely due to the China Sea being largly influenced by global warming, but this is not in the scope for this project.
  You are also able to see the impact when looking at Pensacola and Mumbai. They are both located 5 meters above sea level, yet Pensacola floods faster than Mumbai. Even though the integration lines do not significantly show an increase between eachother until 2150, the ocean a city is located on depends on the rate of flooding, no matter how small the increase may be. 
  
  ### Machine Learning
  In addition to useing future predicted regression lines to calculate flooding year, I used the K-means clustering algorithm to cluster my data. You can see the output of my clustering below. 
  
  ![image](https://user-images.githubusercontent.com/66328517/88107644-77381e00-cb75-11ea-996b-69cc68b551d2.png)

The clustering grouped together cities with similar flooding year and distance from sea level. Clustering may be more informative if I had a larger dataset of cities to see which oceans landed in the same clusters or if they are all spread. From my clustering with limited data points, you can see that the individual oceans are spead across all clusters. Many cities I picked do not share the same influenced ocean, so the conclusions I can draw from clustering are minimal.

## Conclusion

### Insight
If the sea level's continues to progress how it is currently, in a linear fashion, sea level rise will become a huge issue for many costal cities as they will start to loose ports and land mass, as well as displace citizens who live near the coast. This will cause strain on the local governments and unhappy individuals who will loose their house and recreational parks and beaches. 
  Many of the first settlers who decided to settle costal cities did this for ease of trade and not having to move all their supplies further inland. Also, many of these people came by boat so it was benifical to settle right where they landed. They did not realize this at the time, but settling so close to the ocean may just be some cities downfall due to the rapid rise in sea level. This issue will persist for many years, but, if the world comes together and address issues related to global warming and melting ice caps, we may be able to slow down the rate at which the sea levels are rising and prepare for the inevitible. 
  
### Ice Melt Relation?

![image](https://user-images.githubusercontent.com/66328517/88109493-9a180180-cb78-11ea-9820-c1b74d60c7f5.png)

  When I was first looking at the mean sea level graph by year, I noticed a huge spike around the year 2016/2017, as seen in the graph above. I wanted to investigate to see what caused this huge spike. This article explained this peak very well: https://www.nytimes.com/interactive/2017/06/09/climate/antarctica-rift-update.html. This large peak was caused by part of an ice shelf, located in the pacific ocean, breaking off. When large chunks of ice break and slide into the ocean that were above sea level, the sea level rises. For many costal cities, the rise of sea level over time will be an issue. But, for many inland cities and even some costal cities, these large peaks will be their demise. It may be sudden surges and waves that persist throught the year that more inland cities will have to deal with and learn to control. Overall, wether a city is located on the cost or further inland, they will all have to learn to combat or live with the rise in sea level. Many people with homes next to the ocean will be displaced and need to find a safer place to live.
  
