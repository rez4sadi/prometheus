#install node exporter
export RELEASE="1.8.0"
wget https://github.com/prometheus/node_exporter/releases/download/v$RELEASE/node_exporter-$RELEASE.linux-386.tar.gz
tar xzf node_exporter-$RELEASE.linux-386.tar.gz
cd node_exporter-$RELEASE.linux-386/
mv ./node_exporter /usr/local/bin/
useradd --no-create-home --shell /bin/false node_exporter

echo  -n "
Description=Node Exporter
After=network.target

[Service]
User=node_exporter
Group=node_exporter
Type=simple
ExecStart=/usr/local/bin/node_exporter

[Install]
WantedBy=multi-user.target " >  /etc/systemd/system/node_exporter.service


systemctl daemon-reload
systemctl enable node_exporter.service --now
-------------------------------------------------------------------------------------------------------------------------------
echo "Cron_BK 1" | curl --data-binary @- http://192.168.6.104:9091/metrics/job/BK/instance/192.168.6.104/backupdb/
echo "Cron_BK 1"  | curl --data-binary @- http://192.168.6.121:9091/metrics/job/BK/instance/192.168.6.121/

echo $?
--------------------------------------------------------------------------------------------------------------------------
ping -c 2 8.8.8.8
RET=$(echo $?)
if [ $RET ==  "0" ]; then
echo "Cron_BK 1"  | curl --data-binary @- http://192.168.6.121:9091/metrics/job/BK/instance/192.168.6.121/backupdb/
else
echo "Cron_BK 0"  | curl --data-binary @- http://192.168.6.121:9091/metrics/job/BK/instance/192.168.6.121/backupdb/

fi
--------------------------------------------------------------------------------------------------------------------------------------------
ping -c 2 8.8.8.8
RET=$(echo $?)
if [ $RET ==  "0" ]; then
echo "Cron_BK 1"  | curl --data-binary @- http://192.168.6.104:9091/metrics/job/BK/instance/192.168.6.104/backupdb/
else
echo "Cron_BK 0"  | curl --data-binary @- http://192.168.6.104:9091/metrics/job/BK/instance/192.168.6.104/backupdb/

fi
