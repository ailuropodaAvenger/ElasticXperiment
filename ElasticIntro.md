<h3> Elastic </h3>
Elasticsearch is an open source, distributed, RESTful search engine built on top of the Apache Lucene library.When data is imported, it immediately becomes available for searching. Elasticsearch is schema-free, stores data in JSON documents, and can automatically detect the data structure and type. It has many client libraries for almost any programming language.

Elasticsearch is a better choice for applications that require not only text search but also complex time series search and aggregations.

<h5>Installation</h5>
Elasticsearch requires at least Java 8.It is recommended that to use the Oracle JDK version 1.8.0_73.
check Java version first by running (and then install/upgrade accordingly if needed):
```{r, engine='bash', count_lines}
    java -version
    echo $JAVA_HOME
```
<h6>Install Java 8 </h6>
ubuntu :  https://www.unixmen.com/installing-java-jrejdk-ubuntu-16-04/

windows :  http://www.herongyang.com/JVM/JDK-180-Windows-Download-Installation.html.html

<h6> Install Elasticsearch </h6>
The best way to start from Elastic documentation

https://www.elastic.co/guide/en/elasticsearch/reference/5.0/install-elasticsearch.html

<h5> Let's start with example </h5>
Start with installing Sense, a Chrome plugin for Elasticsearch. It offers autocompletion, code highlighting and formatting and can help you with exploring Elasticsearch. 

https://chrome.google.com/webstore/detail/sense-beta/lhjgkmllcaadmopgmanpapmpjgmfcfig

<h5> Indexing some video files </h5>
<h6>Indexing single video</h6>

```json
PUT /videos/video/1?pretty
{
  "id": 1,
  "title": "Mr & Mrs",
  "description": "Mr & Mrs (মিস্টার এন্ড মিসেস) is a bengali drama released in ntv in this Eid Ul Azha 2016. This drama is telecast by International Television Channel Ltd (NTV). Actor and actress on this drama are Tahsan & Mithila, Alif, Khalid, Flora and so on.",
  "duration": "00:44:56",
  "link": "https://www.youtube.com/watch?v=KwWbgLD7YCY",
  "categories":{"id": 1,"name": "Teleflim"},
  "role": {
    "Production": [{"id": 1,"name": "NTV","logo": ""}],
    "Actor": [
                {"id": 1,"name": "Tahsan Khan","image": "","gender": "male"},
                {"id": 2,"name": "Mithila","image": "","gender": "female"}
              ]
            }
  }

PUT /videos/video/2?pretty
{
  "id": 2,
  "title": "Rupkotha Ekhon Ar Hoy Na",
  "description": "Rupkotha Ekhon Ar Hoy Na (রূপকথা এখন আর হয় না) is a bengali drama released in ntv on this eid festive 2016. This drama is telecast by International Television Channel Ltd (NTV). Actor and actress on this drama are Momo & Tahsan. ",
  "duration": "00:43:38",
  "link": "https://www.youtube.com/watch?v=WPqwre_TX-w",
  "categories":{"id": 1,"name": "Teleflim"},
  "role": {
    "Production": [{"id": 1,"name": "NTV","logo": ""}],
    "Actor": [
              {"id": 1,"name": "Tahsan Khan","image": "","gender": "male"},
              {"id": 3,"name": "Zakia Bari Mamo","image": "","gender": "female"}
              ]
            }
}

PUT /videos/video/3?pretty
{
  "id": 3,
  "title": "Tomay Vebe Lekha",
  "description": "Tomay Vebe Lekha Bangla Natok Download Website for Valentines Day Natok (2016). Tahsan & Tisha fans for New Drama Tomay Vebe Lekha Bangla Natok 2016 .",
  "duration": "00:42:10",
  "link": "https://www.youtube.com/watch?v=dfJOaA076k0",
  "categories":{"id": 1,"name": "Teleflim"},
  "role": {
    "Production": [{"id": 1,"name": "NTV","logo": ""}],
    "Actor": [
              {"id": 1,"name": "Tahsan Khan","image": "","gender": "male"},
              {"id": 4,"name": "Tisha","image": "","gender": "female"}
              ]
            }
 }
```
<h6>Indexing with bulk API </h6>
must put new line/enter after 1st line
if got json parse error... sense auto indent
```json
POST /videos/video/_bulk?pretty
{"index":{"_id":"4"}}
{"id":4,"title":"Prem o Protishodh","description":"Mosharraf Karim & Tisha Natok Prem o Protishodh | প্রেম ও প্রতিশোধ. [HD]Exclusively brought to you by Mosharraf Karim HD.","duration":"00:43:25","link":"https://www.youtube.com/watch?v=ZwTSwpwB0e8","categories":{"id":1,"name":"Teleflim"},"role":{"Production":[{"id":1,"name":"NTV","logo":""}],"Actor":[{"id":5,"name":"Mosharraf Karim","image":"","gender":"male"},{"id":4,"name":"Tisha","image":"","gender":"female"}]}}
{"index":{"_id":"5"}}
{"id":5,"title":"Tomar Amar","description":".","duration":"00:4:11","link":"https://www.youtube.com/watch?v=VdTSv6u6iP0","categories":{"id":2,"name":"Music"},"role":{"Production":[{"id":1,"name":"NTV","logo":""}],"Actor":[{"id":1,"name":"Tahsan","image":"","gender":"male"},{"id":2,"name":"Mithila","image":"","gender":"female"}]}}
{"index":{"_id":"6"}}
{"id":6,"title":"Sporsher Baire Tumi","description":"","duration":"00:03:39","link":"https://www.youtube.com/watch?v=Vw1qLsdIVtw","categories":{"id":2,"name":"Music"},"role":{"Production":[{"id":1,"name":"NTV","logo":""}],"Actor":[{"id":1,"name":"Tahsan","image":"","gender":"male"},{"id":6,"name":"Elita","image":"","gender":"female"}]}}

```

<h5>get search data </h5>
Search across all indexes and all types.

POST /_search 

Search across all types in the videos index.

POST /videos/_search 

Search explicitly for documents of type video

POST /videos/video/_search


In order to make a more useful search request we can use ElasticSearch's query DSL.
Let's try a search for the word "Tumi" which is present in the title of one of our videos
```json
POST /videos/video/_search 
{
    "query": {
        "query_string": {
            "query": "tumi"
        }
    }
}
```

Nested Query:
Find "music" category videos of actor "Tahsan" & "Mithila" :

```json
POST /videos/video/_search 
{
  "query": {
    "bool": {
      "must": [
        { "match": { "categories.name": "Music" }},
        { "match": { "role.Actor.name":  "Tahsan" }},
        { "match": { "role.Actor.name":  "Mithila" }} 
      ]
    }
  }
}
```
Fuzzy query:
The fuzzy query generates all possible matching terms that are within the maximum edit distance specified in fuzziness

```json
POST /videos/video/_search 
{
    "query": {
        "fuzzy" : {
            "title" : {
                "value" :         "tomr",
                    "boost" :         1.0,
                    "fuzziness" :     2,
                    "prefix_length" : 1
            }
        }
    }
}
```
 *prefix_length - The number of initial characters which will not be “fuzzified”.
return values :
"title": "Sporsher Baire Tumi"
"title": "Tomar Amar"
"title": "Tomay Vebe Lekha"

<h5>Tutorial</h5>
http://joelabrahamsson.com/elasticsearch-101/

https://dzone.com/articles/23-useful-elasticsearch-example-queries

http://opensourceconnections.com/blog/2015/09/18/the-simple-power-of-elasticsearch-analyzers/
