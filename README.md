<h1 align = center> SPACEX Falcon 9 Analysis </h1>

## Overview

Space X advertises Falcon 9 rocket launches on its website with a cost of 62 million dollars; other providers cost upward of 165 million dollars each, much of the savings is because Space X can reuse the first stage. Therefore, if we can determine if the first stage will land, we can determine the cost of a launch. This information can be used if an alternate company wants to bid against space X for a rocket launch. In this assignment, we will predict if the Falcon 9 first stage will land successfully. 

## Task

Firstly, we will need to determine what factors determine if the rocket will land successfully?
- The interaction amongst various features that determine the success rate of a successful landing.
- What operating conditions needs to be in place to ensure a successful landing program.

The dataset we will be using to answer some of these questions can be seen [here](https://github.com/YaasirM/SPACEX_Falcon_9_Analysis/blob/main/assets/dataset/SPACEX_Falcon9_Analysis.csv)

## Flight Number vs Launch Site

After looking at the dataet, the first step was to try create some visualisations, to see if there were any connections between some of the columns. The first visualisation I created was a scatter graph depicting the flight number vs the launch site:

```python
# Firstly, I imported all the required packages and libraries to complete the visualisations
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns
# Then i used seaborn to create the scater graph for flight number vs launch site
sns.catplot(y="LaunchSite", x="FlightNumber", hue="Class", data=df, aspect = 5)
plt.xlabel("Flight Number",fontsize=20)
plt.ylabel("LaunchSite",fontsize=20)
plt.show()
```
![Flight Number vs Launch Site](https://github.com/YaasirM/SPACEX_Falcon_9_Analysis/blob/main/assets/images/Flight%20number%20vs%20Launch%20Site.png)

*Flight Number vs Launch Site*

From the scatter graph, we can see that the VAFB SLC 4E has the least number of Launch sites, whilst CCAFS SLC 40 has the most. We can also idenitfy that most of the flight numbers roughly between 30 and 40 are at KSC LC 39A, whilst majority of the rest (averaging per 10) is at CCAFS SLC 40.

## Payload vs Launch Site

The next visualisation was to create another scatter graph between payload and the launch site:

```python
sns.catplot(y="LaunchSite", x="PayloadMass", hue="Class", data=df, aspect = 5)
plt.xlabel("PayloadMass",fontsize=20)
plt.ylabel("LaunchSite",fontsize=20)
plt.show()
```
![Payload vs Launch Site](https://github.com/YaasirM/SPACEX_Falcon_9_Analysis/blob/main/assets/images/Payload%20vs%20Launch%20Site.png)

*Payload vs Launch Site*

For VAFB SLC 4E there is only one more Launchsite that has a heavier payload mass than 6000kg, whilst the other Launchsite's have relatively more rockets with a payload mass higher than 6000kg. Majority of both CCAFS SLC 40 and VAFB SLC 4E have majority of their rocket payload mass lower than 8000kg. KSC LC 39A has the highest number of rockets with a payload mass above 8000kg, however CCAFS SLC 40 has the most amount of launch site rockets as a whole.

## Success Rate vs Orbit Type

The next visualisation I created from the dataset was a bar chart utilising the success rate and the orbit type.

```python
df.groupby('Orbit')['Class'].mean().plot.bar()
```
![Success Rate vs Orbit Type](https://github.com/YaasirM/SPACEX_Falcon_9_Analysis/blob/main/assets/images/Success%20Rate%20vs%20Orbit%20Type.png)

*Success Rate vs Orbit Type*

From the graph we can see that only 4 Orbit types have a success rate of 100%, which are ES-L1, GEO, HEO and SSO. SO is the only Orbit type to have a 0% success rate, meaning it should be avoided if possible. The only other orbit types that share the same success rate (apart from 100%) are the PO and MEO orbit types, which both share a success rate of roughly 65%. Apart from SO, there are no other orbit types with a success rate lower than 50%.

## Flight Number vs Orbit Type
Here we will create another scatter graph between the flight number and the orbit type:

```python
sns.catplot(y="Orbit", x="FlightNumber", hue="Class", data=df, aspect = 5)
plt.xlabel("FlightNumber",fontsize=20)
plt.ylabel("Orbit",fontsize=20)
plt.show()
```

![Flight Number vs Orbit Type](https://github.com/YaasirM/SPACEX_Falcon_9_Analysis/blob/main/assets/images/Flight%20Number%20vs%20Orbit%20Type.png)

*Flight Number vs Orbit Type*

The only other orbit types that share the same success rate (apart from 100%) are the PO and MEO orbit types, which both share a success rate of roughly 65%. Apart from SO, there are no other orbit types with a success rate lower than 50%.

## Payload vs Orbit Type 
For the last visualisation, we will be creating a scatter graph between the payload and the orbit type:

```python
sns.catplot(y="Orbit", x="PayloadMass", hue="Class", data=df, aspect = 5)
plt.xlabel("PayloadMass",fontsize=20)
plt.ylabel("Orbit",fontsize=20)
plt.show()
```

![Payload vs Orbit Type](https://github.com/YaasirM/SPACEX_Falcon_9_Analysis/blob/main/assets/images/Payload%20vs%20Orbit%20Type.png)

*Payload vs Orbit Type*

Majority of both CCAFS SLC 40 and VAFB SLC 4E have majority of their rocket payload mass lower than 8000kg. KSC LC 39A has the highest number of rockets with a payload mass above 8000kg, however CCAFS SLC 40 has the most amount of launch site rockets as a whole.

By now, we should obtain some preliminary insights about how each important variable would affect the success rate. Now that we also understand which areas might not be necessary for the analysis, we have repurposed the dataset to only include areas of desired interest. The updated dataset can be found [here](https://github.com/YaasirM/SPACEX_Falcon_9_Analysis/blob/main/assets/dataset/SPACEX_Falcon9_Analysis%202.csv)

## SQL Questions
Now that the dataset has been repurposed, we will utilise SQL and ask the dataset some questions to discover more about Falcon 9.

### Question 1
Display the names of the unique launch sites in the space mission.

```sql
SELECT
 DISTINCT LAUNCH_SITE
FROM
 SPACEXTABLE
```
![SQL Q1](https://github.com/YaasirM/SPACEX_Falcon_9_Analysis/blob/main/assets/images/SQL%20Q1.png)


This query identifies the distinct launch sites used for Falcon 9 rocket launches. The variety of launch sites—like CCAFS LC-40, VAFB SLC-4E, and KSC LC-39A—could indicate different geographic and operational constraints impacting each mission's launch success.

### Question 2
Display 5 records where launch sites begin with the string 'CCA'.

```sql
SELECT
 LAUNCH_SITE
FROM
 SPACEXTABLE
WHERE
 Launch_SITE
LIKE
 'CCA%'
LIMIT
 5
```

![SQL Q2](https://github.com/YaasirM/SPACEX_Falcon_9_Analysis/blob/main/assets/images/SQl%20Q2.png)


This query narrows down launches from sites starting with 'CCA', likely referring to Cape Canaveral launch sites. Repeated entries for CCAFS LC-40 suggest frequent use of this site for Falcon 9 missions. Repeated successful landings at specific launch sites can give us insight into which sites are more suited for recovery attempts. If a launch site consistently achieves successful landings, this could be an indicator for future predictions at this site.

### Question 3
Display the total payload mass carried by boosters launched by NASA (CRS).

```sql
SELECT
 sum(payload_mass__kg_) AS sum
FROM
 SPACEXTABLE
WHERE
 customer = 'NASA (CRS)'
```
![SQL Q3](https://github.com/YaasirM/SPACEX_Falcon_9_Analysis/blob/main/assets/images/SQL%20Q3.png)

NASA (CRS) missions have a total payload mass of 45,596 kg. Since payload size can affect the rocket's performance, a larger payload could strain the rocket’s ability to return its first stage successfully. Larger payload masses may reduce the amount of fuel available for stage recovery. If the payload is large, the Falcon 9 may not have enough fuel left after delivering it to orbit, decreasing the likelihood of a successful landing.

### Question 4
Display average payload mass carried by booster version F9 v1.1.

```sql
SELECT
 AVG(payload_mass__kg_) AS Average
FROM
 SPACEXTABLE
WHERE
 booster_version = 'F9 v1.1'
```
![SQL Q4](https://github.com/YaasirM/SPACEX_Falcon_9_Analysis/blob/main/assets/images/SQL%20Q4.png)

The average payload mass for the Falcon 9 booster version F9 v1.1 is 2,928.4 kg. This suggests a moderate payload size, where the first stage might still have a reasonable chance of landing successfully due to the balance between payload and fuel. A smaller payload mass generally increases the likelihood of a successful first-stage landing, as there is less weight for the rocket to carry, leaving more fuel for the landing maneuver.

### Question 5
List the date when the first succesful landing outcome in ground pad was acheived.

```sql
SELECT
 min(Date)
FROM
 SPACEXTABLE
WHERE
 Landing_Outcome = 'Success (ground pad)' 
```

![SQL Q5](https://github.com/YaasirM/SPACEX_Falcon_9_Analysis/blob/main/assets/images/SQL%20Q5.png)

The first successful ground pad landing occurred on 2015-12-22. This provides a benchmark for Falcon 9’s ability to land its first stage on land. This milestone suggests that SpaceX’s technology has evolved over time, and this historical success can serve as a reference for predicting future landings, especially for missions targeting ground recovery.

### Question 6
List the names of the boosters which have success in drone ship and have payload mass greater than 4000 but less than 6000.

```sql
SELECT
 Booster_Version
FROM
 SPACEXTABLE
WHERE
 Landing_Outcome = 'Success (drone ship)'
 AND
 PAYLOAD_MASS__KG_ BETWEEN 4000
  AND
   6000
```
![SQL Q6](https://github.com/YaasirM/SPACEX_Falcon_9_Analysis/blob/main/assets/images/SQL%20Q6.png)

Successful drone ship landings occurred for boosters with payloads ranging from 4000 to 6000 kg, suggesting that such payload sizes are conducive to successful recovery on the drone ship. Drone ship landings are more likely when the payload is within this range. Therefore, the first stage may have enough fuel to reach the drone ship without the excessive weight from larger payloads.

### Question 7
List the total number of successful and failure mission outcomes.

```sql
SELECT
 Mission_Outcome, COUNT(*)
FROM
 SPACEXTABLE
GROUP BY
 Mission_Outcome
```
![SQL Q7](https://github.com/YaasirM/SPACEX_Falcon_9_Analysis/blob/main/assets/images/SQL%20Q7.png)

Most missions ended successfully (98), while a few had unclear payload statuses or failures. This highlights the high success rate of SpaceX missions. A high success rate might suggest that other factors, such as mission complexity and landing site readiness, play a larger role than just the payload size or type of mission.

### Question 8
List the names of the booster_versions which have carried the maximum payload mass.

```sql
SELECT Booster_Version, Max
FROM (
    SELECT Booster_Version, MAX(PAYLOAD_MASS__KG_) AS Max
    FROM SPACEXTABLE
    GROUP BY Booster_Version
    ORDER BY Max DESC
    LIMIT 10
) 
```

![SQL Q8](https://github.com/YaasirM/SPACEX_Falcon_9_Analysis/blob/main/assets/images/SQL%20Q8.png)


The F9 B5 B1060 series boosters carry the maximum payload mass of 15,600 kg, which could indicate the upper limits of Falcon 9's payload capacity. Heavy payloads like these are less likely to be able to land successfully. With such high payloads, there may be insufficient fuel for a first-stage recovery, especially on long-distance missions requiring more fuel for orbital insertion.

### Question 9
List the records which will display the month names, failure landing_outcomes in drone ship ,booster versions, launch_site for the months in year 2015.

```sql
SELECT 
    SUBSTR(Date, 6, 2) AS Month, 
    Landing_Outcome, 
    Booster_Version, 
    Launch_Site 
FROM SPACEXTABLE 
WHERE SUBSTR(Date, 1, 4) = '2015' 
    AND Landing_Outcome = 'Failure (drone ship)';
```

![SQL Q9](https://github.com/YaasirM/SPACEX_Falcon_9_Analysis/blob/main/assets/images/SQL%20Q9.png)


The failures of drone ship landings occurred in April and October, indicating that even successful programs like Falcon 9 have challenges with landings at certain times. These failures can be indicative of weather conditions, sea conditions, or other environmental factors that affect drone ship landings. Recognizing these trends can help predict when conditions are more likely to cause landing failures.

### Question 10
Find the count of landing outcomes (such as Failure (drone ship) or Success (ground pad)) between the date 2010-06-04 and 2017-03-20 (completed in descending order).

```sql
SELECT 
    COUNT(Landing_Outcome) AS count, 
    Landing_Outcome 
FROM SPACEXTABLE 
WHERE Date BETWEEN '2010-06-04' AND '2017-03-20'
GROUP BY Landing_Outcome 
ORDER BY count DESC;
```

![SQL Q10](https://github.com/YaasirM/SPACEX_Falcon_9_Analysis/blob/main/assets/images/SQL%20Q10.png)

The 'No attempt' category has the highest count (21), followed by several other landing outcomes. This suggests that while there were many successful landings, there were also many missions where no attempt was made. Missions that do not attempt landings could skew predictions, especially when the goal is to predict success or failure in landing. These records might indicate test flights or missions where stage recovery was not a priority.

## Conclusion
By considering all of these factors—launch site, payload mass, booster type, and external conditions—SpaceX can continue to refine their prediction models for successful first-stage landings:
Launch Site Influence: Launch sites closer to the ocean or those with less infrastructure for recovery might see fewer successful landings, especially if drone ships are not available.
- **Payload Mass:** Heavier payloads tend to reduce the likelihood of a successful first-stage landing, as the rocket has to carry more weight, reducing the fuel available for recovery. A payload in the range of 4000-6000 kg has been found to correlate with successful drone ship landings.
- **Booster Version Impact:** Boosters like the F9 B5 B1060 with very high payload capacities are more likely to have issues with successful landings due to fuel limitations.
- **Historical Data:** The consistent increase in successful landings over time points to ongoing improvements in SpaceX’s technology and landing capabilities.
- **Environmental and External Factors:** Failed landings, particularly in 2015, suggest external factors (e.g., weather, sea conditions) can impact landing success, especially with drone ship landings.

Additionally, competitors could use similar predictive models to understand whether their missions could land successfully using different technologies or strategies, potentially positioning themselves in bids for launches against SpaceX.

## Jupyter Notebook
The analysis and modelling were performed using Python, SQL and Jupyter Notebook. You can find the complete notebooks [here](https://github.com/YaasirM/SPACEX_Falcon_9_Analysis/tree/main/assets/notebook)
