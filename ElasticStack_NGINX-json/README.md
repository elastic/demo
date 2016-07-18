### Getting Started with Elastic Stack for NGINX (JSON) Logs
This **Getting Started with Elastic Stack** example provides sample files to ingest, analyze & visualize **NGINX access logs** using the Elastic Stack. This example uses JSON formatted version of Nginx logs. The NGINX `log format` entry used to generate these logs is shown in  Download section below.

##### WARNING: NGINX uses \xXX to encode some characters. The resulting "JSON" string is not valid (the JSON standard specifies \u00XX) and in Logstash you will get a json parse error for the invalid JSON string. It only happens when any variables e.g. $http_user_agent contain characters that NGINX will escape e.g. backslash or reverse solidus.

##### Version
Example has been tested in following versions:
- Elasticsearch 2.0
- Logstash 2.0
- Kibana 4.2.0

### Installation & Setup
* Follow the [Installation & Setup Guide](https://github.com/elastic/examples/blob/master/Installation%20and%20Setup.md) to install and test the Elastic Stack stack (*you can skip this step if you have a working installation of the Elastic Stack,*)

* Run Elasticsearch & Kibana
  ```
  <path_to_elasticsearch_root_dir>/bin/elasticsearch
  <path_to_kibana_root_dir>/bin/kibana
  ```

* Check that Elasticsearch and Kibana are up and running.
  - Open `localhost:9200` in web browser -- should return status code 200
  - Open `localhost:5601` in web browser -- should display Kibana UI.

  **Note:** By default, Elasticsearch runs on port 9200, and Kibana run on ports 5601. If you changed the default ports during/after installation, change the above calls to use appropriate ports.

### Download Example Files

Download the following files in this repo to a local directory:
- `nginx_json_logs` - sample JSON formatted NGINX logs**
- `nginx_json_logstash.conf` - Logstash config for ingesting data into Elasticsearch
- `nginx_json_template.json` - template for custom mapping of fields
- `nginx_json_kibana.json` - config file to load prebuilt Kibana dashboard

Unfortunately, Github does not provide a convenient one-click option to download entire contents of a subfolder in a repo. Use sample code provided below to download the required files to a local directory:

```shell
mkdir  nginx_json_ELK_Example
cd nginx_json_ELK_Example
wget https://raw.githubusercontent.com/elastic/examples/master/ELK_NGINX-json/nginx_json_logstash.conf
wget https://raw.githubusercontent.com/elastic/examples/master/ELK_NGINX-json/nginx_json_kibana.json
wget https://raw.githubusercontent.com/elastic/examples/master/ELK_NGINX-json/nginx_json_template.json
wget https://raw.githubusercontent.com/elastic/examples/master/ELK_NGINX-json/nginx_json_logs
```

** The JSON formatted NGINX logs used in this example were created with the following `log_format` entry in the `nginx.config` file.

```
log_format json_logstash '{ "time": "$time_local", '
                           '"remote_ip": "$remote_addr", '
                           '"remote_user": "$remote_user", '
                           '"request": "$request", '
                           '"response": "$status", '
                           '"bytes": "$body_bytes_sent", '
                           '"referrer": "$http_referer", '
                           '"agent": "$http_user_agent" }';
```

### Run Example
##### 1. Ingest data into Elasticsearch using Logstash
* Execute the following command to load sample logs into Elasticsearch.

```shell
cd nginx_json_ELK_Example
cat nginx_json_logs | <path_to_logstash_root_dir>/bin/logstash -f nginx_json_logstash.conf
```

 * Verify that data is successfully indexed into Elasticsearch

  Running `http://localhost:9200/nginx_json_elk_example/_count` should return a response a `"count":51462`

 **Note:** Included `nginx_json_logstash.conf` configuration file assumes that you are running Elasticsearch on the same host as     Logstash and have not changed the defaults. Modify the `host` and `cluster` settings in the `output { elasticsearch { ... } }`   section of nginx_json_logstash.conf, if needed.

##### 2. Visualize data in Kibana

* Access Kibana by going to `http://localhost:5601` in a web browser
* Connect Kibana to the `nginx_json_elk_example` index in Elasticsearch (auto-created in step 1)
    * Click the **Settings** tab >> **Indices** tab >> **Add New**. Specify `nginx_json_elk_example` as the index pattern name and click **Create** to define the index pattern
* Load sample dashboard into Kibana
    * Click the **Settings** tab >> **Objects** tab >> **Import**, and select `nginx_json_kibana.json`
<<<<<<< HEAD
* Open dashboard
    * Click on **Dashboard** tab and open `Sample Dashboard for Nginx Logs` dashboard

Voila! You should see the following dashboards. Enjoy!
![Kibana Dashboard Screenshot](https://cloud.githubusercontent.com/assets/5269751/9672317/7c5b763e-524e-11e5-949f-b8dad81bce8f.png)

### We would love your feedback!
If you found this example helpful and would like to see more such Getting Started examples for other standard formats, we would love would to hear from you. If you would like to contribute examples to this repo, we'd love that too!
