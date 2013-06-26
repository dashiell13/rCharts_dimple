---
title: rCharts, dimple, and time series
subtitle: Some Early Experiments
author: Timely Portfolio
github: {user: timelyportfolio, repo: rCharts_dimple, branch: "gh-pages"}
framework: bootstrap
mode: selfcontained
ext_widgets: {rCharts: "libraries/dimple"}
highlighter: prettify
hitheme: twitter-bootstrap
---



```r
require(quantmod)
require(rCharts)

xtsMelt <- function(data) {
  require(reshape2)
  
  #translate xts to time series to json with date and data
  #for this behavior will be more generic than the original
  #data will not be transformed, so template.rmd will be changed to reflect
  
  
  #convert to data frame
  data.df <- data.frame(cbind(format(index(data),"%Y-%m-%d"),coredata(data)))
  colnames(data.df)[1] = "date"
  data.melt <- melt(data.df,id.vars=1,stringsAsFactors=FALSE)
  colnames(data.melt) <- c("date","indexname","value")
  #remove periods from indexnames to prevent javascript confusion
  #these . usually come from spaces in the colnames when melted
  data.melt[,"indexname"] <- apply(matrix(data.melt[,"indexname"]),2,gsub,pattern="[.]",replacement="")
  return(data.melt)
  #return(df2json(na.omit(data.melt)))
}


#now get the US bonds from FRED
USbondssymbols <- paste0("DGS",c(1,2,3,5,7,10,20,30))

ust.xts <- xts()
for (i in 1:length( USbondssymbols ) ) {
  ust.xts <- merge( 
    ust.xts,
    getSymbols( 
      USbondssymbols[i], auto.assign = FALSE,src = "FRED"
    )
  )
}

ust.melt <- na.omit( xtsMelt( ust.xts["2012::",] ) )

ust.melt$date <- format(as.Date(ust.melt$date), '%m/%d/%Y')
ust.melt$value <- as.numeric(ust.melt$value)
ust.melt$indexname <- factor(
  ust.melt$indexname, levels = colnames(ust.xts)
)
ust.melt$maturity <- as.numeric(
  substr(
    ust.melt$indexname, 4, length( ust.melt$indexname ) - 4
  )
)
ust.melt$country <- rep( "US", nrow( ust.melt ))

#simple line chart of 10 year
d1 <- dPlot(
  value ~ date,
  data = subset(ust.melt, maturity == 10),
  type = 'line'
)
d1$print('chart1')
```


