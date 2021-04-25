## dd
1. 输入
```
sudo bash -c 'cat << EOF > /etc/systemd/system/mnt-hgfs.mount
[Unit]
Description=VMware mount for hgfs
DefaultDependencies=no
Before=umount.target
ConditionVirtualization=vmware
After=sys-fs-fuse-connections.mount

[Mount]
What=vmhgfs-fuse
Where=/mnt/hgfs
Type=fuse
Options=default_permissions,allow_other

[Install]
WantedBy=multi-user.target
EOF'
```
2. 命令
```
sudo bash -c 'cat << EOF > /etc/modules-load.d/open-vm-tools.conf <<EOF
fuse
EOF'
```
3. 启用服务
```
sudo systemctl daemon-reload
sudo systemctl enable mnt-hgfs.mount
sudo modprobe -v fuse
sudo systemctl start mnt-hgfs.mount
```
4. 进入菜单 `虚拟机 > 共享 > 共享设置` 设置共享目录并启用
5. 