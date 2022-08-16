# Apache Pinot

## acessing portal

```sh
http://localhost:9000/
http://localhost:9000/help
````

## adding schema

```curl
# http://localhost:9000/help#/Schema/addSchema
# curl -X GET "http://localhost:9000/schemas" -H "accept: application/json" | jq
# curl -X GET "http://localhost:9000/schemas/sch_output_faust_enriched_rides" -H "accept: application/json" | jq
curl -X POST "http://localhost:9000/schemas?override=true" -H "accept: application/json" -H "Content-Type: application/json" -d "{ \"schemaName\": \"sch_output_faust_enriched_rides_54acb974\", \"dimensionFieldSpecs\": [ { \"name\": \"user_id\", \"dataType\": \"LONG\" }, { \"name\": \"gender\", \"dataType\": \"STRING\" }, { \"name\": \"car_type\", \"dataType\": \"STRING\" }, { \"name\": \"model_type\", \"dataType\": \"STRING\" }, { \"name\": \"country\", \"dataType\": \"STRING\" }, { \"name\": \"city\", \"dataType\": \"STRING\" }, { \"name\": \"distance_in_km\", \"dataType\": \"FLOAT\" }, { \"name\": \"final_price_real\", \"dataType\": \"FLOAT\" }, { \"name\": \"dynamic_fare\", \"dataType\": \"STRING\" }, { \"name\": \"time_period\", \"dataType\": \"STRING\" }, { \"name\": \"processing_time\", \"dataType\": \"STRING\" } ], \"dateTimeFieldSpecs\": [{ \"name\": \"event_time\", \"dataType\": \"STRING\", \"format\": \"1:MILLISECONDS:SIMPLE_DATE_FORMAT:yyyy-MM-dd HH:mm:ss.SSSSSS\", \"granularity\": \"1:DAYS\" }]}"

# http://localhost:9000/help#/Table/addTable
# curl -X GET "http://localhost:9000/tables?sortAsc=true" -H "accept: application/json" | jq
# curl -X GET "http://localhost:9000/tables/realtime_output_faust_enriched_rides_events/status?type=realtime" -H "accept: application/json" | jq
# curl -X GET http://localhost:9000/tables/realtime_output_faust_enriched_rides_events/idealstate?tableType=realtime | jq
curl -X POST "http://localhost:9000/tables" -H "accept: application/json" -H "Content-Type: application/json" -d "{ \"tableName\": \"realtime_output_faust_enriched_rides_events\", \"tableType\": \"REALTIME\", \"segmentsConfig\": { \"timeColumnName\": \"event_time\", \"timeType\": \"DAYS\", \"retentionTimeUnit\": \"DAYS\", \"retentionTimeValue\": \"60\", \"schemaName\": \"sch_output_faust_enriched_rides_54acb974\", \"replication\": \"1\", \"replicasPerPartition\": \"1\" }, \"tenants\": {}, \"tableIndexConfig\": { \"loadMode\": \"MMAP\", \"invertedIndexColumns\": [ \"user_id\" ], \"streamConfigs\": { \"streamType\": \"kafka\", \"stream.kafka.consumer.type\": \"lowlevel\", \"stream.kafka.topic.name\": \"output-faust-enriched-rides\", \"stream.kafka.decoder.class.name\": \"org.apache.pinot.plugin.stream.kafka.KafkaJSONMessageDecoder\", \"stream.kafka.consumer.factory.class.name\": \"org.apache.pinot.plugin.stream.kafka20.KafkaConsumerFactory\", \"stream.kafka.broker.list\": \"edh-kafka-brokers.ingestion.svc.Cluster.local:9092\", \"realtime.segment.flush.threshold.time\": \"3600000\", \"realtime.segment.flush.threshold.size\": \"50000\", \"stream.kafka.consumer.prop.auto.offset.reset\": \"smallest\" } }, \"metadata\": { \"customConfigs\": {} }}"
```

## housekeeping

```sh
# table
curl -X DELETE "http://localhost:9000/tables/realtime_output_faust_enriched_rides_events?type=realtime" -H "accept: application/json"

# schema
curl -X DELETE "http://localhost:9000/schemas/sch_output_faust_enriched_rides" -H "accept: application/json"
```
