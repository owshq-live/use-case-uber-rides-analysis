# Faust

```shell
# verify kafka connectivity
export KAFKA_BOOTSTRAP_SERVER = "kafka://167.99.20.155:9094"

# generate data into topic
export TOPIC_SRC_APP_RIDES_JSON = "src-app-ride-events-json"
http://159.89.247.235:3636/docs

# init python faust application
# faust/pr-rides-analysis
faust -A src.app worker -l info

# verify topics
kcat -C -b 167.99.20.155:9094 -t src-app-ride-events-json -J -o end
kcat -C -b 167.99.20.155:9094 -t output-faust-enriched-rides -J -o end
```
