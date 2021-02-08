Kibana is a popular visualization tool for Elasticsearch. If you have enabled esLog in you .env file and wanted to view the logs in Elasticsearch these are the steps.

**1. Find your Elasticsearch version**
a. Go to your Elasticsearch Endpoint. (/protected/.env under "ESLOG_ENDPOINT")
b. Enter this link into your browser. eg. https://xxxxxxxxxxxxxxxxxxxx.ap-southeast-1.es.amazonaws.com

![Elasticsearch Version](https://firebasestorage.googleapis.com/v0/b/edmondtm-1d8ed.appspot.com/o/elasticsearchversion.png?alt=media&token=36a27df2-8921-49a4-81a8-6598ae7955a2)

**2. Download Kibana corresponding to your Elasticsearch version and Operating System**

[https://www.elastic.co/downloads/past-releases#elasticsearch](Download Kibana)

**3. Install**

**4. Configure Kibana**
For Linux and Mac, your configuration file should be at etc/kibana/kibana.yml
![Kibana Configuration](https://firebasestorage.googleapis.com/v0/b/edmondtm-1d8ed.appspot.com/o/kibana%20configuration.png?alt=media&token=2b15de9e-a2de-4a69-a4f9-ff72acf23d80)

[AWS Documentation](https://docs.aws.amazon.com/elasticsearch-service/latest/developerguide/es-kibana.html)

**5. Open http://localhost:5601 on your web browser.
![Kibana Screenshot](https://firebasestorage.googleapis.com/v0/b/edmondtm-1d8ed.appspot.com/o/kibana%20screenshot.png?alt=media&token=9a2724b9-b082-42be-9c3b-aa284f539324)




