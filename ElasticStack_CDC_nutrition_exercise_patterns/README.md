### Using Elasticsearch & Kibana to Analyze Health Behavior Survey data from CDC

This example provides supplementary material to the Movember Data Dives - see [blog 1](https://www.elastic.co/blog/movember-data-dive-part-1) and [blog 2](https://www.elastic.co/blog/movember-data-dive-part-2)

In this example, we will analyze the 2013 [Behavioral Risk Factor Surveillance System] (http://www.cdc.gov/brfss/annual_data/annual_2013.html) data using the Elastic stack. Every year, the Centers for Disease Control and Prevention (CDC) conducts approximately 500,000 telephone surveys to collect data on a variety of personal health-related topics, such as nutrition, drinking habits, physical activity and health history. We analyzed this data to explore exercise and nutrition patterns for male respondents as a part of our Movember Data Dive. For additional commentary on the analysis and unearthed insights, refer to the accompanying blogs [here](https://www.elastic.co/blog/movember-data-dive-part-1) (for analysis of physical activity and exercise patterns) and [here](https://www.elastic.co/blog/movember-data-dive-part-2) (for analysis of eating and drinking patterns).

##### Version
Example has been tested in following versions:
- Elasticsearch 1.7.0
- Kibana 4.1.0

### Installation & Setup
* Follow the [Installation & Setup Guide](https://github.com/elastic/examples/blob/master/Installation%20and%20Setup.md) to install and test the ELK stack (*you can skip this step if you already have a working installation of the ELK stack*)

* Run Elasticsearch & Kibana
  ```shell
  <path_to_elasticsearch_root_dir>/bin/elasticsearch
  <path_to_kibana_root_dir>/bin/kibana
  ```

* Check that Elasticsearch and Kibana are up and running.
  - Open `localhost:9200` in web browser -- should return status code 200
  - Open `localhost:5601` in web browser -- should display Kibana UI.

  **Note:** By default, Elasticsearch runs on port 9200, and Kibana run on ports 5601. If you changed the default ports, change   the above calls to use appropriate ports.

### Download & Ingest Data

In this example, we ingest the data into Elasticsearch using the Elasticsearch Python client.
Follow the instructions in the  [ReadMe](https://github.com/elastic/examples/blob/master/ELK_CDC_nutrition_exercise_patterns/Ingest%20Scripts%20-%20Python/README.md) in the [Ingest Scripts - Python](https://github.com/elastic/examples/tree/master/ELK_CDC_nutrition_exercise_patterns/Ingest%20Scripts%20-%20Python) folder if you want to try this option.

#### Check data availability
Once the index is created, data will available in Elasticsearch. If all goes well, you should get a `count` response of `491773` when you run the following command.

  ```shell
  curl -XGET localhost:9200/brfss/_count -d '{
  	"query": {
  		"match_all": {}
  	}
  }'
  ```

#### Visualize Data in Kibana
* Access Kibana by going to `http://localhost:5601` in a web browser
* Connect Kibana to the `brfss` index in Elasticsearch
    * Click the **Settings** tab >> **Indices** tab >> **Create New**. Specify `brfss` as the index pattern name, select `Interview_Date` as the **Time-field name**, and click **Create** to define the index pattern. (Leave the **Use event times to create index names** box unchecked)
* Load sample dashboard into Kibana
    * Click the **Settings** tab >> **Objects** tab >> **Import**, and select `brfss_kibana_dashboard.json`
* Open dashboard(s)
    * Click on **Dashboard** tab and open either the `BRFSS: Nutrition` or `BRFSS: Fitness` dashboard. Voila! You should see one of the following dashboards. Happy Data Exploration!

![Kibana Dashboard Screenshot](https://cloud.githubusercontent.com/assets/5269751/11385430/a0e76804-92cd-11e5-99a7-0450c87ca375.png)
![Kibana Dashboard Screenshot](https://cloud.githubusercontent.com/assets/5269751/11385436/a542ed56-92cd-11e5-9f0a-8a243a55f054.png)

### We would love to hear from you!
If you run into issues running this example or have suggestions to improve it, please use Github issues to let us know. Have an easy fix? Submit a pull request. We will try our best to respond in a timely manner!

Have you created interesting examples using the ELK stack? Looking for a way to share your amazing work with the community? We would love to include your awesome work here. For more information on how to contribute, check out the **[Contribution](https://github.com/elastic/examples#contributing)** section!
