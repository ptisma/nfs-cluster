# nfs-cluster


## Intro
I wanted to setup the NFS storage on my homelab cluster on Hetzner Cloud. Previously I deployed the extra disk (Hetzner Cloud Volume) which I mounted on the "Server" VM and using SSHFS "shared" this mounted filesystem to other VM-s.

While the SSHFS is relatively simply to setup if SSH already works, its generally slower than NFS due to encryption overhead.

NFS is not encrypted by default, for that something like Kerberos should be used, it's ideal for trusted networks and VPN-s.

NFS works in a fashion of client-server architecture: where the server exports directories to clients, and the clients can mount the exported directories.

## nfs

Inside the folder /nfs you will find out the instructions how to get it working.
