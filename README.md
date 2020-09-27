# Nintendo Switch Kubernetes

This repo will walk you through the setup and running of Kubernetes on a Nintendo Switch.

## Switch L2T

- Download https://torrents.switchroot.org/ubuntu/
- Download the update: https://download.switchroot.org/ubuntu/updates/switchroot-l4t-ubuntu-3.0.0-2020-03-02/update-3.0.1-for-switchroot-l4t-ubuntu-3.0.0-2020-03-02.zip
- Use [etcher](https://www.balena.io/etcher/) to flash the .img
- Delete `l4t-ubuntu` folder and `bootloader/ini/00-ubuntu.ini` from the mounted volume on the SD card
- Copy the contents of `update-3.0.1-for-switchroot-l4t-ubuntu-3.0.0-2020-03-02` to the mounted volume on the SD card
- Download [hekate](https://github.com/ctcaer/hekate/releases) and copy the contents of the zip to the mounted volume on the SD card
- Download either [TegraRcmGUI](https://github.com/eliboa/TegraRcmGUI) if you are on Windows; or [fusee-launcher](https://github.com/Qyriad/fusee-launcher) on Linux/Mac.
- Connect the USB Type-C cable to your switch and computer. Power off your Nintendo Switch by holding the Power button
- Insert Joy-Con JIG then hold the VOL+ button and press the POWER button. Your switch should appear to do nothing
- If you used [TegraRcmGUI](https://github.com/eliboa/TegraRcmGUI), select the `hekate_ctcaer_5.3.3.bin` and inject payload
- If you used [fusee-launcher](https://github.com/Qyriad/fusee-launcher), run `./fusee-launcher.py hekate_ctcaer_5.3.3.bin`
- Your switch will boot to the Hekate menu. Remove the Joy-Con JIG, reconnect the controllers and unplug the USB Type-C cable.
- Click Nyx Options, then click Dump Joy-Con BT
- Click More Configs on the Hekate menu then select Ubuntu Linux
- Proceed through the setup process until you have booted into Ubuntu. Once logged in, reboot the system back to the Hekate menu. Re-open Ubuntu Linux
- Expand the disk volume by using the `Disk` utility in Ubuntu.
- Connect to WiFI, then update the system: `sudo apt update && sudo apt upgrade`

## Kubernetes

```bash
# Install dependencies
sudo su
curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | apt-key add -
cat <<EOF > /etc/apt/sources.list.d/kubernetes.list
deb http://apt.kubernetes.io/ kubernetes-xenial main
EOF
apt-get update
apt-get install -y docker.io kubeadm

# Setup cluster
kubeadm init

# Back out of root
exit

# Setup kubectl locally
sudo cp /etc/kubernetes/admin.conf $HOME/
sudo chown $(id -u):$(id -g) $HOME/admin.conf
export KUBECONFIG=$HOME/admin.conf
```

## Attribution

- [L4T Ubuntu - A fully featured linux on your switch](https://gbatemp.net/threads/l4t-ubuntu-a-fully-featured-linux-on-your-switch.537301/)
- [Tutorial:How To Install L4t Ubuntu 3.0.1 on the Nintendo Switch In 2020](https://www.youtube.com/watch?v=xcrC1-fXs7c)
- [luxas/kubeadm-workshop](https://github.com/luxas/kubeadm-workshop)
