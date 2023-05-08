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

Anomaly detection is used in all aspects of our life. In fact, our brains are aptly hard wired to identify patterns in data, and anomalies in that data. It's something we do subconsciously and without any visible effort. Our only restraint? Our limited human memory. Using modern technology and an engine like OpenSearch, it may require a bit more effort, but what you get out of it is something with a much longer and reliable memory than a human. Especially yours truly. Follow along to get a quick example workflow using anomaly detection and to learn of the great advantages you get using the Anomaly Detection panel in OpenSearch. 

### What is an anomaly, and are you sure?

You tell me. No, seriously. You might say it's anything we deem out of the ordinary of course, but defining "out of the ordinary" comes from being **familiar and cognizant** about the data we're examining. Similarly, part of configuring your anomaly detection model in OpenSearch comes from knowing what you're looking for. I'll share the details of the things we have to consider in a tick. 

Here's what I'm fixing to build: 



![anomaly detection workflow diagram](/assets/media/blog-images/demystifying-anomaly-detection-3-.png "workflow diagram")

### Fluentbit ingesting like a champ as always.



### Ensure you have data, then create a detector.

Choose your features

Choose your interval