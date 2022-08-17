# Apache Kafka

```sh
kcat -C -b 167.99.20.155:9094 -t src-app-ride-events-json -J -o end | jq
kcat -C -b 167.99.20.155:9094 -t output-faust-enriched-rides -J -o end | jq
```
