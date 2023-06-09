## Introduction

Time and time again, we are reminded of the importance of representing underrepresented groups. In the case of media, representation is about creating a social environment where overrepresented groups need to be aware of the world's diversity. In the context of statistics, representation is about ensuring that decisions are made with everyone's needs in mind. Unfortunately but unsurprisingly, queer people are frequently left out of these decisions due to a lack of interest and desire to incorporate minorities' needs into these decision-making processes by said overrepresented groups. However, we have seen some progress on this front in recent years.

In 2015, the media agency attempted to compile this data into the so-called Gaybourhoods data set, which geographically positions data concerning queer people in different neighbourhoods in 15 major US cities. Over the last semester, we have sought to use this data to answer real-world questions about queerness in the United States. Specifically, we sought to answer four principal research questions: Do queer communities concentrate in space? How do queer communities tend to vote in US elections? How are queer communities affected by taxes relative to the average US community? And finally, is the presence of gay bars in a given neighbourhood indicative of whether or not a pride parade runs through it?

## Exploratory Data Analysis
A substantial portion of our exploratory data analysis involved trying to determine how best to represent our data on a two-dimensional plane. The two approaches we settled on involved using density (later topological) maps and scatter plots with respect to the geographical coordinates of each observation:

![Scatter plot of observations from the Gaybourhoods dataset](analysis/images/graphs/1-naive-scatter1.png)
![Hexbin plot illustrating the density of counties across the US](analysis/images/graphs/4-plot-naive-hexbin.png)

In the case of scatter plots, representing a tertiary dimension of the data could usually be accomplished by colouring the observations relative to some additional statistic. For example, one of the graphs we created in our exploratory data analysis illustrated all of the observations in Boston, coloured by how many gay/lesbian individuals resided in each neighbourhood:

![Scatter plot of Gaybourhoods observations in Boston coloured by queer concentration](analysis/images/graphs/6-plot-boston-scatter2)

This approach proved to be effective, particularly when combined with nice maps to better visually position the data in space, and so we used it throughout our analyses.

## Do queer communities concentrate in space?
The objective of this research question is to determine if queer communities are geographically concentrated. More specifically, we wanted to determine if a community with a high population of gay and lesbian residents is likely to be surrounded by communities with a similarly-sized population of gay and lesbian residents. This can be broken down more quantitatively by asking the following: for a neighbourhood measurably queer to some degree, how queer are the adjacent neighbourhoods on average?

## Quantitatively measuring queerness
At several points during this analysis, we will refer to a given neighbourhood's "queerness" as though it's a single, continuous, quantitative variable. We do this for convenience and to more effectively work within the constraints of the data we have available, although it's worth admitting and discussing what this means and it's limitations. It should be obvious that how we quantitatively measure the queerness of a space is subjective, and the decisions we make in this analysis can be problematic.

To begin, we must acknowledge the role statisticians have played presently and historically in systematically eliminating minorities. One local example of this is the way that Canada's Indian Act works to incrementally strip Indigenous people of their legal recognition of being Indigenous through the maliciously-named process of "enfranchisement"[^1], in a process many now refer to as "statistical genocide." Discretely categorizing people enables oppression and marginalizes deviation. That second issue is particularly pertinent in the case of the queer community, which is predicated on "bending rules," so to speak. For that reason, we are hesitant to use the phrase "queer community," as it implicitly makes the assumption that the constituents of the so-called "queer community" have a universal experience, which is untrue.

Jan Diehm admits the following in "Men are from Chelsea, Women are from Park Slope"[^2] [^3].

> Currently, there’s no comprehensive way to quantitatively measure gayborhoods, or even where LGBTQ Americans live. Most of the existing data sticks to a narrow view (i.e. traditional marriage, the male/female gender binary) of the queer spectrum and “rainbow-washes” any intersectionality of race, ethnicity, class, gender, and sexuality. This project aims to paint a slightly more complete picture, combining several metrics to create a gayborhood index, but even then it admittedly underrepresents and under-counts areas with non-binary and minority populations. Still, this is some of the most complete data that we have.

This dataset fails to represent queerness outside the context of monogamous partnerships between cisgender people (or at least, those who have been statistically represented as such). For this reason, we seek to be very upfront that we are only exploring so-called "same-sex" partnerships.

The individuals who worked on the article attempted to mitigate some of these issues by drawing from a variety of sources to measure the queerness of a given neighbourhood, including whether or not a pride parade travels through the neighbourhood or the number of bars tagged as "gay" on Yelp in the area. In our study of the concentration of queer communities and their political inclinations, we decided to ignore all factors that don't involve the number of gay and/or lesbian residents in the neighbourhood.

![TOTINDEX topographically compared to SS_Index in New York City](analysis/images/graphs/9-ny-queer-metrics.png)

