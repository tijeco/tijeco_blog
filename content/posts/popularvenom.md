+++
title = "Popularity of venom"
description = "Is Venom just a fad?"
tags = [
    "tutorial",
    "bioinformatics",
    "molecular evolution",
    
]
date = "2018-01-21"

categories = [
    "Bioinformatics",
    
]
menu = "main"
+++

For a while now I have been enamored by the question of what is venom? It is a fairly nuanced question that will undoubtedly have a nuanced answer, but today I have decided to describe trends in what has been called a venom to this date.

<!--more-->

# What is venom?

Well, this question is a bit too deep and philosophical for me to have any interest in diving to it at this point in time, but suffice it to say, it’s complicated. Perhaps we can move on to a slightly more manageable question. 

# What has been called venom?

Alright, maybe I can tackle that a little bit better. Just for clarification, when I talk about “venom”, I actually mean venom proteins d not the whole of venom. So what proteins have been characterized as venom proteins? Currently, UniProt is the end-all-be-all for fully describing the properties of a given protein through vigorous amounts of testing and verification. Over 5,000 proteins have been verified to be venom proteins.

## Why so few venoms described?

The small number of described venoms has always disturbed me. It’s such a diverse trait found among numerous phyla of animals, so I would expect there to be upwards of millions of actual venom proteins in the animal kingdom. The bottleneck here seems to be the verification/annotation step. It’s fairly easy to determine the sequence of a protein, but much less easy determine what the damn thing actually does. So that leaves us with the situation we are in now, too much data then we know what to do with. Venom gland transcriptomes with thousands of putative venom protein sequences are coming out each month, yet it doesn’t appear that efforts in describing these sequences in UniProt are catching up.

So I decided to test my suspicion that venom protein annotation is not on the rise lately. I downloaded metadata for sequences from Uniprot that are verified to be venom and plotted the number of sequences that have been verified each year.


The data I used is available from UniProt search terms "taxonomy:metazoa keyword:toxin AND reviewed:yes". 
R scripts to generate the plot will be included as I go through them. Data includes two columns of years that correspond to the year that a venom protein was either first uploaded or last modified. 






# Get the data

We just need to load up the data and take a quick look at it.
```{r}
library(tidyverse)
venom_dates <- read.table("data/venom.txt",header = T)
```

```{r}
summary(venom_dates)
```
It's interesting that entries go back as far as 1986!

# Prepare data

Now we just need to do some minor adjustments to the data to prepare it for plotting. Since the data is literally just a list of years, it is best that we convert it to a table describing the frequencies that each year occurred. This will correspond to the number of venom proteins uploaded in a given year. 

The other lines are probably a bit clunky, but it gets the job done, in a somewhat concise matter. 
```{r}
 modified <- as.data.frame(table(venom_dates$modified))
modified$Var1 = as.numeric(as.character(modified$Var1))
created <- as.data.frame(table(venom_dates$created))
created$Var1 = as.numeric(as.character(created$Var1))
venom_dates <- data.frame(modified$Var1,modified$Freq,created$Var1,created$Freq)
colnames(venom_dates) <- c("yearMod","numMod","year","verified")
```




# Make a plot

Let's throw this into ggplot and see what happens!

```{r}
ggplot(data = venom_dates)+
  geom_smooth(mapping = aes(x = year, y = verified),se=F)+
  geom_smooth(mapping = aes(x = yearMod, y =   numMod),         colour="red",se=F)
  
```
![This file is gone now](http://www.blog.tijeco.info/plots/unnamed-chunk-4-1.png)


The blue line is when venom sequences were first verified, the red line is when venom sequences were last modified. 

It seems that there was a nice uptick around 2010. I'd imagine life was great for venom biologists at that time. They would probably make this graph and optimistically think that venom annotation is on the rise! Well unfortunately here in the future things aren't as peachy. There has been a sharp downward trend in the number of verified venom sequences in the past seven years.

I'll say this definitely isn't the chart I wanted to see. Optimistically this baby would be flying through the roof nearly exponentially! But it doesn't seem to be doing that. At least not yet! This only reminds me that there is plenty more work to be done, so I will remain optimistic.