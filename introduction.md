# Introduction to Elastic Stack

<img src="https://raw.githubusercontent.com/avwsolutions/DVT-Elastic-Introduction/master/content/banner.png" alt="introduction banner">

Below an overview of all Elastic components which help you to setup an Elastic Stack. Most familiar is using Elastic-Logstash-Kibana and additional Beats agents. if the customer buys support additional features can be added such as Security (Shield), Monitoring (Marvel, Alerting (Watcher) and Reporting. Other nice kick-ass features for Platinum and Enterprise are Graph and Machine Learning (Prelert).

<img src="https://raw.githubusercontent.com/avwsolutions/DVT-Elastic-Introduction/master/content/elastic_stack.png" alt="Elastic stack">

- Elasticsearch ; is our lucene search enabled document store, for indexing and analysing our data.
- Logstash ; is our swiss knife for advanced indexing to Elasticsearch and complex ingesting of data sources like data queues.
- Kibana ; is the key user interface component, which brings data & visualisations together on one or more beautiful dashboards.
- Beats ; is the family name for many lightweight data shippers like MetricBeat, FileBeat, WinlogBeat, PacketBeat and HeartBeat. For diehards we have LibBeat to brew your own GoLang.
- Optional X-Pack ; Adds amazing features like Security, Monitoring, Reporting, Graph or even Machine Learning capabilities.
- Optional Elastic Cloud ; This is the hosted variant for currently Elasticsearch and Kibana. New release will include Logstash.




