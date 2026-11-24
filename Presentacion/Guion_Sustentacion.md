# Guion de la sustentación (inglés)

Presentación ejecutiva de 15 minutos. El mismo texto está en las notas del expositor de `Final_Presentation_NYC.pptx`.

| Orden | Expositor | Diapositivas |
| :---: | :--- | :---: |
| 1 | Andrés | 1–5 |
| 2 | Iván | 6–10 |
| 3 | Julián | 11–15 |
| 4 | Luis | 16–19 |

## Andrés (diapositivas 1–5)

**1. Crime and Road Safety in New York City**

Good morning, everyone. We are Andrés, Luis, Iván and Julián. The State of New York hired our team as data consultants to improve two indicators: the number of arrests and the number of traffic crashes. We worked with open data from the city, on a Spark cluster with HDFS, and we followed CRISP-DM from start to finish. In the next fifteen minutes we will show you the data, the answers to our eight business questions, the models we built, and a clear action plan.

**2. Fewer arrests and fewer road deaths in New York City**

We turned the two indicators into three goals, one for each agency that makes the decisions. For the NYPD, we look at when, where and who is arrested, so they can place resources better. For the Department of Transportation and its Vision Zero program, we look at where, when and why people die on the roads. For NYC Opportunity, we add the social side: poverty, jobs, education, health and weather. Every question and every model today connects to one of these three goals.

**3. Five data sources processed on a Spark cluster with HDFS**

We used five data sources. Three come from NYC Open Data: arrests, traffic crashes and the poverty survey. The crash table is the big one, with 2.3 million records. We used the Crashes table instead of the Vehicles table because it has one row per crash, which is the unit we need. The other two sources are our bonus work: cancer rates collected by web scraping, and daily weather from an API. All the data was loaded into HDFS and processed on the Spark cluster.

**4. Four filters and six transformations in PySpark**

Before any analysis, we cleaned the data with four filters and six transformations in PySpark. We removed only 268 crashes: corrupted rows, negative victim counts, or totals that did not add up. We filled 1,394 missing arrest severities with the most common value for each offense code. Crashes without valid coordinates were not deleted. We kept them, and left them out only of the map-based analyses. The most important step is T5, because it lets us compare boroughs per resident, not just by raw count.

**5. Per resident, the Bronx leads in arrests, not Brooklyn**

That leads to our first key finding. If you count arrests, Brooklyn is first, and the budget would go there. But Brooklyn is also the biggest borough. When we divide by population, using the survey weights from the poverty data, the Bronx has the highest arrest rate per resident. The Bronx is also the borough with the most poverty and unemployment. Manhattan comes second because many people work or visit there but do not live there. Now Iván will take you through our business questions.

## Iván (diapositivas 6–10)

**6. 53% of felonies happen between Tuesday and Thursday**

Thank you, Andrés. Our first question was about felonies across the week. The arrest records have a date but no time, so we looked at the weekly cycle. In all five boroughs, between 52.6 and 53.6 percent of felonies happen from Tuesday to Thursday. The peak day also moves: Tuesday in the Bronx, Wednesday in Brooklyn and Queens, and Thursday in Manhattan and Staten Island. For the NYPD, this means tactical squads can be scheduled in a staggered way, borough by borough.

**7. Adjusted for population, more poverty goes with more misdemeanors**

Our second question asked if poverty is linked to minor crimes, or misdemeanors. With raw counts the correlation is weak, 0.39, because Brooklyn is so large. When we use the rate per resident, it rises to 0.60, and the Bronx is first in both poverty and misdemeanors. We want to be honest here: with only five boroughs, the p-value is 0.28, so the result is not statistically significant. We use it as a signal that social prevention should go together with policing.

**8. Driver inattention and failure to yield cause most crashes with pedestrians and cyclists**

Question three is about pedestrians and cyclists, the focus of Vision Zero. We took the crashes where at least one pedestrian or cyclist was hurt or killed, and counted each factor and vehicle type once per crash. Two driver behaviors explain most cases: inattention and failure to yield. SUVs are involved more often than sedans. And one number stands out: almost half of all road deaths are pedestrians. For the DOT, this points to leading pedestrian signals and safer crossings.

**9. Road death rates do not follow unemployment or education**

Question four asked if unemployment or education explain road deaths. The answer is no, at least at the borough level. Queens, the Bronx and Manhattan have almost the same fatality rate, around 1.37 per 100,000 residents. But their profiles are opposite: the Bronx has twice Manhattan’s unemployment, and Manhattan has almost three times the share of college graduates. What matters more is exposure: freight corridors, highways and heavy foot traffic. So for the next questions, we moved from boroughs to small grid cells.

