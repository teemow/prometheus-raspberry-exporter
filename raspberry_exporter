#!/bin/bash 

set -eu

VCGEN=/opt/vc/bin/vcgencmd
TEXTFILE=/var/lib/node_exporter/textfile_collector/raspberry-metrics.prom
TEMPFILE=$TEXTFILE.$$

mkdir -p /var/lib/node_exporter/textfile_collector/

TEMP=$(awk '{printf "%.2f", $1/1000}' /sys/class/thermal/thermal_zone0/temp)

if [ -z "$TEMP" ];
then
	echo "$NOW - Error: Value variable empty"
else
	echo "CPU_Temperature ${TEMP}" > $TEMPFILE

	for id in arm core h264 isp v3d uart pwm emmc pixel vec hdmi dpi; do
		echo "Freq_$id $($VCGEN measure_clock $id | awk '{split($0,a,"="); print a[2]}')" >> $TEMPFILE
	done

	for id in core sdram_c sdram_i sdram_p; do
		echo "Volt_$id $($VCGEN measure_volts $id | awk '{split($0,a,"="); print a[2]}' | sed 's/V$//')" >> $TEMPFILE
	done

	for id in arm gpu; do
		echo "Mem_$id $($VCGEN get_mem $id | awk '{split($0,a,"="); print a[2]}' | sed 's/M$//')" >> $TEMPFILE
	done

	mv $TEMPFILE $TEXTFILE
fi
