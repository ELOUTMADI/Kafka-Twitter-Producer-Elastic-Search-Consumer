# Kafka to Elasticsearch Consumer

This code is a Java program that consumes data from a Kafka topic, processes the data, and then indexes the data into Elasticsearch.

## Dependencies

- `com.google.gson.JsonParser` - for parsing JSON data
- `org.apache.http` - for interacting with Elasticsearch
- `org.elasticsearch` - for interacting with Elasticsearch

## Functions

### `createClient`

This method returns a `RestHighLevelClient` object used to connect to Elasticsearch. It takes in the following arguments:

- `hostname`: the hostname of the Elasticsearch cluster
- `username`: the username for authenticating with the Elasticsearch cluster
- `password`: the password for authenticating with the Elasticsearch cluster

The method uses the input arguments to create a `CredentialsProvider` object. It then uses the `RestClient` and `RestClientBuilder` classes to create a builder that can be used to create a `RestHighLevelClient` object.

### `createConsumer`

This method returns a `KafkaConsumer` object used to consume data from a Kafka topic. It takes in the following argument:

- `topic`: the name of the Kafka topic to consume from

The method uses the input topic and a set of predetermined properties to create the `KafkaConsumer` object.

### `extractIDfromTweet`

This method extracts an ID from the record's JSON data. It takes in the following argument:

- `tweetJson`: the JSON data as a string

It returns the ID as a string.

## `main` method

This method serves as the entry point for the program. It does the following:

1. Creates a `RestHighLevelClient` object and a `KafkaConsumer` object using the previously defined methods.
2. Enters a loop that continually polls the Kafka consumer for new records and processes each record as it comes in. 
3. For each record:
   1. Extracts the ID from the record's JSON data using the `extractIDfromTweet` method.
   2. Creates an `IndexRequest` object and uses this to index the record's data into Elasticsearch.
   3. Keeps track of the number of records processed and, once a certain number have been processed, sends a bulk index request to Elasticsearch to improve performance.
4. Continues to run and process records until it is manually stopped.

# Twitter to Kafka Producer

This code is a Java program that connects to the Twitter streaming API, consumes tweets that match a given set of keywords, and produces these tweets to a Kafka topic.

## Dependencies

- `com.google.common.collect.Lists` - for creating lists of keywords to track
- `com.twitter.hbc` - for interacting with the Twitter streaming API
- `org.apache.kafka` - for interacting with Kafka
- `org.slf4j` - for logging

## Functions

### `run` method

This method serves as the entry point for the program. It does the following:

1. Creates a `BlockingQueue` and a `Client` object using the `createTwitterClient` method.
2. Connects the client to the Twitter streaming API.
3. Creates a `KafkaProducer` object using the `createKafkaProducer` method.
4. Adds a shutdown hook that stops the client and closes the producer when the program is interrupted.
5. Enters a loop that continually polls the `BlockingQueue` for new tweets and produces each tweet to the Kafka topic as it comes in. The loop exits when the client is done.

### `createTwitterClient`

This method returns a `Client` object used to connect to the Twitter streaming API. It takes in the following argument:

- `msgQueue`: a `BlockingQueue` to store tweets in

The method uses a set of predetermined constants (e.g. consumer key, consumer secret) and the input `BlockingQueue` to create the `Client` object. It sets up the client to track a predetermined list of keywords and to use OAuth 1.0a for authentication.

### `createKafkaProducer`

This method returns a `KafkaProducer` object used to produce tweets to a Kafka topic. It uses a set of predetermined properties to create the `KafkaProducer` object.

## `main` method

This method serves as the entry point for the program. It calls the `run` method to start the program.

## Notes

- The program uses the `slf4j` library for logging.
- The constants (e.g. consumer key, consumer secret) and properties (e.g. bootstrap servers) used in this program should be stored in a configuration file in a production environment.