**10. Five grid cells under 1 km² concentrate both arrests and road deaths**

For question five, we divided the city into grid cells of about one square kilometer. In the cells that had both arrests and fatal crashes, we built an index from the two percentiles. Two zones stand out. The first is the central Bronx corridor around Tremont and the Cross Bronx Expressway, with 1,584 arrests and two fatal crashes. The second is East Brooklyn, around Brownsville. These are exact places where one investment in lighting, crossings and patrols helps both public safety and road safety. Julián will continue with the last two questions and the models.

## Julián (diapositivas 11–15)

**11. The most common profile is the same at all three severity levels**

Thank you, Iván. Question six looked at the profile of people arrested at each severity level. We found the most common combination of age, sex and recorded race. It is the same for felonies, misdemeanors and violations: men aged 25 to 44, recorded as Black. The difference is in young people, who make up a larger share of felonies. We want to be careful here: this describes who the NYPD arrests, not how much crime a group commits. That is why we link it to prevention and youth jobs, not to more enforcement.

**12. Manhattan has crashes all day; Staten Island mostly at commute times**

Question seven compared crash timing in a commercial borough and a residential one. We normalized the hourly curve inside each borough. Staten Island follows a commuter pattern: a peak in the morning, another when school and work end, and very little at night. Manhattan stays high all afternoon, peaks at 4 p.m., and has much more night activity because of nightlife, taxis and deliveries. So signal timing and night enforcement should be planned differently in each borough.

**13. A split by year, to test the model the way it would be used**

Now the models. The label is whether a crash had at least one death. We trained on 2024, tuned on 2025, and tested only once on the first half of 2026. This is how the model would work in real life: it learns from the past and predicts the future. We checked correlations in Spark, and no pair of variables passed 0.90, so we did not need to drop any. Fatal crashes are only 0.25 percent of the data, so we gave them a much larger weight. We also removed injuries and deaths from the inputs, because they are the outcome we want to predict.

**14. Logistic regression finds half of the fatal crashes, with many false alarms**

We chose logistic regression because it supports class weights, controls overfitting with regularization, and its coefficients are easy to explain to a city agency. On the test set it finds 50 of the 102 fatal crashes, but with more than 16,000 false alarms, and its AUROC of 0.54 is close to chance. Its real value is the explanation, which answers question eight: vehicle type and contributing factor matter most, and lighting defects raise the risk of a fatal crash.

**15. Two types of area: 351 high-risk cells and 348 outer cells**

Grid cells have no risk label, so we used K-Means to find profiles. We tested k from 2 to 4, and two groups gave the best silhouette score. Group 0, in blue, is one connected area across Manhattan, central Brooklyn, the South Bronx and Queens. It has ten times more arrests per cell, and twice the share of crashes with pedestrians or cyclists. Group 1, in orange, covers the outer areas and Staten Island. This tells the City where to intervene strongly and where to focus on prevention. Luis will now present our bonus work and our recommendations.

## Luis (diapositivas 16–19)

**16. A neural network ranks risk better than the linear model**

Thank you, Julián. As our bonus, we built a deep learning model: a multilayer perceptron in Spark MLlib. We used exactly the same data, features and time split as the logistic regression, so the comparison is fair. We tested six configurations, and the best one has three hidden layers. On the test set it raises the AUROC from 0.54 to 0.60, and it cuts false alarms by almost two thirds, although it finds fewer fatal crashes. This shows there are interactions between time, place and vehicle that a linear model misses. Still, the improvement is moderate.

**17. Health and weather: two outside sources joined in Spark**

These are the bonus sources from our first delivery, now part of the analysis. With web scraping, we collected cancer rates for 55 neighborhoods from a state Tableau dashboard. Staten Island has the highest rate, even though it has the lowest crime and crash rates. With a weather API, we joined 546 days of temperature and rain to our crash and arrest data. The practical finding is seasonal: hot days bring more injuries, so prevention campaigns should be stronger in the warm months.

**18. Four lines of action for the City**

We close CRISP-DM with deployment: an action plan where every recommendation has an owner and a number behind it. The most concrete one is the first: the two dual hotspots in the Bronx and Brooklyn, where one investment serves both safety and mobility. Second, staggered NYPD shifts from Tuesday to Thursday. Third, safer crossings for pedestrians. The last one is about data: without speed and lighting on the police crash form, no model will predict road deaths well.

**19. What the City should take away**

To close, three ideas. First, looking at rates per resident, not raw counts, changes where the money goes. Second, risk is not spread evenly: it has specific days, places and behaviors. Third, our MLlib models add value, but the limit is the data the police form collects today. Thank you for your attention. We are happy to take your questions.
