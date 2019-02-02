---
title       : Introduction to the Tidyverse
subtitle    : Beware You May not Want to go back to Base R
author      : Adam J Sullivan 
job         : Assistant Professor of Biostatistics
work        : Brown University
framework   : io2012        # {io2012, html5slides, shower, dzslides, ...}
highlighter : highlight.js # {highlight.js, prettify, highlight}
hitheme     :  github     # 
widgets     : [mathjax, quiz, bootstrap, interactive] # {mathjax, quiz, bootstrap}
ext_widgets : {rCharts: [libraries/nvd3, libraries/leaflet, libraries/dygraphs]}
mode        : selfcontained # {standalone, draft}
knit        : slidify::knit2slides
logo        : publichealthlogo.png
biglogo     : publichealthlogo.png
assets      : {assets: ../../assets}
--- .class #id



<script src="https://ajax.googleapis.com/ajax/libs/jquery/3.1.1/jquery.min.js"></script>
<script>
$(document).ready(function() {
  $('slide:not(.backdrop):not(.title-slide)').append('<div class=\"footnotes\">');

  $('footnote').each(function(index) {
    var text  = $(this).html();
    var fnNum = (index+1).toString();
    $(this).html(fnNum.sup());

    var footnote   = fnNum + '. ' + text + '<br/>';
    var oldContent = $(this).parents('slide').children('div.footnotes').html();
    var newContent = oldContent + footnote;
    $(this).parents('slide').children('div.footnotes').html(newContent);
  });
});
</script>







## The Tidyverse

![](assets/img/tidyverse.png)<footnote> [Martin Frigaard](http://www.storybench.org/author/martinfri/)

--- .class .id

## What is the tidyverse?

- Collection of R Packages that:
    - have common coding styles
    - work well together
    - work quickly as they are coded to run in C++
- Can be easily extended to work with:
      - relational databases
      - spark
      - hadoop
      - ...

--- &twocol

## How Do we get this?


***  =left
### Do This Once

```r
install.packages("tidyverse")
```



*** =right
### Do this each time

```r
library(tidyverse)
```


--- .class .id 


## Roadmap

Activity | Time 
------ | --------
Introduction | 30 minutes
Visualizing with `ggplot` | 1 hour
Tidying Data with `tidyr` | 30 minutes
Summarizing data with `dplyr`  | 1 hour
Extended Practice | 1 hour
