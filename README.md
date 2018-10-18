# stream-solr: 
https://www.youtube.com/watch?v=3u4MDH9P_9A
[Activate 2018](https://activate2018.sched.com/event/FkMi/building-analytics-applications-with-streaming-expressions-in-apache-solr)
[Fifth Elephant 2018](https://fifthelephant.talkfunnel.com/2018/33-building-analytics-application-with-streaming-expr)

https://github.com/lucidworks/zeppelin-solr

1. Load the Zepellin Solr dashboard config json.
2. Download weekly_data.zip, unzip, use Solr BACKUP API to load it into Solr.
3. Create local mysql database: cost_db and table: cost.

| campaign_id_s | currency_cost_i |
| ------------- | --------------- |
| cmp-01  | 6600  |
| cmp-02  | 5840 |
| cmp-03  | 8400 |

Sample streaming expression:

innerJoin(
select(
facet(weekly_data,
q="org_id_s:org-01",
buckets="campaign_id_s",
bucketSorts="campaign_id_s asc",
bucketSizeLimit=100,
sum(conversations_i),
sum(impressions_i),
sum(clicks_i)),
campaign_id_s as campaign_id_s, 
sum(conversations_i) as aggr_conv,sum(impressions_i)
as aggr_impr, sum(clicks_i) as aggr_clicks),
jdbc(connection="jdbc:mysql://localhost/cost_db?
user=root&password=root",
sql="SELECT campaign_id_s,currency_cost_i FROM cost",
sort="campaign_id_s asc",
driver="com.mysql.jdbc.Driver"),
on="campaign_id_s")


