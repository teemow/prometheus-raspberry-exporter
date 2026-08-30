# Raspberry exporter

Export raspberry pi information to prometheus.

## Dependencies

 * Prometheus (obviously)
 * Node Exporter with textfile collector

## Install

Copy `raspberry_exporter` to `/usr/local/bin`.

Copy the systemd units to `/etc/systemd/system` and run 

```
systemctl enable prometheus-raspberry-exporter.timer
systemctl start prometheus-raspberry-exporter.timer
```

## Configure your node exporter

Make sure your node exporter uses `textfile` in `--collectors.enabled` and add the following parameter: `--collector.textfile.directory=/var/lib/node_exporter/textfile_collector`

## Example queries

```
rpi_temperature{job="node"}
rpi_freq{job="node",device="arm"}
rpi_volt{job="node",device="core"}
rpi_mem{job="node",device="arm"}
```
