# Exploring mental health incidents in New Zealand Police Data

## Introduction
In 2021, RNZ reported that the Police commissioner Andrew Coster expressed wanting a new approach to addressing the call-outs related to mental health issues. Based on their annual police report at the time, about half of all mental health-related call-outs were left unattended by the police. They also admit that the police are not the best responders to those kinds of call-outs. Despite this, they expect the volume of calls to increase in the near future, even up to 44% by 2025. (Scotcher, 2021)

While police call-outs alone provide an incomplete picture of the situation of mental health in the country, it may lead to insights on the overt expression of distress, and potential underlying social factors that could be mitigated to reduce the occurrence of these call-outs. To explore potential solutions to this, it would be important to understand the situation through the following questions:

1. Has there been an increase in the proportion of mental health-related activities since 2021?
2. Is there a way to see overt expressions of mental health distress manifested in police records? If so, what are the top categories?
3. Is there a difference in how mental health incidents occur between city and non-city areas?

To understand reports in the context of mental health. We will first explore the trend of mental health reports over time split by whether or not it was correctly reported as a mental health issue. Explore the correlation between crime types and the occurrence of mental health reports.

## Methods
From the Demand and Activity reports, we have access to records with key variables such as location, day of week, time of day, reported occurrence, and recorded occurrence. (New Zealand Police, 2023) However, the report contains limited demographic data, which limits the insights that can be derived from this paper. Another limitation of the report is the lack of context as to what 'Mental Health' means as an occurrence subgroup; however, for the purpose of this report, it is interpreted as overt expressions of mental health distress or occurrences that could be traced back to it.  There is also a lack of what action the police have taken to resolve the incident.

The primary dataset includes incidents involving the occurrence subgroup 'Mental Health'. This means the data sets to download are from both Reported and Recorded occurrence sets. Much of the data is categorical, with the number of occurrences being the only numerical variable. This means much of the representations in this report will be represented by proportions and frequencies per category, as we drill down on dimensions relevant to the questions.

Because there are occurrences recorded as 'Mental Health' that were reported as something else and vice versa, we create a dataframe for each category. `misreported` is created for reports misreported as 'Mental Health' and later on recorded as a different category. `redirected` is for reports under other subgroups that are redirected as 'Mental Health'. Meanwhile, `true_mh` is for reports reported as 'Mental Health' and also recorded as 'Mental Health'. These are then combined into a master dataframe `mh_incidents` which will be used to subset in succeeding analyses.

Within the `mh_incidents` dataframe, year and month data is reformatted to the standard r date format for uniformity. This allows us to join statistics of different groups on a common month-year value. An additional column `iscity` is created to indicate a city or non-city call-out based on the Police Area column. The column value is derived from finding the word "City" or "Auckland" in the Police Area column. The use of Auckland came after an initial trial wherein the Police District contained City districts (Auckland City, Manukau, and Waitemata).

To examine the increase in mental health reports over time, we will examine the situation for each month. We will use the proportions of mental health incidents by getting the total mental health incidents for that month out of all police reports for that period. That monthly proportion is then plotted against the total reports per month. This presents the changes in proportion over time more clearly than a stacked bar chart, as the share of mental health incidents will be too small to see. 

The source provides a csv of the count of all incident types aggregated per month. Downloading this file as opposed to the entire dataset allows for more lean operations. To match this dataset on the side of 'Mental Health' incidents, we create a table from the month of year column and store it as a dataframe. These two are then joined by date, and a new variable `rate_mentalhealth` is calculated as the proportion.

## Results and Discussion
### *Has there been an increase in the proportion of mental health-related activities since 2021?*
Based on Figure 1, the monthly proportion of mental health incidents relative to all reports is at most 1.54%. From May 2020 to March 2023, there is an increased level of proportions, especially after 2021. We can combine these monthly proportions into yearly proportions to observe the year-on-year changes.

<p align = 'center'> Figure 1: Combination plot of monthly proportion of Mental Health reports vs total monthly reports </p>