The above graph illustrates a topological graph of gaybouhoods in New York City shaded darker by two metrics of queerness: "TOTINDEX" being the composite index and the latter representing only the number of gay and lesbian residents. While the graphs are visually distinct, the distinction is relatively minor. Nonetheless, we proceed using the latter as a key symbolic decision.

To facilitate the discussion of queerness in space in the first two research questions, we introduce an additional index that discretely classifies neighbourhoods into 7 categories labelled `0` through `6`, with zero indicating a region has the fewest relative gay/lesbian residents and 6 indicating that the region has relatively the most gay/lesbian residents. The choice to divide the data frame into seven categories was arbitrary, although inspired by Alfred Kinsey's research into the fluidity of human sexuality[^4]. Similarly to the Kinsey scale, the relationship will be linear.

Besides the Kinsey index of a given observation, we are also interested in the Kinsey index of observations adjacent to a given neighbourhood. This, we refer to as the observation's "neighbourhood Kinsey index," or NKI, where our usage of the word "neighbourhood" is borrowed from graph theory, in referring to the set of all vertices connected by an edge to a given vertex. This measurement is calculated algorithmically by sampling a small set of observations geographically near each neighbourhood. A full implementation of this algorithm can be found [here](./analysis/code/project_functions1.py).

### Quantitatively representing queer concentration

![Two graphs, the first a bar graph and the second a scatter plot](analysis/images/graphs/13-neighbourhood-kinsey-comparison.png)

The first graph illustrates the mean neighbourhood Kinsey index of all observations for each Kinsey index, and as such, the height of each graph represents how queer adjacent neighbourhoods of a given neighbourhood will be on average. Notably, in general, the neighbourhoods adjacent to a given relatively queer neighbourhood are not on average more queer than the given neighbourhood. This is not particularly surprising when we consider the fact that queer people form a minority of the general population. However, on average, the more queer a given neighbourhood is, the more queer its adjacent neighbourhoods will be on average across the United States. This provides some evidence that queer communities tend to concentrate in space.

The second graph compares the mean neighbourhood Kinsey index of each observation to its same-sex index, revealing that the same trend is present, although there is a substantial amount of variation. Similar to the first graph, we see that this trend becomes less representative for neighbourhoods with a higher Kinsey index. This makes sense when we consider that observations forming the geographical peak will necessarily be surrounded by neighbourhoods of a lower Kinsey index.

### Topographically illustrating queer concentration

![15 topographical graphs illustrating queer concentration in 15 American cities](analysis/images/graphs/12-queer-concentration-nationally.png)

The previous 15 graphs topographically represent the concentration of queer communities in 15 cities across the United States. Regions shaded darker contain more queer residents per neighbourhood. In all 15 cities studied, we see a relatively sharp "peak" in gay residents in one area. Further, neighbourhoods tend to get less queer radially outwards of this peak. Another interesting observation is that with the exception of Chicago and Miami, all of the queerest communities in each city tend to be clustered around the geographical city centre. This is in line with conventional wisdom that the inner-city tends to be inhabited primarily by poor people and other marginalized groups, while the more privileged groups tend to live outside the city, commuting in for work. The exceptionally of Chicago and Miami could be due to unique city planning.

Although the overarching trend remains, there are some inherent limitations to using topological graphs to illustrate this data. These limitations are explored further in our [complete analysis](analysis/analysis1.ipynb).

To briefly summarize: our research is consistent with the notion that queer communities tend to concentrate in space, and this trend appears to be consistent in cities across the United States.

## How do queer communities lean politically?

In the second research question, we sought to determine the relationship between the concentration of gay and lesbian residents in a neighbourhood and the percentage of the neighbourhood's county who voted democrat in the 2012 American presidential election. From this, we hope to gleam insight into how living in a highly queer neighbourhood influences residents' political alignment.

This analysis is limited by the fact that the smallest spatial unit in American presidential elections is the county, and the gaybourhood data set observations are positioned spatially by coordinates. We solved this problem by using the geographic coordinates of the counties represented in the gaybourhoods dataset to calculate the nearest county to each gaybourhood observation. This algorithm works similar to how we calculated the average Kinsey index about a given neighbourhood, and its complete implementation can again be found [here](./analysis/code/project_functions1.py).

![Bar graph of democrat vote share by Kinsey index)(analysis/images/graphs/15-kinsey-democrat-plot.png)

Using the tools discussed in the previous section, it is immediately apparent that on average, neighbourhoods across the country that have a higher Kinsey index tend to vote more democrat, although, similar to last time, this trend becomes less apparent for neighbourhoods with a higher concentration of queer people. There's a number of factors that could influence this. As shown in the previous section, neighbourhoods with a higher Kinsey index tend to have less spread across the city, and when we factor in gerrymandering, it's possible these regions are more likely to be unfairly drawn into a county with fewer Democratic voters.

To take a closer look on the city-level, we can use the same approach as last time to visualize the two phenomena topographically:

![Overlapping topographical maps for queer and democrat density in 15 cities in the US](analysis/images/graphs/14-queer-vs-democrat-density.png)

When we illustrate the density of queerness and democrat votership, we see that in seven of the cities, the peaks completely overlap. In the vast majority of the cities studied, the peaks mostly overlap. Only in the case of Miami do the peaks seem to not overlap. This exception is likely due to the fact that here, our usage of this type of graph is misleading, because most of the region covered by the gaybourhoods dataset is contained within a single county.

Through both our numerical and spatial research, the results consistently show that neighbourhoods with a higher number of queer residents tend to vote more democrat.

## Do queer communities pay more in taxes?
![Scatter plot of queer communities by tax rate](analysis/images/graphs/queer-tax-rate.png)
![Scatter plot of communities by tax rate](analysis/images/graphs/typical-tax-rate.png)

The previous two graphs depict the neighbourhoods in the Gaybourhoods data set coloured by how much their residents pay in taxes. It's visually clear from the first graph that queer neighbourhoods tend to pay more in their taxes. This phenomenon can be explored more quantitatively in the following diagram:

![Pair plot comparing queer to overall tax rates](analysis/images/graphs/queer-tax-decomposition.png)

As we can infer by taking the first derivative of the correlation line of this graph, queer communities pay significantly more taxes than other neighbourhoods. One explanation for this is that queer people may, through one mechanism or another, end up correlating strongly with demographics who pay more taxes. Do note that the analysis is severely limited by severe sampling bias as only hyper urban geographical stratum have been surveyed in the construction of this data set.

From a previous analysis, we know that queer communities tend to concentrate in the geographical centre of all the cities surveyed. So, we can draw the related conclusion that people who live in the middle of large urban centres tend to pay more in taxes, which in turn provides some basis for why these queer communities tend to have a higher tax rate.

## Is there a correlation between the number of gay bars in a given neighbourhood and pride parade activity?
Our leading hypothesis for this research question is that if a neighbourhood has more gay bars, then it will be more likely to be traversed by a pride parade at some point during the year. We can illustrate the relationship between these two variables like so:

![Bar graph illustrating the relationship between gay bars and pride parades](analysis/images/graphs/bars-parades1.png)
![Bar graph illustrating the relationship between gay bars and pride parades](analysis/images/graphs/bars-parades2.png)

Alternatively, we can approach the data from a more quantitative perspective and find that:

![Bar graph illustrating the relationship between gay bars and pride parades](analysis/images/graphs/bars-parades-decomposition.png)

The data seems to suggest that our hypothesis should be rejected. Against our expectations, it appears that more gay bars are located in regions pride parades don't pass through. One possible explanation is that pride parade organizers tend to focus their effort on bringing their parades into communities that have a lower presence of queer residents overall. It's also possible that this correlation is insufficiently representative due to the fact that it exists mostly in regions with more than 10 bars. A third and final explanation would be to cite the sampling bias again. Although this isn't as extreme as was the case with the previous research question, it is nonetheless substantial enough to merit consideration.

## Conclusion

Over the last semester, we have analyzed data from numerous sources to find answers to four geographic questions about the queer community. Firstly, we wanted to understand whether or not queer communities tend to concentrate in space, and found that neighbourhoods with a higher density of gay and lesbian residents tend to be close to other neighbourhoods with a higher density, clustering in city centres, such that there's typically a geographical peak in queerness around the middle of each city. We used similar methods of analysis to study the political alignment of residents of queer neighbourhoods and found that across the country, neighbourhoods with more queer people tend to vote more democrat. Thirdly, we asked if there was a meaningful difference in the amount of money queer people pay in taxes versus non-queer people, and found that in general, suburban queer people tend to pay higher taxes. Finally we sought to determine if there is a correlation between the number of gay bars in a region and whether or not a pride parade passes through, and learned that a high number of gay bars in a given neighbourhood doesn't imply that that a pride parade is more likely to visit it.

In a world increasingly dominated by data-driven decision-making, minority communities, being already underrepresented, are particularly at risk of being further marginalized. While there are numerous risks associated with collecting and publishing data on these groups, it is equally important to ensure queer people are present and included. Answering questions about social issues regarding the queer community is greatly complicated by the fact that we are systematically excluded from the discussion, and considerably more effort is necessary to eliminate the systematic bias disabling queer representation.

## Footnotes

[^1]: Source: <https://www.cbc.ca/radio/unreserved/how-the-indian-act-continues-to-impact-the-lives-of-first-nation-people-1.5614187>

[^2]: Source: <https://pudding.cool/2018/06/gayborhoods/>

[^3]: The title of this in and of itself makes a very clear statement about how unrepresentative their project is.

[^4]: The Kinsey scale is a seven point scale measuring a person's attraction to people of the same sex. For further reading, see: <https://en.wikipedia.org/wiki/Kinsey_scale>