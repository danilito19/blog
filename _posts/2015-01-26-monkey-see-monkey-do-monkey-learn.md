---
layout: post
title: "Monkey see, monkey do, monkey learn"
modified: 2015-01-26 08:47:38 -0600
tags: [statistics, data, learning, R, healthcare, government data, data science, big data]
image:
  feature: 
  credit: 
  creditlink: 
comments: true
share: 
---
More than reading textbooks or watching videos, I learn by looking at someone else's project, running through the code, and imitating his/her steps to learn about the analysis. Writing this post also pushed me to understand the code; otherwise, I wouldn't be able to write about it.

I came across [Vik Paruchuri's blog](http://www.vikparuchuri.com/) and was happy to learn about someone who also began a career in the social sciences, someone who - as Vik claims - was afraid of math, but who learned how to use computer science tools and analytical thinking to work on socially-oriented projects. I thought - since Vik came from a similar educational background, then maybe he would have examples that would interest me.

I took Vik's analysis of a huge Medicare dataset from the Center for Medicare and Medicaid Services. This [dataset](http://www.cms.gov/Research-Statistics-Data-and-Systems/Statistics-Trends-and-Reports/Medicare-Provider-Charge-Data/Physician-and-Other-Supplier.html) provides information about 880,644 healthcare providers across the country who provided 5,949 unique medical services to Medicare beneficiaries. From this dataset, we learn for the first time how much healthcare providers are paid for the services they render to Medicare beneficiaries.

###Convert, understand, and load the Medicare data

Vik's post was not for absolute R beginners. For example, he assumed that I would have enough RAM to be able to open the data. He challenged me in this respect. After downloading the tab delimited txt file, I attempted to create an SQLite database that I could use to read from R - but this proved too tedius. Then, I Googled around to see what others do to work with big data in R. I came across the ff package and decided to use that. Finally, I used a bit of Python to transform the data from txt to csv, just in case the ff package gave me problems with txt files.

{% highlight Python %}
import csv

txt_file = r"mytxtfile.txt" #name of your input txt
csv_file = r"mycsvfile.csv" #name of your output csv
in_txt = csv.reader(open(txt_file, "rU"), delimiter = '\t')
out_csv = csv.writer(open(csv_file, 'w'))
out_csv.writerows(in_txt)
{% endhighlight %}

[Here's](https://www.cms.gov/Research-Statistics-Data-and-Systems/Statistics-Trends-and-Reports/Medicare-Provider-Charge-Data/Downloads/Medicare-Physician-and-Other-Supplier-PUF-Methodology.pdf) an explanation of the data variables, but these are the important fields:

* npi, the National Provider Identifier, is a unique physician ID
* hcpcs_code is a unique ID for a service the doctor performs
* line_srvc_cnt indicates the count of a specific service the doctor performed 
* average_Medicare_allowed_amt is the sum average of what Medicare pays and what the beneficiary and/or a third party is responsible for paying (averaged for each service)
* average_submitted_chrg_amt is the average charge the physician submitted for the specific service
* average_Medicare_payment_amt is the average amount Medicare paid the doctor for the service after deductible and coinsurance

And here is how I started working with the file in R:


{% highlight R %}
library(ff)
setwd('/Users/myDirectory')

#enabling VERBOSE allows you to see how many rows it has uploaded
data <- read.csv.ffdf(file="MedicareLARGE.csv", header=TRUE, VERBOSE=TRUE, first.rows=10000, next.rows=50000, colClasses=NA)
head(data, n = 2)
#my second row has a copyright remark, so I created a new dataframe to eliminate it. You don't have to create a new frame - it's just a habit.
data2 <- data[-1,]

#disable scipen so I don't see exponential values
options(scipen=999)
{% endhighlight %}

Depending on your RAM, this could take a while to load and to view, but I always like to give the data a quick glance. Notice that the submitted charge amount seems to often be greater than the Medicare allowed amount. Is this another example of doctors inflating service prices to get more out of insurance? I've had some experience with this, but this is just first-look speculation and something I may not be able to investigate through this dataset.

##Aggregate data by npi 

Notice that the data does not have a primary key. We have all 9 million rows of data, with multiple rows representing the same doctor who performed different services. We want to aggregate the data so each row represents one doctor. Let's use the data.table library for that, which extends the capabilities of core R's data.frame with speed of many features (so good for big data sets). Use this [cheat sheet](https://s3.amazonaws.com/assets.datacamp.com/img/blog/data+table+cheat+sheet.pdf) for data.table.


{% highlight R %}
library(data.table)
data2 = data.table(data2)
phys_summ = data2[
  ,
  list(
    code = nppes_entity_code,
    service_total=sum(line_srvc_cnt),
    ben_total=sum(bene_unique_cnt),
    M_payment_to_dr=sum(average_Medicare_payment_amt * line_srvc_cnt),
    dr_charged=sum(average_submitted_chrg_amt * line_srvc_cnt),
    allowed=sum(average_Medicare_allowed_amt * line_srvc_cnt),
    unique_services_per_patient=sum(bene_day_srvc_cnt)/sum(bene_unique_cnt),
    duplicates_per_service=sum(line_srvc_cnt)/sum(bene_day_srvc_cnt),
    services_per_patient=sum(line_srvc_cnt)/sum(bene_unique_cnt)
  ),
  by="npi"
  ]
{% endhighlight %}

##Learning from the data

Vik first analyzed income inequality among doctors. That is, we want to see what percent of doctors makes what percent of total income. To do this, we have to filter by doctors-only because the data includes organizations (labs, hospitals). Follow Vik's [post](http://www.vikparuchuri.com/blog/exploring-us-healthcare-data/) to see how he analyzed this. As he mentions, there is evident income inequality in this data, but we cannot make this a generalization for all doctors. Remember, this is Medicare data - some doctors do not bill Medicare much. Vik also analyzes gender-based inequality by medical occupation, shows a map of where the top-payed doctors are, and looks at the correlation between state life expectancy and Medicare spending by ethnic group.

##Challenges

I got to learn a lot from Vik's post through many challenges I faced. For example, when Vik showed how to aggregate the data, he did not include the nppes_entity_code, which is the code that indicates if the npi is a doctor or an organization. Later, when he filtered doctors out, he referred to the phys_summ data, but mine did not include the nppes_entity_code. I had to figure out a way to include it. It seems that R didn't allow me to create phys_summ BY npi and then BY nppes_entity_code, so I looked for a way around it. I recognized that nppes_entity_code had 7 rows with "." as the code, so I filtered these seven rows out of our data. I re-created phys_summ without the seven rows, and then I created a new "code" column in phys_summ with an ifelse statement. 

{% highlight R %}
#create new column named code, if the nppes_entity_code was
#I, give that row I, else give it O
phys_summ$code = ifelse(data3$nppes_entity_code == "I", "I", "O")
{% endhighlight %}

I'm sure there is a better way to do this - let me know if you think so. Regardless if the method, at least now I could work with the doctor-only data! If I wanted to mock Vik's gender-based analysis, I also would've had to do something about including the physician type and gender in phys_summ. 

##What I learned:
* Working with big data in R - the ff package and data.table
* Aggregation in R
* Cool insights about Medicare

##What I need to learn:
* d3 to create beautiful visualizations like Vik's
* Aggregation by two factors

