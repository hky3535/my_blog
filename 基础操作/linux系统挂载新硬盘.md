mkfs.ext4 /dev/vdb
mkdir /data
mount /dev/vdb /data
UUID=$(blkid /dev/vdb | awk '{print $2}' | awk -F \" '{print $2}')
echo "UUID=$UUID /data ext4 defaults 0 0" >> /etc/fstab