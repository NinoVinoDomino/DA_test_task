# DA_test_task
DA test task for segmentation analysis for the investment product

# Task description 
The company has launched an advertising campaign for a banking investment product. The campaign
ended, the results were received, but the customer became interested in which
competing banks new customers used before opening a brokerage account with MTS.
An analysis is required on the impact of the applications used on the interest in the
MTS Investment product.
The target event for our product is an application to open a brokerage
account after communication, it becomes a client of MTS Investments.

# Data structure
Data is given in 2 csv files with the following structure:
# Host table
Contains information about subscriber traffic for each day.
The msisdn- request_dt- host composite key  
• msisdn - subscriber's number  
• request_dt - date of access to the host  
• host - the name of the application host  
• traffic_volume - the amount of data uploaded by the subscriber  
# Communication table  
A table with data on advertising communications   
The msisdn- event_dt composite key  
• msisdn - subscriber's number  
• event_dt - date of the event  
• application_flg - flag: is the brokerage account open after communication  

# 1 task 
Create a target showcase with the following indicators:
• msisdn is the subscriber's number
• event_dt - date of the event
• application_flg - flag: is the brokerage account open after communication
• host – the name of the application host
• sum_traffic_volume - total traffic for the last 14 days before the event date

# Soluction
In short, the key points for the target showcase are: 
df_merged = pd.merge(df_2, df_1, left_on=['msisdn'], right_on=['msisdn'], how='left') #join tables
df_merged['day_space'] = (df_merged['event_dt']-df_merged['request_dt']).dt.days #create timespace column
ddf = df_merged[(df_merged['day_space'] < 15)&(df_merged['day_space'] >=0)] #filter dataset to remove events with timespace more than 14 days (as said in the task requirements)
report = report.groupby(['msisdn', 'event_dt', 'application_flg', 'host'], as_index=False)['traffic_volume'].sum() #calculate the traffic sum needed. Here group by with all the columns needed for the final showcase.

# Result
![image](https://github.com/NinoVinoDomino/DA_test_task/assets/98032823/fa789655-af48-4a0d-9e1c-66923ac120d0)

# 2 task
Analyze the resulting target showcase. 
## What bank applications did new MTS Investment clients use?
  ![image](https://github.com/NinoVinoDomino/DA_test_task/assets/98032823/9330799f-bd24-4cb8-83a9-ce218e864e31)
episode
## Analysis:
- Based on the constructed histogram, it can be concluded that most often (with a 2.5-fold separation from the previous bank) users yellowbank.invest.ru We turned to MTS Investments. At the same time, users of the same bank, but who did not use the "Investments" product, were more reluctant to come to MTS Investments (the lowest indicator). It is likely that either these users had certain needs that partially or completely close the MTS Investment offers, or they considered the transition profitable for themselves because, for example, attractive input offers. Here it is worth clarifying whether there were any for users yellowbank.invest.ru special conditions have been provided for the transfer to MTS Investments.
- The second one turned out to be greenbank.ru . Since there is no evidence that users of this bank have previously used the Investment product, it is difficult to say whether this was an appeal to MTS Investments for the sake of transition or whether they decided to try such a product for the first time for themselves, but chose not an offer from greenbank.ru , and MTS.
- Users redbank.ru and redbank.invest.ru We did not contact MTS so often, regarding the leaders. However, it is interesting to note that those who had not previously used the product "Investments" in their red bank, but eventually decided to try it at MTS turned out to be more than those who had already used investments in their red bank. From this, it can be concluded that users redbank.invest.ru they see less benefit for themselves in switching to another bank.

## Which application are the subscribers most interested in MTS Investments using?
  ![image](https://github.com/NinoVinoDomino/DA_test_task/assets/98032823/63f39eb7-a363-4c1c-958b-630264a404c4)
## Analysis:
- Despite the fact that, based on the previous paragraph, the users who were most interested in switching to MTS Investments turned out to be yellowbank.invest.ru it can be seen that the most interested users were redbank.ru . They most often switched to a targeted action: an application to open a brokerage account after communication.
At the same time, from the users who applied redbank.invest.ru after the communication, no one created an application to open a brokerage account. Again, we see that these users do not see benefits for themselves in the MTS product.
- User Conversion rate yellowbank.invest.ru I took the second place, and they took the first place in terms of appeals. It can be concluded that some customers changed their mind about opening an account after learning the full terms of use of the product.
- Users greenbank.ru and yellowbank.ru they have a conversion rate at the same level, but almost 2 times lower than that of the leaders. It's worth considering here that these customers probably haven't used investments before (at least they came from just banks), which means their confidence threshold and desire to start investing may be potential reasons for such conversion rates.

## What other conclusions can be drawn from the target showcase?
  
Let's estimate the dynamics of data received from subscribers by day for a given period using a summary table and heatmap.

![image](https://github.com/NinoVinoDomino/DA_test_task/assets/98032823/ae9268f0-41dc-457d-a241-92bbc6a557ef)

# Analysis:
- Despite the fact that the leader turned out to be yellowbank.invest.ru We see that the consistently high dynamics of user traffic was greenbank.ru during the entire time period. Users yellowbank.invest.ru They also came steadily, but an obvious jump can be observed after the event on January 20.
- We can observe the most heterogeneous activity among users redbank.invest.ru On January 25, there was a peak in incoming traffic (a light pink rectangle is clearly visible on the heatmap), this invention brought more than half of all traffic for a given period of time.
- Answering the question of which event gave the most leads, it is worth noting that peaks for customers of different banks are observed on different days. The events were probably targeted at the uncovered needs of each of the groups. January 25th for yellowbank.ru and redbank.invest.ru , 22nd for redbank.ru . Peak greenbank.ru it fell on the last day available for analysis on January 31. For yellowbank.invest.ru It is impossible to give a clear answer, the activity was uniformly stable.

# Conclusion

Answering the question of how the applications used affect the interest in the MTS Investment product, the following can be noted:

- Clients using yellowbank.invest.ru Most often, they turned their attention to the MTS Investment product (the volume of requests during the study period). This means that the events held by MTS Investments respond well to the audience of this bank and can potentially cover some of their needs. Here it is necessary to analyze in more detail whether there were special promotions / offers for the clientele of this bank when switching to the MTS product.
The target event for our product is an application to open a brokerage account after communication. Customers have the highest rate for this item redbank.ru and yellowbank.invest.ru banks. We see that almost half (43%) of the customers who have completed the target action are customers redbank.ru . It can be assumed that the creation of a new event, specifically aimed at this audience, will bring even more traffic. It is also worth noting that customers redbank.ru and yellowbank.invest.ru They are similar groups of clients, as they showed high activity after the events.
- Users greenbank.ru they showed average activity both when contacting and when performing a target action. The audience that consistently comes after the events shows that the MTS offer is attractive to them. Clients greenbank.ru an obvious potential audience growth point that is worth paying attention to.
- Clients redbank.invest.ru they turned out to be outsiders. Despite a little interaction after the event, none of the clients opened a brokerage account. From this, it can be concluded that the MTS Investment offer is unattractive for them. Here you can improve the situation by introducing a welcome bonus for this category of customers when switching to MTS Investments. However, first you need to calculate whether the incoming users will pay back the costs of this promotion.


