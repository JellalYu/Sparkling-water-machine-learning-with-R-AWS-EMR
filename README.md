# Sparkling water machine learning with R in AWS EMR


## About this lab
### Senario 

When researchers train machine learning or deep learning models, huge amounts of data and multilayer algorithms make the model training slow.

AWS EMR services combine with Spark based memory processing architecture that allows users to do this work in a very short time. 

In addition, H2o is a scalable machine learning framework developed by h2o.ai and supports many APIs (such as R, python, Scala and Java, or spark) in statistical languages.

Sparkling water is a machine learning framework that is a combination of H2o and Spark, allowing users to use H2o flow UI training and is very suitable for users who are first in contact with machine learning.

This experiment mainly lead you to use the R studio in AWS EMR services and Sparkling water to train several machine learning models and to present the results by R flexdachboard package.


###

## Prerequisites

>Make sure your are in US East (N. Virginia), which short name is us-east-1.

>Download Putty and PuTTYgen: IF you don’t already have the **PuTTy client/PuTTYgen** installed on your machine, you can download and then launch it from here:
https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html


###


INSTALL RSTUDIO SERVER
# Update
sudo yum update
sudo yum install libcurl-devel openssl-devel # used for devtools

# Install RStudio Server
wget -P /tmp https://s3.amazonaws.com/rstudio-dailybuilds/rstudio-server-rhel-0.99.1266-x86_64.rpm
sudo yum install --nogpgcheck /tmp/rstudio-server-rhel-0.99.1266-x86_64.rpm

# Make User
sudo useradd -m rstudio-user
sudo passwd rstudio-user

# Permission
export HADOOP_USER_NAME=yarn

# Create new directory in hdfs
hadoop fs -mkdir /user/rstudio-user
hadoop fs -chmod 777 /user/rstudio-user

Load data from github
# Make download directory
mkdir /tmp/wineQualityWhites

wget -O /tmp/wineQualityWhites.csv https://minhaskamal.github.io/DownGit/#/home?url=https://github.com/JellalYu/Sparkling-water-machine-learning-with-R-AWS-EMR/blob/master/wineQualityWhites.csv

DISTRIBUTE INTO HDFS
# Copy data to HDFS
hadoop fs -mkdir /user/rstudio-user/wineQualityWhites/
hadoop fs -put /tmp/wineQualityWhites /user/rstudio-user/

Create Hive tables
# Open Hive prompt
hive

# Create metadata for airlines
CREATE EXTERNAL TABLE IF NOT EXISTS wineQualityWhites
(
fixed.acidity	int
volatile.acidity	int
citric.acid		int
residual.sugar	int
chlorides		int
free.sulfur.dioxide	int
total.sulfur.dioxide	int
density		int
pH		int
sulphates		int
alcohol		int
quality		int
)
ROW FORMAT DELIMITED
FIELDS TERMINATED BY ','
LINES TERMINATED BY '\n'
TBLPROPERTIES("skip.header.line.count"="1");

# Load data into table
LOAD DATA INPATH '/user/rstudio-user/wineQualityWhites ' INTO TABLE wineQualityWhites;

 

R
pack<-c("sparklyr","readr","knitr","tidyr","ggplot2","ggrepel","dplyr","jsonlite","h2o","rsparkling")
ipak <- function(pkg){
  new.pkg <- pkg[!(pkg %in% installed.packages()[, "Package"])]
  if (length(new.pkg)) 
    install.packages(new.pkg, dependencies = TRUE)
  sapply(pkg, require, character.only = TRUE)
}
ipak(pack);lapply(pack, require, character.only = TRUE)







## Reference

http://docs.h2o.ai/h2o/latest-stable/h2o-docs/data-science/stacked-ensembles.html#defining-an-h2o-stacked-ensemble-model

http://docs.h2o.ai/h2o/latest-stable/h2o-docs/save-and-load-model.html
