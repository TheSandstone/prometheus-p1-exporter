# Prometheus P1 exporter #

Prometheus exporter for smart meter statistics fetched with a P1 cable.
This is a fork of jordyv's prometheus-p1-exporter because I was in need of the peak power metric and digital water meter (Belgium). Most of the work you find here is created by him.


## Installation ##

### Docker ###

$ git clone https://github.com/TheSandstone/prometheus-p1-exporter.git
$ cd prometheus-p1-exporter

```
$ sudo docker build -t p1meter .
```

Use the docker-compose.yml file with the following command to start.

```
$ sudo docker-compose up -d

```


## Usage ##

```
Usage of ./prometheus-p1-exporter:
  -apiEndpoint string
        Use API endpoint to read the telegram (use for HomeWizard)
  -interval duration
        Interval between metric reads (default 10s)
  -listen string
        Listen address for HTTP metrics (default "127.0.0.1:8888")
  -mock
        Use dummy source instead of ttyUSB0 socket
  -usbserial string
    	USB serial device (default "/dev/ttyUSB0")
  -verbose
        Verbose output logging
```

By default the exporter will collect metrics from `/dev/ttyUSB0` every 10 seconds and export the metrics to an HTTP endpoint at `http://127.0.0.1:8888/metrics`. This endpoint can be added to your Prometheus configuration.

Example metrics page:

```
# HELP p1_active_tariff Active tariff
# TYPE p1_active_tariff gauge
p1_active_tariff 2
# HELP p1_current_usage_electricity_high Electricity currently used high tariff
# TYPE p1_current_usage_electricity_high gauge
p1_current_usage_electricity_high 0
# HELP p1_current_usage_electricity_low Electricity currently used low tariff
# TYPE p1_current_usage_electricity_low gauge
p1_current_usage_electricity_low 0.2
# HELP p1_power_failures_long Power failures long
# TYPE p1_power_failures_long gauge
p1_power_failures_long 2
# HELP p1_power_failures_short Power failures short
# TYPE p1_power_failures_short gauge
p1_power_failures_short 57
# HELP p1_returned_electricity_high Electricity returned high tariff
# TYPE p1_returned_electricity_high gauge
p1_returned_electricity_high 0
# HELP p1_returned_electricity_low Electricity returned low tariff
# TYPE p1_returned_electricity_low gauge
p1_returned_electricity_low 0.016
# HELP p1_usage_electricity_high Electricity usage high tariff
# TYPE p1_usage_electricity_high gauge
p1_usage_electricity_high 1225.59
# HELP p1_usage_electricity_low Electricity usage low tariff
# TYPE p1_usage_electricity_low gauge
p1_usage_electricity_low 1179.186
# HELP p1_electricity_peak Monthly peak in electricity usage
# TYPE p1_electricity_peak gauge
p1_electricity_peak 5.584
# HELP p1_watergasUsageMetric1 Total amount of used water/gas
# TYPE p1_watergasUsageMetric1 gauge
p1_watergasUsageMetric1 1019.003
# HELP p1_watergasUsageMetric2 Total amount of used water/gas
# TYPE p1_watergasUsageMetric2 gauge
p1_watergasUsageMetric2 2019.003
```

## Remarks ##
Gas and water meter use the same code, it depends on which one is placed first.


## Development ##

Currently only the ESMR 5.0 format is supported and the parser is default configured to parse the telegram message with the keys the Sagemcom XS210 is using.
If you have to support a different ESMR 5.0 message, feel free to create your own implementation of the TelegramFormat struct. To support a different format then ESMR 5.0 you can implement your own implementation of the TelegramReaderOptions struct.
