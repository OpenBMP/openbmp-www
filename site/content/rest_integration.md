---
title: "Rest Integration"
date: 2017-01-04T15:04:10.000Z
---

Network data published by the snas.io collector is stored in a MySQL database and developers can directly query the DB
See example MySQL queries [here](examples#mysql-client).

Alternatively, the DB supports REST APIs that developers can use to query the DB. A full list of the
REST APIs supported can be found [here](https://github.com/OpenBMP/db_rest).

All queries have the following JSON syntax/output:
![Example](/img/json_format_for_website.png)

<p>Below is an example DB response to a GET for count of BGP peers by type (GET /db_rest/v1/peer/type/count)-</p>

<img src="/img/json_output_for_website.png" alt="" class="left db mb1" style="width: 740px">

The REST interface provide multiple APIs to get filtered data about BGP peers and prefixes in widely
adopted JSON format - a flexible and extensible alternative to MySQL queries or using Kafka data from the message bus directly!
