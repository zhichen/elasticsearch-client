
Elasticsearch Java Client
=========================

A simple Elasticseach client based on the 5.x HTTP API. This is by no means a full blown client and it support only a small sub set of the Elasticseach operations.
Furethemore, when the Elasticteam will release a full Java client, this client will probably become redundent. 
Nevertheless, the client is very simple for use, supports the basic operations and is designed to easily be extended.

The supported operations are:

* Index creation and deletion
* Document creation and deletion
* Query by term

## Getting started

**prerequisites**

* Java 8

**Maven**

```Xml

    <repositories>
        <repository>
            <id>topq</id>
            <url>http://maven.top-q.co.il/content/groups/public</url>
        </repository>
    </repositories>
    .
    .
    .
    <dependencies>
    .
    .
        <dependency>
            <groupId>il.co.topq.elastic</groupId>
            <artifactId>elasticseach-client</artifactId>
            <version>1.0.00</version>
        </dependency>

    .
    .

    </dependencies>

```


## Usage Examples

**Index Operations**

```Java

String settings = "{\"settings\": { \"index\": { \"number_of_shards\": 3, \"number_of_replicas\": 1  }}}";
String index = "reddit";

try (client = new ESClient("localhost", 9200)) {
    client
       .index(index)
       .create(settings);

    client
       .index(index)
       .delete();
}       

```

**Document Operations**

```Java
String index = "reddit";
String doc = "post";

Post post0 = new Post();
post0.setId(1212);
post0.setOp("Itai");
post0.setPoints(100);
post0.setSubreddit("all");

try (client = new ESClient("localhost", 9200)) {
    client
        .index(index)
        .document(doc)
        .add()
        .single("100", post0);
}

```


**Query Operations**

```Java
String index = "reddit";
String doc = "post";

try (client = new ESClient("localhost", 9200)) {
    List<Post> posts = client
        .index(index)
        .document(doc)
        .query()
        .byTerm("id", "1212")
        .asClass(Post.class);
}

```