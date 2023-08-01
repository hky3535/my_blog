cp -r /var/lib/docker/ /data
rm -rf /var/lib/docker/
ln -s /data/docker/ /var/lib/docker