![Fig 1](https://github.com/christianllave/nz-police-data/assets/70957302/dced4da2-ffe6-4583-854c-f14ee636a049)

The box plot in Figure 2 also displays this trend. Both 2020 and 2021 are both right-skewed, which has a higher concentration of low-valued proportions for that year. Meanwhile, 2022 to 2023 are left skewed and close to the center respectively.

<p align = 'center'> Figure 2: Box plot of monthly proportions of mental health reports grouped by year </p>

![Fig 2](https://github.com/christianllave/nz-police-data/assets/70957302/05f8540b-f825-48d9-b690-21ac8358f320)

There is an increase in the proportion of mental health incidents in police records as evident in the proportion over time, and the changes in skew over time. This may substantiate the initial expectation of the police commissioner that incidents will continue to rise. 

### *Is there a way to see overt expressions of mental health distress manifested in police records? If so, what are the top categories?*
Based on Table 1, over 56% of mental health occurrences are reported correctly. Over 37% are reported as a different occurrence subgroup and later on, recorded as 'Mental Health'.  

<p align = 'center'> Table 1: Classification of mental health incidents </p>
<div align = 'center'>

| Classification | Occurrences | Relative Frequency |
| ------------- | :-------------: | :-------------: |
| true_mh | 82952 | 0.5673289 |
| redirected | 54992 | 0.3761037 |
| misreported | 8271 | 0.0565674 | 

</div>
According to Table 2, over 40% come from the combined frequencies of social service activities and social service demand, while 18% are from crime and crash.

<p align = 'center'> Table 2: Top 5 Report subgroups from Redirected records </p>
<div align = 'center'>
  
| Reported Subgroup | Occurrences | Relative Frequency |
| ------------- | :-------------: | :-------------: |
| Social/Service Activities | 16706 | 0.3037896 |
| Crime and Crash Demand | 10389 | 0.1889184 |
| Social/Service Demand | 7625 | 0.1386565 |
| Other Demand | 6600 | 0.1200175 |
| Public Order Offences | 3997 | 0.0726833 |

</div>

From Figure 3, we can see that out of the social service activities and demand, over 57% involve harm or require care. Meanwhile, for crime and crash demand, we can see over 73% involve suspicious behavior, and 21% involve breaches of peace. 

<p align = 'center'> Figure 3: Bar charts of top report subgroups redirected to a Mental Health record </p>
  
![Fig 3](https://github.com/christianllave/nz-police-data/assets/70957302/d3c90d62-bdd4-4523-9721-efbf1ff74700)

From police records, there were occurrences of mental health distress that were most often initially reported as either a crime, crash, or social service demand. If we were to look into reducing mental health incidents in the scope of police work, it would be good to look at these types of incidents and design better social support systems that may be put in place.

### *Is there a difference in how mental health incidents occur between city and non-city areas?*
From Figure 4, we can see a difference in the scale of occurrences, but a similar shape of the overarching trend for the two contexts; however, there are differences in the hourly trends depending on the day of the week. For example, between 11 AM to 6 PM on Friday, there are fluctuations in how similar the trends are between the two contexts. These fluctuations then occur earlier on Sundays. There was also a distinct peak on Tuesdays in non-city contexts that was not present in city contexts. 

<p align = 'center'> Figure 4: Line charts of Mental Health incidents by time of day for City and Non-City occurrences </p>

![Fig 4](https://github.com/christianllave/nz-police-data/assets/70957302/c3edf5bc-c953-4f30-b070-66ac34666813)

Based on Figure 5, there are mostly similar proportions in terms of the split between true mental health incidents, redirections, and misreports between city and non-city contexts. This could mean there are similar levels of awareness in terms of requesting help for mental distress. The big difference between the two is that the city context is more concentrated around specific areas; meanwhile, the non-city context is more spread out in lower volumes, which limits insights that could be derived from mere visual observation. For future studies, it would be good to compare these proportions for significant differences.

<p align = 'center'> Figure 5: Bar charts of top report subgroups redirected to a Mental Health record </p>

![Fig 5](https://github.com/christianllave/nz-police-data/assets/70957302/d233de76-ab30-4424-859c-b8cb1ab30f46)

## Summary
Based on our findings, there has been an increase in the proportion of mental health incidents in police records. We also saw a portion of mental health incidents that were reported as different overt manifestations of mental distress and found that they were mostly reported as social/service demand and activity, and crime. We then noticed similarities in the proportion of misclassified reports between city and non-city contexts. On the topic of city and non-city contexts, we noticed differences in the trends over the course of the day depending on the day of the week. Despite the limitations in the available dataset, there are signs of potential further analyses that could produce more definitive insights for actions that the government could take to reduce mental health incidents and refocus the function of the police to allow for more specialized support to those experiencing mental distress. 

## References
\[1\] New Zealand Police. (2023, May 12). Demand and activity. https://www.police.govt.nz/about-us/statistics-and-publications/data-and-statistics/demand-and-activity

\[2\] Scotcher, K. (2021, November 19). As police call-outs for mental health issues rise, the commissioner wants a new approach. RNZ. https://www.rnz.co.nz/news/national/456062/as-police-call-outs-for-mental-health-issues-rise-the-commissioner-wants-a-new-approach

\[3\] Scotcher, K. (2021, November 18). Report states half of all mental health-related callouts in the past year went unattended by police. NZ Herald. https://www.nzherald.co.nz/nz/report-states-half-of-all-mental-health-related-callouts-in-the-past-year-went-unattended-by-police/DOLVP67YCE5HXHXKUO3CR2C7WY/
