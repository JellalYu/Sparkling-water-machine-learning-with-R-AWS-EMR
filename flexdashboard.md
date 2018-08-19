```yaml
---
title: "Whites wine quality"
output: 
  flexdashboard::flex_dashboard:
    orientation: columns
    vertical_layout: fill
---

\```{r setup, include=FALSE}

load('C:/Users/ECV/Desktop/Wine')

library(sparklyr)
library(knitr)
library(tidyr)
library(ggplot2)
library(ggrepel)
library(dplyr)
library(jsonlite)
library(h2o)
library(kableExtra)

Sys.setenv(SPARK_HOME='C:/Users/ECV/AppData/Local/spark/spark-2.1.0-bin-hadoop2.7')
sc <- spark_connect(master = "yarn-client", version = "2.1.0")
Wine_tbl <- copy_to(sc, Wine)
\```
Variables
=======================================================================
Row {.tabset}
-----------------------------------------------------------------------

### residual_sugar

\```{r}
Wine_tbl %>% 
  ggplot(aes(x=quality,y=residual_sugar,fill=quality))+
  geom_boxplot()+#fill=c('red','green','yellow','yellow','yellow','yellow','yellow'),alpha=0.2)+
  theme(legend.position="none")+
  scale_fill_brewer(palette="BuPu")
\```

### pH

\```{r}
Wine_tbl %>% 
  ggplot(aes(x=quality,y=pH,fill=quality))+
  geom_boxplot()+#fill=c('red','green','yellow','yellow','yellow','yellow','yellow'),alpha=0.2)+
  theme(legend.position="none")+
  scale_fill_brewer(palette="BuPu")
\```

### alcohol

\```{r}
Wine_tbl %>% 
  ggplot(aes(x=quality,y=alcohol,fill=quality))+
  geom_boxplot()+#fill=c('red','green','yellow','yellow','yellow','yellow','yellow'),alpha=0.2)+
  theme(legend.position="none")+
  scale_fill_brewer(palette="BuPu")
\```

### sulphates

\```{r}
Wine_tbl %>% 
  ggplot(aes(x=quality,y=sulphates,fill=quality))+
  geom_boxplot()+#fill=c('red','green','yellow','yellow','yellow','yellow','yellow'),alpha=0.2)+
  theme(legend.position="none")+
  scale_fill_brewer(palette="BuPu")
\```

### fixed_acidity

\```{r}
Wine_tbl %>% 
  ggplot(aes(x=quality,y=fixed_acidity,fill=quality))+
  geom_boxplot()+#fill=c('red','green','yellow','yellow','yellow','yellow','yellow'),alpha=0.2)+
  theme(legend.position="none")+
  scale_fill_brewer(palette="BuPu")
\```

### volatile_acidity

\```{r}
Wine_tbl %>% 
  ggplot(aes(x=quality,y=volatile_acidity,fill=quality))+
  geom_boxplot()+#fill=c('red','green','yellow','yellow','yellow','yellow','yellow'),alpha=0.2)+
  theme(legend.position="none")+
  scale_fill_brewer(palette="BuPu")
\```

### citric_acid

\```{r}
Wine_tbl %>% 
  ggplot(aes(x=quality,y=citric_acid,fill=quality))+
  geom_boxplot()+#fill=c('red','green','yellow','yellow','yellow','yellow','yellow'),alpha=0.2)+
  theme(legend.position="none")+
  scale_fill_brewer(palette="BuPu")
\```

### chlorides

\```{r}
Wine_tbl %>% 
  ggplot(aes(x=quality,y=chlorides,fill=quality))+
  geom_boxplot()+#fill=c('red','green','yellow','yellow','yellow','yellow','yellow'),alpha=0.2)+
  theme(legend.position="none")+
  scale_fill_brewer(palette="BuPu")
\```

### free_sulfur_dioxide

\```{r}
Wine_tbl %>% 
  ggplot(aes(x=quality,y=free_sulfur_dioxide,fill=quality))+
  geom_boxplot()+#fill=c('red','green','yellow','yellow','yellow','yellow','yellow'),alpha=0.2)+
  theme(legend.position="none")+
  scale_fill_brewer(palette="BuPu")
\```

Machine Learning Result
=======================================================================
Row{.tabset}
-----------------------------------------------------------------------

### Random Forests
    
\```{r  fig.height = 6, fig.width = 6.5}
#par(mfrow=c(1,2))
h2o.varimp_plot(rf_model)
plot(rf_model, timestep = "number_of_trees", metric = "rmse")

\```
   
### GBM

\```{r  fig.height = 6, fig.width = 6.5}
#par(mfrow=c(1,2))
h2o.varimp_plot(gbm_model)
plot(gbm_model, timestep = "number_of_trees", metric = "rmse")

\```

### DeepLearning

\```{r  fig.height = 6, fig.width = 6.5}

plot(dl_fit, timestep = "epochs", metric = "rmse")

\```

Model Comparison
=======================================================================
Column
-----------------------------------------------------------------------
\```{r}
ggplot(data = mComp) + 
  geom_bar(mapping = aes(x = model, y=value, fill = factor(variable) ), position = "dodge",stat = "identity") +
  ggtitle('Position = "dodge"')
\```

Column
-----------------------------------------------------------------------
 ```{r  fig.height = 6, fig.width = 9.5}
kable(Comp)%>%
  kable_styling()%>%
  scroll_box(height = "300px",width = "600px")

\```

```
