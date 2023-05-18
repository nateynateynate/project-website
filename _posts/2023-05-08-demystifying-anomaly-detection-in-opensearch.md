---
date: 2023-05-05T21:42:27.337Z
meta_description: An educational example of working with the anomaly detection
  feature in OpenSearch.
meta_keywords: opensearch, anomaly detection, open source
feature_image: /assets/media/blog-images/demystifying-anomaly-detection-3-.png
authors:
  - nateboot
layout: post
title: Demystifying Anomaly Detection in OpenSearch
categories:
  - technical-post
---
### Anomaly Detection, what have you done for me lately?

Anomaly detection is used in all aspects of our life. In fact, our brains are hard wired to identify patterns in data, and anomalies in that data. It's something we do subconsciously and without any visible effort. Our only restraint? Our limited human memory. Using modern technology and an engine like OpenSearch you get out it something with a much longer and reliable memory than a human. Especially yours truly. Follow along to get a quick example workflow using anomaly detection and to learn of the great advantages you get using the Anomaly Detection panel in OpenSearch. 

### What is an anomaly, and are you sure?

You might say it's anything we deem out of the ordinary.  Defining "out of the ordinary" comes from being **familiar and cognizant** about the data we're examining. Similarly, part of configuring your anomaly detection model in OpenSearch comes from knowing what you're looking for. I'll share the details of the things we have to consider in a tick. 

Here's what I'm hoping we can build together!

![anomaly detection workflow diagram](/assets/media/blog-images/demystifying-anomaly-detection-3-.png "anomaly detection workflow diagram")

### Ingestion and Verification

I'll be using Calyptia's [fluentbit ](https://fluentbit.io/)for ingesting data from [systemd](https://systemd.io) and putting it into an OpenSearch index. Here's the minimum configuration needed to make that happen. 

```
[INPUT]

    # Use the input plugin 'systemd'
    name systemd
    # Give these entries a tag.
    tag local.systemd.*
    # I had problems when certain fields began with an underscore, so I had to make use of this. 
    strip_underscores On
    # Read the entirety of the file on start or just start tailing it.
    read_from_tail On


[OUTPUT]
    # Use the output plugin 'opensearch'
    name opensearch
    # Enable tls
    tls On
    # If you're using a self-signed certificate, you'll want to turn off tls.verify
    tls.verify Off
    # We stopped using document types some time ago. 
    suppress_type_name On
    # Match input lines with tags starting with local.
    match local.*
    # I've got OpenSearch running on the same machine as fluent-bit. 
    host localhost
    # Ingesting on port 9200, the default. 
    port 9200
    # Deliver to the index named systemd
    index systemd
    # I beg your humble forgiveness for hard-coding credentials. 
    http_user admin
    http_passwd admin
```

### Ensure you have data, then create a detector.

Once you've started fluent-bit (it's available for many Linux distributions, so please use the service manager or binary appropriate for your environs) you can check to see if your data is making it to OpenSearch by using the 'discover' tab. 

![opensearch discovery tab](/assets/media/blog-images/discover_tab.png "opensearch discovery tab"){: width="450" }

### Choose your anomaly detector features.

I am of the opinion that calling it a feature is incorrect. You're just picking the indices that you want to check for anomalies. You can pick up to five. The anomaly detector will work off of an aggregation of each of your features, so for each of them you can choose between a few aggregation methods. 

![anomaly detection features](/assets/media/blog-images/anomaly_detection_features.png "anomaly detection features"){: width="450" }

### Decide on a schedule.

Anomaly detectors are not something that run constantly in your OpenSearch environment. You'll have to choose a **detector interval** and **delay.** It will then be a scheduled job, which will launch every **detector interval** minutes to analyze the last **detector interval** minutes worth of data to check for an anomaly. 

Think of the **delay** as like an offset. Once the detector interval passes, the detector will wait another **delay** minutes, for the purposes of accommodating an ingestion pipeline that might have lag in it. This is a matter of accuracy - if your cluster is large, and there are many pieces in your ingestion pipeline, odds are that it will take a little bit more than ten minutes to receive exactly ten minutes worth of log entries. Accommodate for this pipeline induced latency with a **delay.**

![anomaly detection window](/assets/media/blog-images/anomaly_detection_time_interval.png "anomaly detection window"){: width="450" }

### Categorical Fields

Practically, categorical fields provide you a grid heatmap of your anomalies, with each axis representing the two fields you've chosen. For my example, I'm really looking to organize my anomalies by the \`hostname\` and \`systemd_unit\`. This will show me a grid of anomalies organized by the hostname (as reported by systemd) as well as the unit (service). Here's what you can expect from the UI: 

![anomaly detection categorical fields](/assets/media/blog-images/anomaly_detection_categorical_fields.png "categorical fields UI"){: width="450" }

You can't change these after you create your detector. You should have a pretty good idea on how you want to categorize your anomalies before you make any permanent decisions. **Be familiar and cognizant** about your data beforehand. 

### Historical Analysis

### Revisiting the Panel

### Alerting