<div id='chart1' class='rChart dimple'></div>
<script type="text/javascript">
  var opts = {
 "dom": "chart1",
"width":    800,
"height":    400,
"x": "date",
"y": "value",
"type": "line",
"id": "chart1" 
},
    data = [
 {
 "date": "01/02/2012",
"indexname": "DGS10",
"value":   1.97,
"maturity":     10,
"country": "US" 
},
{
 "date": "01/03/2012",
"indexname": "DGS10",
"value":      2,
"maturity":     10,
"country": "US" 
},
{
 "date": "01/04/2012",
"indexname": "DGS10",
"value":   2.02,
"maturity":     10,
"country": "US" 
},
{
 "date": "01/05/2012",
"indexname": "DGS10",
"value":   1.98,
"maturity":     10,
"country": "US" 
},
{
 "date": "01/08/2012",
"indexname": "DGS10",
"value":   1.98,
"maturity":     10,
"country": "US" 
},
{
 "date": "01/09/2012",
"indexname": "DGS10",
"value":      2,
"maturity":     10,
"country": "US" 
},
{
 "date": "01/10/2012",
"indexname": "DGS10",
"value":   1.93,
"maturity":     10,
"country": "US" 
},
{
 "date": "01/11/2012",
"indexname": "DGS10",
"value":   1.94,
"maturity":     10,
"country": "US" 
},
{
 "date": "01/12/2012",
"indexname": "DGS10",
"value":   1.89,
"maturity":     10,
"country": "US" 
},
{
 "date": "01/16/2012",
"indexname": "DGS10",
"value":   1.87,
"maturity":     10,
"country": "US" 
},
{
 "date": "01/17/2012",
"indexname": "DGS10",
"value":   1.92,
"maturity":     10,
"country": "US" 
},
{
 "date": "01/18/2012",
"indexname": "DGS10",
"value":   2.01,
"maturity":     10,
"country": "US" 
},
{
 "date": "01/19/2012",
"indexname": "DGS10",
"value":   2.05,
"maturity":     10,
"country": "US" 
},
{
 "date": "01/22/2012",
"indexname": "DGS10",
"value":   2.09,
"maturity":     10,
"country": "US" 
},
{
 "date": "01/23/2012",
"indexname": "DGS10",
"value":   2.08,
"maturity":     10,
"country": "US" 
},
{
 "date": "01/24/2012",
"indexname": "DGS10",
"value":   2.01,
"maturity":     10,
"country": "US" 
},
{
 "date": "01/25/2012",
"indexname": "DGS10",
"value":   1.96,
"maturity":     10,
"country": "US" 
},
{
 "date": "01/26/2012",
"indexname": "DGS10",
"value":   1.93,
"maturity":     10,
"country": "US" 
},
{
 "date": "01/29/2012",
"indexname": "DGS10",
"value":   1.87,
"maturity":     10,
"country": "US" 
},
{
 "date": "01/30/2012",
"indexname": "DGS10",
"value":   1.83,
"maturity":     10,
"country": "US" 
},
{
 "date": "01/31/2012",
"indexname": "DGS10",
"value":   1.87,
"maturity":     10,
"country": "US" 
},
{
 "date": "02/01/2012",
"indexname": "DGS10",
"value":   1.86,
"maturity":     10,
"country": "US" 
},
{
 "date": "02/02/2012",
"indexname": "DGS10",
"value":   1.97,
"maturity":     10,
"country": "US" 
},
{
 "date": "02/05/2012",
"indexname": "DGS10",
"value":   1.93,
"maturity":     10,
"country": "US" 
},
{
 "date": "02/06/2012",
"indexname": "DGS10",
"value":      2,
"maturity":     10,
"country": "US" 
},
{
 "date": "02/07/2012",
"indexname": "DGS10",
"value":   2.01,
"maturity":     10,
"country": "US" 
},
{
 "date": "02/08/2012",
"indexname": "DGS10",
"value":   2.04,
"maturity":     10,
"country": "US" 
},
{
 "date": "02/09/2012",
"indexname": "DGS10",
"value":   1.96,
"maturity":     10,
"country": "US" 
},
{
 "date": "02/12/2012",
"indexname": "DGS10",
"value":   1.99,
"maturity":     10,
"country": "US" 
},
{
 "date": "02/13/2012",
"indexname": "DGS10",
"value":   1.92,
"maturity":     10,
"country": "US" 
},
{
 "date": "02/14/2012",
"indexname": "DGS10",
"value":   1.93,
"maturity":     10,
"country": "US" 
},
{
 "date": "02/15/2012",
"indexname": "DGS10",
"value":   1.99,
"maturity":     10,
"country": "US" 
},
{
 "date": "02/16/2012",
"indexname": "DGS10",
"value":   2.01,
"maturity":     10,
"country": "US" 
},
{
 "date": "02/20/2012",
"indexname": "DGS10",
"value":   2.05,
"maturity":     10,
"country": "US" 
},
{
 "date": "02/21/2012",
"indexname": "DGS10",
"value":   2.01,
"maturity":     10,
"country": "US" 
},
{
 "date": "02/22/2012",
"indexname": "DGS10",
"value":   1.99,
"maturity":     10,
"country": "US" 
},
{
 "date": "02/23/2012",
"indexname": "DGS10",
"value":   1.98,
"maturity":     10,
"country": "US" 
},
{
 "date": "02/26/2012",
"indexname": "DGS10",
"value":   1.92,
"maturity":     10,
"country": "US" 
},
{
 "date": "02/27/2012",
"indexname": "DGS10",
"value":   1.94,
"maturity":     10,
"country": "US" 
},
{
 "date": "02/28/2012",
"indexname": "DGS10",
"value":   1.98,
"maturity":     10,
"country": "US" 
},
{
 "date": "02/29/2012",
"indexname": "DGS10",
"value":   2.03,
"maturity":     10,
"country": "US" 
},
{
 "date": "03/01/2012",
"indexname": "DGS10",
"value":   1.99,
"maturity":     10,
"country": "US" 
},
{
 "date": "03/04/2012",
"indexname": "DGS10",
"value":      2,
"maturity":     10,
"country": "US" 
},
{
 "date": "03/05/2012",
"indexname": "DGS10",
"value":   1.96,
"maturity":     10,
"country": "US" 
},
{
 "date": "03/06/2012",
"indexname": "DGS10",
"value":   1.98,
"maturity":     10,
"country": "US" 
},
{
 "date": "03/07/2012",
"indexname": "DGS10",
"value":   2.03,
"maturity":     10,
"country": "US" 
},
{
 "date": "03/08/2012",
"indexname": "DGS10",
"value":   2.04,
"maturity":     10,
"country": "US" 
},
{
 "date": "03/11/2012",
"indexname": "DGS10",
"value":   2.04,
"maturity":     10,
"country": "US" 
},
{
 "date": "03/12/2012",
"indexname": "DGS10",
"value":   2.14,
"maturity":     10,
"country": "US" 
},
{
 "date": "03/13/2012",
"indexname": "DGS10",
"value":   2.29,
"maturity":     10,
"country": "US" 
},
{
 "date": "03/14/2012",
"indexname": "DGS10",
"value":   2.29,
"maturity":     10,
"country": "US" 
},
{
 "date": "03/15/2012",
"indexname": "DGS10",
"value":   2.31,
"maturity":     10,
"country": "US" 
},
{
 "date": "03/18/2012",
"indexname": "DGS10",
"value":   2.39,
"maturity":     10,
"country": "US" 
},
{
 "date": "03/19/2012",
"indexname": "DGS10",
"value":   2.38,
"maturity":     10,
"country": "US" 
},
{
 "date": "03/20/2012",
"indexname": "DGS10",
"value":   2.31,
"maturity":     10,
"country": "US" 
},
{
 "date": "03/21/2012",
"indexname": "DGS10",
"value":   2.29,
"maturity":     10,
"country": "US" 
},
{
 "date": "03/22/2012",
"indexname": "DGS10",
"value":   2.25,
"maturity":     10,
"country": "US" 
},
{
 "date": "03/25/2012",
"indexname": "DGS10",
"value":   2.26,
"maturity":     10,
"country": "US" 
},
{
 "date": "03/26/2012",
"indexname": "DGS10",
"value":    2.2,
"maturity":     10,
"country": "US" 
},
{
 "date": "03/27/2012",
"indexname": "DGS10",
"value":   2.21,
"maturity":     10,
"country": "US" 
},
{
 "date": "03/28/2012",
"indexname": "DGS10",
"value":   2.18,
"maturity":     10,
"country": "US" 
},
{
 "date": "03/29/2012",
"indexname": "DGS10",
"value":   2.23,
"maturity":     10,
"country": "US" 
},
{
 "date": "04/01/2012",
"indexname": "DGS10",
"value":   2.22,
"maturity":     10,
"country": "US" 
},
{
 "date": "04/02/2012",
"indexname": "DGS10",
"value":    2.3,
"maturity":     10,
"country": "US" 
},
{
 "date": "04/03/2012",
"indexname": "DGS10",
"value":   2.25,
"maturity":     10,
"country": "US" 
},
{
 "date": "04/04/2012",
"indexname": "DGS10",
"value":   2.19,
"maturity":     10,
"country": "US" 
},
{
 "date": "04/05/2012",
"indexname": "DGS10",
"value":   2.07,
"maturity":     10,
"country": "US" 
},
{
 "date": "04/08/2012",
"indexname": "DGS10",
"value":   2.06,
"maturity":     10,
"country": "US" 
},
{
 "date": "04/09/2012",
"indexname": "DGS10",
"value":   2.01,
"maturity":     10,
"country": "US" 
},
{
 "date": "04/10/2012",
"indexname": "DGS10",
"value":   2.05,
"maturity":     10,
"country": "US" 
},
{
 "date": "04/11/2012",
"indexname": "DGS10",
"value":   2.08,
"maturity":     10,
"country": "US" 
},
{
 "date": "04/12/2012",
"indexname": "DGS10",
"value":   2.02,
"maturity":     10,
"country": "US" 
},
{
 "date": "04/15/2012",
"indexname": "DGS10",
"value":      2,
"maturity":     10,
"country": "US" 
},
{
 "date": "04/16/2012",
"indexname": "DGS10",
"value":   2.03,
"maturity":     10,
"country": "US" 
},
{
 "date": "04/17/2012",
"indexname": "DGS10",
"value":      2,
"maturity":     10,
"country": "US" 
},
{
 "date": "04/18/2012",
"indexname": "DGS10",
"value":   1.98,
"maturity":     10,
"country": "US" 
},
{
 "date": "04/19/2012",
"indexname": "DGS10",
"value":   1.99,
"maturity":     10,
"country": "US" 
},
{
 "date": "04/22/2012",
"indexname": "DGS10",
"value":   1.96,
"maturity":     10,
"country": "US" 
},
{
 "date": "04/23/2012",
"indexname": "DGS10",
"value":      2,
"maturity":     10,
"country": "US" 
},
{
 "date": "04/24/2012",
"indexname": "DGS10",
"value":   2.01,
"maturity":     10,
"country": "US" 
},
{
 "date": "04/25/2012",
"indexname": "DGS10",
"value":   1.98,
"maturity":     10,
"country": "US" 
},
{
 "date": "04/26/2012",
"indexname": "DGS10",
"value":   1.96,
"maturity":     10,
"country": "US" 
},
{
 "date": "04/29/2012",
"indexname": "DGS10",
"value":   1.95,
"maturity":     10,
"country": "US" 
},
{
 "date": "04/30/2012",
"indexname": "DGS10",
"value":   1.98,
"maturity":     10,
"country": "US" 
},
{
 "date": "05/01/2012",
"indexname": "DGS10",
"value":   1.96,
"maturity":     10,
"country": "US" 
},
{
 "date": "05/02/2012",
"indexname": "DGS10",
"value":   1.96,
"maturity":     10,
"country": "US" 
},
{
 "date": "05/03/2012",
"indexname": "DGS10",
"value":   1.91,
"maturity":     10,
"country": "US" 
},
{
 "date": "05/06/2012",
"indexname": "DGS10",
"value":   1.92,
"maturity":     10,
"country": "US" 
},
{
 "date": "05/07/2012",
"indexname": "DGS10",
"value":   1.88,
"maturity":     10,
"country": "US" 
},
{
 "date": "05/08/2012",
"indexname": "DGS10",
"value":   1.87,
"maturity":     10,
"country": "US" 
},
{
 "date": "05/09/2012",
"indexname": "DGS10",
"value":   1.89,
"maturity":     10,
"country": "US" 
},
{
 "date": "05/10/2012",
"indexname": "DGS10",
"value":   1.84,
"maturity":     10,
"country": "US" 
},
{
 "date": "05/13/2012",
"indexname": "DGS10",
"value":   1.78,
"maturity":     10,
"country": "US" 
},
{
 "date": "05/14/2012",
"indexname": "DGS10",
"value":   1.76,
"maturity":     10,
"country": "US" 
},
{
 "date": "05/15/2012",
"indexname": "DGS10",
"value":   1.76,
"maturity":     10,
"country": "US" 
},
{
 "date": "05/16/2012",
"indexname": "DGS10",
"value":    1.7,
"maturity":     10,
"country": "US" 
},
{
 "date": "05/17/2012",
"indexname": "DGS10",
"value":   1.71,
"maturity":     10,
"country": "US" 
},
{
 "date": "05/20/2012",
"indexname": "DGS10",
"value":   1.75,
"maturity":     10,
"country": "US" 
},
{
 "date": "05/21/2012",
"indexname": "DGS10",
"value":   1.79,
"maturity":     10,
"country": "US" 
},
{
 "date": "05/22/2012",
"indexname": "DGS10",
"value":   1.73,
"maturity":     10,
"country": "US" 
},
{
 "date": "05/23/2012",
"indexname": "DGS10",
"value":   1.77,
"maturity":     10,
"country": "US" 
},
{
 "date": "05/24/2012",
"indexname": "DGS10",
"value":   1.75,
"maturity":     10,
"country": "US" 
},
{
 "date": "05/28/2012",
"indexname": "DGS10",
"value":   1.74,
"maturity":     10,
"country": "US" 
},
{
 "date": "05/29/2012",
"indexname": "DGS10",
"value":   1.63,
"maturity":     10,
"country": "US" 
},
{
 "date": "05/30/2012",
"indexname": "DGS10",
"value":   1.59,
"maturity":     10,
"country": "US" 
},
{
 "date": "05/31/2012",
"indexname": "DGS10",
"value":   1.47,
"maturity":     10,
"country": "US" 
},
{
 "date": "06/03/2012",
"indexname": "DGS10",
"value":   1.53,
"maturity":     10,
"country": "US" 
},
{
 "date": "06/04/2012",
"indexname": "DGS10",
"value":   1.57,
"maturity":     10,
"country": "US" 
},
{
 "date": "06/05/2012",
"indexname": "DGS10",
"value":   1.66,
"maturity":     10,
"country": "US" 
},
{
 "date": "06/06/2012",
"indexname": "DGS10",
"value":   1.66,
"maturity":     10,
"country": "US" 
},
{
 "date": "06/07/2012",
"indexname": "DGS10",
"value":   1.65,
"maturity":     10,
"country": "US" 
},
{
 "date": "06/10/2012",
"indexname": "DGS10",
"value":    1.6,
"maturity":     10,
"country": "US" 
},
{
 "date": "06/11/2012",
"indexname": "DGS10",
"value":   1.67,
"maturity":     10,
"country": "US" 
},
{
 "date": "06/12/2012",
"indexname": "DGS10",
"value":   1.61,
"maturity":     10,
"country": "US" 
},
{
 "date": "06/13/2012",
"indexname": "DGS10",
"value":   1.64,
"maturity":     10,
"country": "US" 
},
{
 "date": "06/14/2012",
"indexname": "DGS10",
"value":    1.6,
"maturity":     10,
"country": "US" 
},
{
 "date": "06/17/2012",
"indexname": "DGS10",
"value":   1.59,
"maturity":     10,
"country": "US" 
},
{
 "date": "06/18/2012",
"indexname": "DGS10",
"value":   1.64,
"maturity":     10,
"country": "US" 
},
{
 "date": "06/19/2012",
"indexname": "DGS10",
"value":   1.65,
"maturity":     10,
"country": "US" 
},
{
 "date": "06/20/2012",
"indexname": "DGS10",
"value":   1.63,
"maturity":     10,
"country": "US" 
},
{
 "date": "06/21/2012",
"indexname": "DGS10",
"value":   1.69,
"maturity":     10,
"country": "US" 
},
{
 "date": "06/24/2012",
"indexname": "DGS10",
"value":   1.63,
"maturity":     10,
"country": "US" 
},
{
 "date": "06/25/2012",
"indexname": "DGS10",
"value":   1.66,
"maturity":     10,
"country": "US" 
},
{
 "date": "06/26/2012",
"indexname": "DGS10",
"value":   1.65,
"maturity":     10,
"country": "US" 
},
{
 "date": "06/27/2012",
"indexname": "DGS10",
"value":    1.6,
"maturity":     10,
"country": "US" 
},
{
 "date": "06/28/2012",
"indexname": "DGS10",
"value":   1.67,
"maturity":     10,
"country": "US" 
},
{
 "date": "07/01/2012",
"indexname": "DGS10",
"value":   1.61,
"maturity":     10,
"country": "US" 
},
{
 "date": "07/02/2012",
"indexname": "DGS10",
"value":   1.65,
"maturity":     10,
"country": "US" 
},
{
 "date": "07/04/2012",
"indexname": "DGS10",
"value":   1.62,
"maturity":     10,
"country": "US" 
},
{
 "date": "07/05/2012",
"indexname": "DGS10",
"value":   1.57,
"maturity":     10,
"country": "US" 
},
{
 "date": "07/08/2012",
"indexname": "DGS10",
"value":   1.53,
"maturity":     10,
"country": "US" 
},
{
 "date": "07/09/2012",
"indexname": "DGS10",
"value":   1.53,
"maturity":     10,
"country": "US" 
},
{
 "date": "07/10/2012",
"indexname": "DGS10",
"value":   1.54,
"maturity":     10,
"country": "US" 
},
{
 "date": "07/11/2012",
"indexname": "DGS10",
"value":    1.5,
"maturity":     10,
"country": "US" 
},
{
 "date": "07/12/2012",
"indexname": "DGS10",
"value":   1.52,
"maturity":     10,
"country": "US" 
},
{
 "date": "07/15/2012",
"indexname": "DGS10",
"value":    1.5,
"maturity":     10,
"country": "US" 
},
{
 "date": "07/16/2012",
"indexname": "DGS10",
"value":   1.53,
"maturity":     10,
"country": "US" 
},
{
 "date": "07/17/2012",
"indexname": "DGS10",
"value":   1.52,
"maturity":     10,
"country": "US" 
},
{
 "date": "07/18/2012",
"indexname": "DGS10",
"value":   1.54,
"maturity":     10,
"country": "US" 
},
{
 "date": "07/19/2012",
"indexname": "DGS10",
"value":   1.49,
"maturity":     10,
"country": "US" 
},
{
 "date": "07/22/2012",
"indexname": "DGS10",
"value":   1.47,
"maturity":     10,
"country": "US" 
},
{
 "date": "07/23/2012",
"indexname": "DGS10",
"value":   1.44,
"maturity":     10,
"country": "US" 
},
{
 "date": "07/24/2012",
"indexname": "DGS10",
"value":   1.43,
"maturity":     10,
"country": "US" 
},
{
 "date": "07/25/2012",
"indexname": "DGS10",
"value":   1.45,
"maturity":     10,
"country": "US" 
},
{
 "date": "07/26/2012",
"indexname": "DGS10",
"value":   1.58,
"maturity":     10,
"country": "US" 
},
{
 "date": "07/29/2012",
"indexname": "DGS10",
"value":   1.53,
"maturity":     10,
"country": "US" 
},
{
 "date": "07/30/2012",
"indexname": "DGS10",
"value":   1.51,
"maturity":     10,
"country": "US" 
},
{
 "date": "07/31/2012",
"indexname": "DGS10",
"value":   1.56,
"maturity":     10,
"country": "US" 
},
{
 "date": "08/01/2012",
"indexname": "DGS10",
"value":   1.51,
"maturity":     10,
"country": "US" 
},
{
 "date": "08/02/2012",
"indexname": "DGS10",
"value":    1.6,
"maturity":     10,
"country": "US" 
},
{
 "date": "08/05/2012",
"indexname": "DGS10",
"value":   1.59,
"maturity":     10,
"country": "US" 
},
{
 "date": "08/06/2012",
"indexname": "DGS10",
"value":   1.66,
"maturity":     10,
"country": "US" 
},
{
 "date": "08/07/2012",
"indexname": "DGS10",
"value":   1.68,
"maturity":     10,
"country": "US" 
},
{
 "date": "08/08/2012",
"indexname": "DGS10",
"value":   1.69,
"maturity":     10,
"country": "US" 
},
{
 "date": "08/09/2012",
"indexname": "DGS10",
"value":   1.65,
"maturity":     10,
"country": "US" 
},
{
 "date": "08/12/2012",
"indexname": "DGS10",
"value":   1.65,
"maturity":     10,
"country": "US" 
},
{
 "date": "08/13/2012",
"indexname": "DGS10",
"value":   1.73,
"maturity":     10,
"country": "US" 
},
{
 "date": "08/14/2012",
"indexname": "DGS10",
"value":    1.8,
"maturity":     10,
"country": "US" 
},
{
 "date": "08/15/2012",
"indexname": "DGS10",
"value":   1.83,
"maturity":     10,
"country": "US" 
},
{
 "date": "08/16/2012",
"indexname": "DGS10",
"value":   1.81,
"maturity":     10,
"country": "US" 
},
{
 "date": "08/19/2012",
"indexname": "DGS10",
"value":   1.82,
"maturity":     10,
"country": "US" 
},
{
 "date": "08/20/2012",
"indexname": "DGS10",
"value":    1.8,
"maturity":     10,
"country": "US" 
},
{
 "date": "08/21/2012",
"indexname": "DGS10",
"value":   1.71,
"maturity":     10,
"country": "US" 
},
{
 "date": "08/22/2012",
"indexname": "DGS10",
"value":   1.68,
"maturity":     10,
"country": "US" 
},
{
 "date": "08/23/2012",
"indexname": "DGS10",
"value":   1.68,
"maturity":     10,
"country": "US" 
},
{
 "date": "08/26/2012",
"indexname": "DGS10",
"value":   1.65,
"maturity":     10,
"country": "US" 
},
{
 "date": "08/27/2012",
"indexname": "DGS10",
"value":   1.64,
"maturity":     10,
"country": "US" 
},
{
 "date": "08/28/2012",
"indexname": "DGS10",
"value":   1.66,
"maturity":     10,
"country": "US" 
},
{
 "date": "08/29/2012",
"indexname": "DGS10",
"value":   1.63,
"maturity":     10,
"country": "US" 
},
{
 "date": "08/30/2012",
"indexname": "DGS10",
"value":   1.57,
"maturity":     10,
"country": "US" 
},
{
 "date": "09/03/2012",
"indexname": "DGS10",
"value":   1.59,
"maturity":     10,
"country": "US" 
},
{
 "date": "09/04/2012",
"indexname": "DGS10",
"value":    1.6,
"maturity":     10,
"country": "US" 
},
{
 "date": "09/05/2012",
"indexname": "DGS10",
"value":   1.68,
"maturity":     10,
"country": "US" 
},
{
 "date": "09/06/2012",
"indexname": "DGS10",
"value":   1.67,
"maturity":     10,
"country": "US" 
},
{
 "date": "09/09/2012",
"indexname": "DGS10",
"value":   1.68,
"maturity":     10,
"country": "US" 
},
{
 "date": "09/10/2012",
"indexname": "DGS10",
"value":    1.7,
"maturity":     10,
"country": "US" 
},
{
 "date": "09/11/2012",
"indexname": "DGS10",
"value":   1.77,
"maturity":     10,
"country": "US" 
},
{
 "date": "09/12/2012",
"indexname": "DGS10",
"value":   1.75,
"maturity":     10,
"country": "US" 
},
{
 "date": "09/13/2012",
"indexname": "DGS10",
"value":   1.88,
"maturity":     10,
"country": "US" 
},
{
 "date": "09/16/2012",
"indexname": "DGS10",
"value":   1.85,
"maturity":     10,
"country": "US" 
},
{
 "date": "09/17/2012",
"indexname": "DGS10",
"value":   1.82,
"maturity":     10,
"country": "US" 
},
{
 "date": "09/18/2012",
"indexname": "DGS10",
"value":   1.79,
"maturity":     10,
"country": "US" 
},
{
 "date": "09/19/2012",
"indexname": "DGS10",
"value":    1.8,
"maturity":     10,
"country": "US" 
},
{
 "date": "09/20/2012",
"indexname": "DGS10",
"value":   1.77,
"maturity":     10,
"country": "US" 
},
{
 "date": "09/23/2012",
"indexname": "DGS10",
"value":   1.74,
"maturity":     10,
"country": "US" 
},
{
 "date": "09/24/2012",
"indexname": "DGS10",
"value":    1.7,
"maturity":     10,
"country": "US" 
},
{
 "date": "09/25/2012",
"indexname": "DGS10",
"value":   1.64,
"maturity":     10,
"country": "US" 
},
{
 "date": "09/26/2012",
"indexname": "DGS10",
"value":   1.66,
"maturity":     10,
"country": "US" 
},
{
 "date": "09/27/2012",
"indexname": "DGS10",
"value":   1.65,
"maturity":     10,
"country": "US" 
},
{
 "date": "09/30/2012",
"indexname": "DGS10",
"value":   1.64,
"maturity":     10,
"country": "US" 
},
{
 "date": "10/01/2012",
"indexname": "DGS10",
"value":   1.64,
"maturity":     10,
"country": "US" 
},
{
 "date": "10/02/2012",
"indexname": "DGS10",
"value":   1.64,
"maturity":     10,
"country": "US" 
},
{
 "date": "10/03/2012",
"indexname": "DGS10",
"value":    1.7,
"maturity":     10,
"country": "US" 
},
{
 "date": "10/04/2012",
"indexname": "DGS10",
"value":   1.75,
"maturity":     10,
"country": "US" 
},
{
 "date": "10/08/2012",
"indexname": "DGS10",
"value":   1.74,
"maturity":     10,
"country": "US" 
},
{
 "date": "10/09/2012",
"indexname": "DGS10",
"value":   1.72,
"maturity":     10,
"country": "US" 
},
{
 "date": "10/10/2012",
"indexname": "DGS10",
"value":    1.7,
"maturity":     10,
"country": "US" 
},
{
 "date": "10/11/2012",
"indexname": "DGS10",
"value":   1.69,
"maturity":     10,
"country": "US" 
},
{
 "date": "10/14/2012",
"indexname": "DGS10",
"value":    1.7,
"maturity":     10,
"country": "US" 
},
{
 "date": "10/15/2012",
"indexname": "DGS10",
"value":   1.75,
"maturity":     10,
"country": "US" 
},
{
 "date": "10/16/2012",
"indexname": "DGS10",
"value":   1.83,
"maturity":     10,
"country": "US" 
},
{
 "date": "10/17/2012",
"indexname": "DGS10",
"value":   1.86,
"maturity":     10,
"country": "US" 
},
{
 "date": "10/18/2012",
"indexname": "DGS10",
"value":   1.79,
"maturity":     10,
"country": "US" 
},
{
 "date": "10/21/2012",
"indexname": "DGS10",
"value":   1.83,
"maturity":     10,
"country": "US" 
},
{
 "date": "10/22/2012",
"indexname": "DGS10",
"value":   1.79,
"maturity":     10,
"country": "US" 
},
{
 "date": "10/23/2012",
"indexname": "DGS10",
"value":    1.8,
"maturity":     10,
"country": "US" 
},
{
 "date": "10/24/2012",
"indexname": "DGS10",
"value":   1.86,
"maturity":     10,
"country": "US" 
},
{
 "date": "10/25/2012",
"indexname": "DGS10",
"value":   1.78,
"maturity":     10,
"country": "US" 
},
{
 "date": "10/28/2012",
"indexname": "DGS10",
"value":   1.74,
"maturity":     10,
"country": "US" 
},
{
 "date": "10/30/2012",
"indexname": "DGS10",
"value":   1.72,
"maturity":     10,
"country": "US" 
},
{
 "date": "10/31/2012",
"indexname": "DGS10",
"value":   1.75,
"maturity":     10,
"country": "US" 
},
{
 "date": "11/01/2012",
"indexname": "DGS10",
"value":   1.75,
"maturity":     10,
"country": "US" 
},
{
 "date": "11/04/2012",
"indexname": "DGS10",
"value":   1.72,
"maturity":     10,
"country": "US" 
},
{
 "date": "11/05/2012",
"indexname": "DGS10",
"value":   1.78,
"maturity":     10,
"country": "US" 
},
{
 "date": "11/06/2012",
"indexname": "DGS10",
"value":   1.68,
"maturity":     10,
"country": "US" 
},
{
 "date": "11/07/2012",
"indexname": "DGS10",
"value":   1.62,
"maturity":     10,
"country": "US" 
},
{
 "date": "11/08/2012",
"indexname": "DGS10",
"value":   1.61,
"maturity":     10,
"country": "US" 
},
{
 "date": "11/12/2012",
"indexname": "DGS10",
"value":   1.59,
"maturity":     10,
"country": "US" 
},
{
 "date": "11/13/2012",
"indexname": "DGS10",
"value":   1.59,
"maturity":     10,
"country": "US" 
},
{
 "date": "11/14/2012",
"indexname": "DGS10",
"value":   1.58,
"maturity":     10,
"country": "US" 
},
{
 "date": "11/15/2012",
"indexname": "DGS10",
"value":   1.58,
"maturity":     10,
"country": "US" 
},
{
 "date": "11/18/2012",
"indexname": "DGS10",
"value":   1.61,
"maturity":     10,
"country": "US" 
},
{
 "date": "11/19/2012",
"indexname": "DGS10",
"value":   1.66,
"maturity":     10,
"country": "US" 
},
{
 "date": "11/20/2012",
"indexname": "DGS10",
"value":   1.69,
"maturity":     10,
"country": "US" 
},
{
 "date": "11/22/2012",
"indexname": "DGS10",
"value":    1.7,
"maturity":     10,
"country": "US" 
},
{
 "date": "11/25/2012",
"indexname": "DGS10",
"value":   1.66,
"maturity":     10,
"country": "US" 
},
{
 "date": "11/26/2012",
"indexname": "DGS10",
"value":   1.64,
"maturity":     10,
"country": "US" 
},
{
 "date": "11/27/2012",
"indexname": "DGS10",
"value":   1.63,
"maturity":     10,
"country": "US" 
},
{
 "date": "11/28/2012",
"indexname": "DGS10",
"value":   1.62,
"maturity":     10,
"country": "US" 
},
{
 "date": "11/29/2012",
"indexname": "DGS10",
"value":   1.62,
"maturity":     10,
"country": "US" 
},
{
 "date": "12/02/2012",
"indexname": "DGS10",
"value":   1.63,
"maturity":     10,
"country": "US" 
},
{
 "date": "12/03/2012",
"indexname": "DGS10",
"value":   1.62,
"maturity":     10,
"country": "US" 
},
{
 "date": "12/04/2012",
"indexname": "DGS10",
"value":    1.6,
"maturity":     10,
"country": "US" 
},
{
 "date": "12/05/2012",
"indexname": "DGS10",
"value":   1.59,
"maturity":     10,
"country": "US" 
},
{
 "date": "12/06/2012",
"indexname": "DGS10",
"value":   1.64,
"maturity":     10,
"country": "US" 
},
{
 "date": "12/09/2012",
"indexname": "DGS10",
"value":   1.63,
"maturity":     10,
"country": "US" 
},
{
 "date": "12/10/2012",
"indexname": "DGS10",
"value":   1.66,
"maturity":     10,
"country": "US" 
},
{
 "date": "12/11/2012",
"indexname": "DGS10",
"value":   1.72,
"maturity":     10,
"country": "US" 
},
{
 "date": "12/12/2012",
"indexname": "DGS10",
"value":   1.74,
"maturity":     10,
"country": "US" 
},
{
 "date": "12/13/2012",
"indexname": "DGS10",
"value":   1.72,
"maturity":     10,
"country": "US" 
},
{
 "date": "12/16/2012",
"indexname": "DGS10",
"value":   1.78,
"maturity":     10,
"country": "US" 
},
{
 "date": "12/17/2012",
"indexname": "DGS10",
"value":   1.84,
"maturity":     10,
"country": "US" 
},
{
 "date": "12/18/2012",
"indexname": "DGS10",
"value":   1.82,
"maturity":     10,
"country": "US" 
},
{
 "date": "12/19/2012",
"indexname": "DGS10",
"value":   1.81,
"maturity":     10,
"country": "US" 
},
{
 "date": "12/20/2012",
"indexname": "DGS10",
"value":   1.77,
"maturity":     10,
"country": "US" 
},
{
 "date": "12/23/2012",
"indexname": "DGS10",
"value":   1.79,
"maturity":     10,
"country": "US" 
},
{
 "date": "12/25/2012",
"indexname": "DGS10",
"value":   1.77,
"maturity":     10,
"country": "US" 
},
{
 "date": "12/26/2012",
"indexname": "DGS10",
"value":   1.74,
"maturity":     10,
"country": "US" 
},
{
 "date": "12/27/2012",
"indexname": "DGS10",
"value":   1.73,
"maturity":     10,
"country": "US" 
},
{
 "date": "12/30/2012",
"indexname": "DGS10",
"value":   1.78,
"maturity":     10,
"country": "US" 
},
{
 "date": "01/01/2013",
"indexname": "DGS10",
"value":   1.86,
"maturity":     10,
"country": "US" 
},
{
 "date": "01/02/2013",
"indexname": "DGS10",
"value":   1.92,
"maturity":     10,
"country": "US" 
},
{
 "date": "01/03/2013",
"indexname": "DGS10",
"value":   1.93,
"maturity":     10,
"country": "US" 
},
{
 "date": "01/06/2013",
"indexname": "DGS10",
"value":   1.92,
"maturity":     10,
"country": "US" 
},
{
 "date": "01/07/2013",
"indexname": "DGS10",
"value":   1.89,
"maturity":     10,
"country": "US" 
},
{
 "date": "01/08/2013",
"indexname": "DGS10",
"value":   1.88,
"maturity":     10,
"country": "US" 
},
{
 "date": "01/09/2013",
"indexname": "DGS10",
"value":   1.91,
"maturity":     10,
"country": "US" 
},
{
 "date": "01/10/2013",
"indexname": "DGS10",
"value":   1.89,
"maturity":     10,
"country": "US" 
},
{
 "date": "01/13/2013",
"indexname": "DGS10",
"value":   1.89,
"maturity":     10,
"country": "US" 
},
{
 "date": "01/14/2013",
"indexname": "DGS10",
"value":   1.86,
"maturity":     10,
"country": "US" 
},
{
 "date": "01/15/2013",
"indexname": "DGS10",
"value":   1.84,
"maturity":     10,
"country": "US" 
},
{
 "date": "01/16/2013",
"indexname": "DGS10",
"value":   1.89,
"maturity":     10,
"country": "US" 
},
{
 "date": "01/17/2013",
"indexname": "DGS10",
"value":   1.87,
"maturity":     10,
"country": "US" 
},
{
 "date": "01/21/2013",
"indexname": "DGS10",
"value":   1.86,
"maturity":     10,
"country": "US" 
},
{
 "date": "01/22/2013",
"indexname": "DGS10",
"value":   1.86,
"maturity":     10,
"country": "US" 
},
{
 "date": "01/23/2013",
"indexname": "DGS10",
"value":   1.88,
"maturity":     10,
"country": "US" 
},
{
 "date": "01/24/2013",
"indexname": "DGS10",
"value":   1.98,
"maturity":     10,
"country": "US" 
},
{
 "date": "01/27/2013",
"indexname": "DGS10",
"value":      2,
"maturity":     10,
"country": "US" 
},
{
 "date": "01/28/2013",
"indexname": "DGS10",
"value":   2.03,
"maturity":     10,
"country": "US" 
},
{
 "date": "01/29/2013",
"indexname": "DGS10",
"value":   2.03,
"maturity":     10,
"country": "US" 
},
{
 "date": "01/30/2013",
"indexname": "DGS10",
"value":   2.02,
"maturity":     10,
"country": "US" 
},
{
 "date": "01/31/2013",
"indexname": "DGS10",
"value":   2.04,
"maturity":     10,
"country": "US" 
},
{
 "date": "02/03/2013",
"indexname": "DGS10",
"value":      2,
"maturity":     10,
"country": "US" 
},
{
 "date": "02/04/2013",
"indexname": "DGS10",
"value":   2.04,
"maturity":     10,
"country": "US" 
},
{
 "date": "02/05/2013",
"indexname": "DGS10",
"value":      2,
"maturity":     10,
"country": "US" 
},
{
 "date": "02/06/2013",
"indexname": "DGS10",
"value":   1.99,
"maturity":     10,
"country": "US" 
},
{
 "date": "02/07/2013",
"indexname": "DGS10",
"value":   1.99,
"maturity":     10,
"country": "US" 
},
{
 "date": "02/10/2013",
"indexname": "DGS10",
"value":   1.99,
"maturity":     10,
"country": "US" 
},
{
 "date": "02/11/2013",
"indexname": "DGS10",
"value":   2.02,
"maturity":     10,
"country": "US" 
},
{
 "date": "02/12/2013",
"indexname": "DGS10",
"value":   2.05,
"maturity":     10,
"country": "US" 
},
{
 "date": "02/13/2013",
"indexname": "DGS10",
"value":      2,
"maturity":     10,
"country": "US" 
},
{
 "date": "02/14/2013",
"indexname": "DGS10",
"value":   2.01,
"maturity":     10,
"country": "US" 
},
{
 "date": "02/18/2013",
"indexname": "DGS10",
"value":   2.03,
"maturity":     10,
"country": "US" 
},
{
 "date": "02/19/2013",
"indexname": "DGS10",
"value":   2.02,
"maturity":     10,
"country": "US" 
},
{
 "date": "02/20/2013",
"indexname": "DGS10",
"value":   1.99,
"maturity":     10,
"country": "US" 
},
{
 "date": "02/21/2013",
"indexname": "DGS10",
"value":   1.97,
"maturity":     10,
"country": "US" 
},
{
 "date": "02/24/2013",
"indexname": "DGS10",
"value":   1.88,
"maturity":     10,
"country": "US" 
},
{
 "date": "02/25/2013",
"indexname": "DGS10",
"value":   1.88,
"maturity":     10,
"country": "US" 
},
{
 "date": "02/26/2013",
"indexname": "DGS10",
"value":   1.91,
"maturity":     10,
"country": "US" 
},
{
 "date": "02/27/2013",
"indexname": "DGS10",
"value":   1.89,
"maturity":     10,
"country": "US" 
},
{
 "date": "02/28/2013",
"indexname": "DGS10",
"value":   1.86,
"maturity":     10,
"country": "US" 
},
{
 "date": "03/03/2013",
"indexname": "DGS10",
"value":   1.88,
"maturity":     10,
"country": "US" 
},
{
 "date": "03/04/2013",
"indexname": "DGS10",
"value":    1.9,
"maturity":     10,
"country": "US" 
},
{
 "date": "03/05/2013",
"indexname": "DGS10",
"value":   1.95,
"maturity":     10,
"country": "US" 
},
{
 "date": "03/06/2013",
"indexname": "DGS10",
"value":      2,
"maturity":     10,
"country": "US" 
},
{
 "date": "03/07/2013",
"indexname": "DGS10",
"value":   2.06,
"maturity":     10,
"country": "US" 
},
{
 "date": "03/10/2013",
"indexname": "DGS10",
"value":   2.07,
"maturity":     10,
"country": "US" 
},
{
 "date": "03/11/2013",
"indexname": "DGS10",
"value":   2.03,
"maturity":     10,
"country": "US" 
},
{
 "date": "03/12/2013",
"indexname": "DGS10",
"value":   2.04,
"maturity":     10,
"country": "US" 
},
{
 "date": "03/13/2013",
"indexname": "DGS10",
"value":   2.04,
"maturity":     10,
"country": "US" 
},
{
 "date": "03/14/2013",
"indexname": "DGS10",
"value":   2.01,
"maturity":     10,
"country": "US" 
},
{
 "date": "03/17/2013",
"indexname": "DGS10",
"value":   1.96,
"maturity":     10,
"country": "US" 
},
{
 "date": "03/18/2013",
"indexname": "DGS10",
"value":   1.92,
"maturity":     10,
"country": "US" 
},
{
 "date": "03/19/2013",
"indexname": "DGS10",
"value":   1.96,
"maturity":     10,
"country": "US" 
},
{
 "date": "03/20/2013",
"indexname": "DGS10",
"value":   1.95,
"maturity":     10,
"country": "US" 
},
{
 "date": "03/21/2013",
"indexname": "DGS10",
"value":   1.93,
"maturity":     10,
"country": "US" 
},
{
 "date": "03/24/2013",
"indexname": "DGS10",
"value":   1.93,
"maturity":     10,
"country": "US" 
},
{
 "date": "03/25/2013",
"indexname": "DGS10",
"value":   1.92,
"maturity":     10,
"country": "US" 
},
{
 "date": "03/26/2013",
"indexname": "DGS10",
"value":   1.87,
"maturity":     10,
"country": "US" 
},
{
 "date": "03/27/2013",
"indexname": "DGS10",
"value":   1.87,
"maturity":     10,
"country": "US" 
},
{
 "date": "03/31/2013",
"indexname": "DGS10",
"value":   1.86,
"maturity":     10,
"country": "US" 
},
{
 "date": "04/01/2013",
"indexname": "DGS10",
"value":   1.88,
"maturity":     10,
"country": "US" 
},
{
 "date": "04/02/2013",
"indexname": "DGS10",
"value":   1.83,
"maturity":     10,
"country": "US" 
},
{
 "date": "04/03/2013",
"indexname": "DGS10",
"value":   1.78,
"maturity":     10,
"country": "US" 
},
{
 "date": "04/04/2013",
"indexname": "DGS10",
"value":   1.72,
"maturity":     10,
"country": "US" 
},
{
 "date": "04/07/2013",
"indexname": "DGS10",
"value":   1.76,
"maturity":     10,
"country": "US" 
},
{
 "date": "04/08/2013",
"indexname": "DGS10",
"value":   1.78,
"maturity":     10,
"country": "US" 
},
{
 "date": "04/09/2013",
"indexname": "DGS10",
"value":   1.84,
"maturity":     10,
"country": "US" 
},
{
 "date": "04/10/2013",
"indexname": "DGS10",
"value":   1.82,
"maturity":     10,
"country": "US" 
},
{
 "date": "04/11/2013",
"indexname": "DGS10",
"value":   1.75,
"maturity":     10,
"country": "US" 
},
{
 "date": "04/14/2013",
"indexname": "DGS10",
"value":   1.72,
"maturity":     10,
"country": "US" 
},
{
 "date": "04/15/2013",
"indexname": "DGS10",
"value":   1.75,
"maturity":     10,
"country": "US" 
},
{
 "date": "04/16/2013",
"indexname": "DGS10",
"value":   1.73,
"maturity":     10,
"country": "US" 
},
{
 "date": "04/17/2013",
"indexname": "DGS10",
"value":   1.72,
"maturity":     10,
"country": "US" 
},
{
 "date": "04/18/2013",
"indexname": "DGS10",
"value":   1.73,
"maturity":     10,
"country": "US" 
},
{
 "date": "04/21/2013",
"indexname": "DGS10",
"value":   1.72,
"maturity":     10,
"country": "US" 
},
{
 "date": "04/22/2013",
"indexname": "DGS10",
"value":   1.74,
"maturity":     10,
"country": "US" 
},
{
 "date": "04/23/2013",
"indexname": "DGS10",
"value":   1.73,
"maturity":     10,
"country": "US" 
},
{
 "date": "04/24/2013",
"indexname": "DGS10",
"value":   1.74,
"maturity":     10,
"country": "US" 
},
{
 "date": "04/25/2013",
"indexname": "DGS10",
"value":    1.7,
"maturity":     10,
"country": "US" 
},
{
 "date": "04/28/2013",
"indexname": "DGS10",
"value":    1.7,
"maturity":     10,
"country": "US" 
},
{
 "date": "04/29/2013",
"indexname": "DGS10",
"value":    1.7,
"maturity":     10,
"country": "US" 
},
{
 "date": "04/30/2013",
"indexname": "DGS10",
"value":   1.66,
"maturity":     10,
"country": "US" 
},
{
 "date": "05/01/2013",
"indexname": "DGS10",
"value":   1.66,
"maturity":     10,
"country": "US" 
},
{
 "date": "05/02/2013",
"indexname": "DGS10",
"value":   1.78,
"maturity":     10,
"country": "US" 
},
{
 "date": "05/05/2013",
"indexname": "DGS10",
"value":    1.8,
"maturity":     10,
"country": "US" 
},
{
 "date": "05/06/2013",
"indexname": "DGS10",
"value":   1.82,
"maturity":     10,
"country": "US" 
},
{
 "date": "05/07/2013",
"indexname": "DGS10",
"value":   1.81,
"maturity":     10,
"country": "US" 
},
{
 "date": "05/08/2013",
"indexname": "DGS10",
"value":   1.81,
"maturity":     10,
"country": "US" 
},
{
 "date": "05/09/2013",
"indexname": "DGS10",
"value":    1.9,
"maturity":     10,
"country": "US" 
},
{
 "date": "05/12/2013",
"indexname": "DGS10",
"value":   1.92,
"maturity":     10,
"country": "US" 
},
{
 "date": "05/13/2013",
"indexname": "DGS10",
"value":   1.96,
"maturity":     10,
"country": "US" 
},
{
 "date": "05/14/2013",
"indexname": "DGS10",
"value":   1.94,
"maturity":     10,
"country": "US" 
},
{
 "date": "05/15/2013",
"indexname": "DGS10",
"value":   1.87,
"maturity":     10,
"country": "US" 
},
{
 "date": "05/16/2013",
"indexname": "DGS10",
"value":   1.95,
"maturity":     10,
"country": "US" 
},
{
 "date": "05/19/2013",
"indexname": "DGS10",
"value":   1.97,
"maturity":     10,
"country": "US" 
},
{
 "date": "05/20/2013",
"indexname": "DGS10",
"value":   1.94,
"maturity":     10,
"country": "US" 
},
{
 "date": "05/21/2013",
"indexname": "DGS10",
"value":   2.03,
"maturity":     10,
"country": "US" 
},
{
 "date": "05/22/2013",
"indexname": "DGS10",
"value":   2.02,
"maturity":     10,
"country": "US" 
},
{
 "date": "05/23/2013",
"indexname": "DGS10",
"value":   2.01,
"maturity":     10,
"country": "US" 
},
{
 "date": "05/27/2013",
"indexname": "DGS10",
"value":   2.15,
"maturity":     10,
"country": "US" 
},
{
 "date": "05/28/2013",
"indexname": "DGS10",
"value":   2.13,
"maturity":     10,
"country": "US" 
},
{
 "date": "05/29/2013",
"indexname": "DGS10",
"value":   2.13,
"maturity":     10,
"country": "US" 
},
{
 "date": "05/30/2013",
"indexname": "DGS10",
"value":   2.16,
"maturity":     10,
"country": "US" 
},
{
 "date": "06/02/2013",
"indexname": "DGS10",
"value":   2.13,
"maturity":     10,
"country": "US" 
},
{
 "date": "06/03/2013",
"indexname": "DGS10",
"value":   2.14,
"maturity":     10,
"country": "US" 
},
{
 "date": "06/04/2013",
"indexname": "DGS10",
"value":    2.1,
"maturity":     10,
"country": "US" 
},
{
 "date": "06/05/2013",
"indexname": "DGS10",
"value":   2.08,
"maturity":     10,
"country": "US" 
},
{
 "date": "06/06/2013",
"indexname": "DGS10",
"value":   2.17,
"maturity":     10,
"country": "US" 
},
{
 "date": "06/09/2013",
"indexname": "DGS10",
"value":   2.22,
"maturity":     10,
"country": "US" 
},
{
 "date": "06/10/2013",
"indexname": "DGS10",
"value":    2.2,
"maturity":     10,
"country": "US" 
},
{
 "date": "06/11/2013",
"indexname": "DGS10",
"value":   2.25,
"maturity":     10,
"country": "US" 
},
{
 "date": "06/12/2013",
"indexname": "DGS10",
"value":   2.19,
"maturity":     10,
"country": "US" 
},
{
 "date": "06/13/2013",
"indexname": "DGS10",
"value":   2.14,
"maturity":     10,
"country": "US" 
},
{
 "date": "06/16/2013",
"indexname": "DGS10",
"value":   2.19,
"maturity":     10,
"country": "US" 
},
{
 "date": "06/17/2013",
"indexname": "DGS10",
"value":    2.2,
"maturity":     10,
"country": "US" 
},
{
 "date": "06/18/2013",
"indexname": "DGS10",
"value":   2.33,
"maturity":     10,
"country": "US" 
},
{
 "date": "06/19/2013",
"indexname": "DGS10",
"value":   2.41,
"maturity":     10,
"country": "US" 
},
{
 "date": "06/20/2013",
"indexname": "DGS10",
"value":   2.52,
"maturity":     10,
"country": "US" 
},
{
 "date": "06/23/2013",
"indexname": "DGS10",
"value":   2.57,
"maturity":     10,
"country": "US" 
} 
],
    xAxis = {
 "type": "addCategoryAxis",
"showPercent": false 
},
    yAxis = {
 "type": "addMeasureAxis",
"showPercent": false 
},
    zAxis = [],
    legend = [];
  var svg = dimple.newSvg("#" + opts.id, opts.width, opts.height);

  //data = dimple.filterData(data, "Owner", ["Aperture", "Black Mesa"])
  var myChart = new dimple.chart(svg, data);
  if (opts.bounds) {
    myChart.setBounds(x = opts.bounds.x, y = opts.bounds.y, height = opts.bounds.height, width = opts.bounds.width);//myChart.setBounds(80, 30, 480, 330);
  }
  //dimple allows use of custom CSS with noFormats
  if(opts.noFormats) { myChart.noFormats = opts.noFormats; };
  //for markimekko and addAxis also have third parameter measure
  //so need to evaluate if measure provided
  //x axis
  var x;
  if(xAxis.measure) {
    x = myChart[xAxis.type]("x",opts.x,xAxis.measure);
  } else {
    x = myChart[xAxis.type]("x", opts.x);
  };
  if(!(xAxis.type === "addPctAxis")) x.showPercent = xAxis.showPercent;
  if (xAxis.orderRule) x.addOrderRule(xAxis.orderRule);
  if (xAxis.grouporderRule) x.addGroupOrderRule(xAxis.grouporderRule);  
  if (xAxis.overrideMin) x.overrideMin = xAxis.overrideMin;
  if (xAxis.overrideMax) x.overrideMax = xAxis.overrideMax;
  //y axis
  var y;
  if(yAxis.measure) {
    y = myChart[yAxis.type]("y",opts.y,yAxis.measure);
  } else {
    y = myChart[yAxis.type]("y", opts.y);
  };
  if(!(yAxis.type === "addPctAxis")) y.showPercent = yAxis.showPercent;
  if (yAxis.orderRule) y.addOrderRule(yAxis.orderRule);
  if (yAxis.grouporderRule) y.addGroupOrderRule(yAxis.grouporderRule);
  if (yAxis.overrideMin) y.overrideMin = yAxis.overrideMin;
  if (yAxis.overrideMax) y.overrideMax = yAxis.overrideMax;
  //z for bubbles
    var z;
  if (!(typeof(zAxis) === 'undefined') && zAxis.type){
    if(zAxis.measure) {
      z = myChart[zAxis.type]("z",opts.z,zAxis.measure);
    } else {
      z = myChart[zAxis.type]("z", opts.z);
    };
    if(!(zAxis.type === "addPctAxis")) z.showPercent = zAxis.showPercent;
    if (zAxis.orderRule) z.addOrderRule(zAxis.orderRule);
    if (zAxis.overrideMin) z.overrideMin = zAxis.overrideMin;
    if (zAxis.overrideMax) z.overrideMax = zAxis.overrideMax;
  }
  //here need think I need to evaluate group and if missing do null
  //as the first argument
  //if provided need to use groups from opts
  if(opts.hasOwnProperty("groups")) {
    var s = myChart.addSeries( opts.groups, dimple.plot[opts.type] );
    //series offers an aggregate method that we will also need to check if available
    //options available are avg, count, max, min, sum
    if (!(typeof(opts.aggregate) === 'undefined')) {
      s.aggregate = eval(opts.aggregate);
    }
    if (!(typeof(opts.lineWeight) === 'undefined')) {
      s.lineWeight = eval(opts.lineWeight);
    }
    if (!(typeof(opts.barGap) === 'undefined')) {
      s.barGap = eval(opts.barGap);
    }    
  } else var s = myChart.addSeries( null, dimple.plot[opts.type] );
  //unsure if this is best but if legend is provided (not empty) then evaluate
  if(d3.keys(legend).length > 0) {
    var l =myChart.addLegend();
    d3.keys(legend).forEach(function(d){
      l[d] = legend[d];
    });
  }
  //quick way to get this going but need to make this cleaner
  if(opts.storyboard) {
    myChart.setStoryboard(opts.storyboard);
  };
  myChart.draw();

</script>

<style>
body {
  font: 10px sans-serif;
  margin: 0;
}

.x.axis path {
  display: none;
}
</style>

<script>
//get Dates in d3 js format
data.forEach(function(d) {
  d.date = d3.time.format('%M/%d/%Y').parse(d.date);
});

//remove dimple axis
//hoping x is always drawn first
d3.select(".axis").remove()

//from Bostock example http://bl.ocks.org/mbostock/1166403
var xd3 = d3.time.scale().range([myChart.x, myChart.x + myChart.width]),
xAxisd3 = d3.svg.axis().scale(xd3).tickSize(-myChart.height).tickSubdivide(true);
xd3.domain([data[0].date, data[data.length - 1].date]);
svg.append("svg:g")
  .attr("class", "x axis")
  .attr("transform", "translate(0," + (+myChart.height+myChart.y) + ")")
  .style("stroke","black")
  .call(xAxisd3);
</script>


