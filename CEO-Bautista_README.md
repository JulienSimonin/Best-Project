# α-diversity assessment in R 
By Christian Elmarc O. Bautista

## About documentation
This documentation helps in creating box plots reflecting α-diversity using vegan and ggplot2 packages in R.

## Expected visualizations
![Sample of a α-diversity boxplot](https://raw.githubusercontent.com/JulienSimonin/Best-Project/refs/heads/main/Sample%20Photo.jpg)

## Sample publications
This type of visualization can be used for analyzing the biodiversity of multiple communities. This is a [sample publication](https://smujo.id/biodiv/article/view/13590) that utizilies such graph.

## Steps in creating boxplots in R
- Set working directory
```
setwd("C:/Working-directory")
```
- Load packages into R
```
library(ggplot2)
library(vegan)
library(cowplot)
```
- Load .csv data
```
Data = read.csv("Data.csv", header=T, row.names=1)
Metadata = read.csv("Metadata.csv", header=T)
```
- Analyze α-diversity using vegan
```Count = apply(Data,1,sum)
Alpha = fisher.alpha(Data)
Shannon = diversity(Data)
Simpsons = simpson.unb(Data)
Richness = specnumber(Data)
Alpha_div = as.data.frame(Count)
Alpha_div$Fisher = Alpha
Alpha_div$Richness = Richness
Alpha_div$Shannon = Shannon
Alpha_div$Simpsons = Simpsons
Alpha_div$Site = Metadata$Site
```
- Graph α-diversity data using ggplot2
```
P_Richness = ggplot(Alpha_div, aes(x = as.factor(Site), y = Richness, fill = Site)) + 
  geom_boxplot(alpha = 0.5) + scale_fill_brewer(palette="Greens") +
  xlab("") + ylab("Observed Diversity") + ggtitle("Richness") + 
  theme(axis.text.x = element_text(size = 12),axis.text.y = element_text(size = 12), axis.title = element_text(face="bold", size = 15), legend.position = "none", plot.title = element_text(size=15, face="bold", hjust = 0.5))
P_Fisher = ggplot(Alpha_div, aes(x = as.factor(Site), y = Fisher, fill = Site)) + 
  geom_boxplot(alpha = 0.5) + scale_fill_brewer(palette="Greens") +
  xlab("") + ylab(" ") + ggtitle("Fisher") + 
  theme(axis.text.x = element_text(size = 12),axis.text.y = element_text(size = 12),axis.title = element_text(face="bold", size = 15), legend.position = "none", plot.title = element_text(size=15, face="bold", hjust = 0.5))
P_Shannon = ggplot(Alpha_div, aes(x = as.factor(Site), y = Shannon, fill = Site)) + 
  geom_boxplot(alpha = 0.5) + scale_fill_brewer(palette="Greens") +
  xlab("") + ylab(" ") + ggtitle("Shannon") + 
  theme(axis.text.x = element_text(size = 12),axis.text.y = element_text(size = 12),axis.title = element_text(face="bold", size = 15), legend.position = "none", plot.title = element_text(size=15, face="bold", hjust = 0.5))
P_Simpson = ggplot(Alpha_div, aes(x = as.factor(Site), y = Simpsons, fill = Site)) + 
  geom_boxplot(alpha = 0.5) + scale_fill_brewer(palette="Greens") +
  xlab("") + ylab(" ") + ggtitle("Simpson") + 
  theme(axis.text.x = element_text(size = 12),axis.text.y = element_text(size = 12),axis.title = element_text(face="bold", size = 15), legend.position = "none", plot.title = element_text(size=15, face="bold", hjust = 0.5))
```
- Compile graphs using cowplot
```
plot_grid(P_Richness,P_Fisher,P_Shannon,P_Simpson, labels = c(" "), align = "hv", nrow = 1)
```
