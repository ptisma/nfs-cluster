## Instructions how to setup the NFS

I've used Ubuntu 20.04 VM-s on my Hetzner Cloud.

## Commands

### Run on Server

sudo apt update -y
sudo apt install nfs-kernel-server -y

mkdir /nfs

Use this if you are using the root squash option (disables the translation from root user on client into a non privileged user on server):
sudo chown nobody:nogroup /nfs

Add this line in /etc/exports:
/nfs *(rw,sync,no_root_squash,no_subtree_check)

sudo exportfs -a

sudo systemctl restart nfs-kernel-server

### Run on Client

sudo apt install nfs-common -y

mkdir /nfs

sudo mount node-0:/nfs /nfs

Enable on boot mount, add to /etc/fstab:
node-0:/nfs               /nfs      nfs auto,nofail,noatime,nolock,intr,tcp,actimeo=1800 0 0

Adding to /etc/fstab only won't work because this gets executed during boot time while the network isn't still loaded, so we resort to creating a systemd service.

Create a systemd mount service at /etc/systemd/system/nfs-mount.service:
[Unit]
Description=Mount NFS Share
After=network-online.target
Wants=network-online.target

[Service]
Type=oneshot
ExecStart=/usr/bin/mount -a
RemainAfterExit=yes

[Install]
WantedBy=multi-user.target

Enable and run the service:
sudo systemctl enable nfs-mount.service
sudo systemctl start nfs-mount.service
sudo systemctl status nfs-mount.service
