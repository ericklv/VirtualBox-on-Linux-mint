# VirtualBox-on-Linux-mint
Run unsigned kernel modules with Secure Boot enabled

1. Create signing keys
openssl req -new -x509 -newkey rsa:2048 -keyout MOK.priv -outform DER -out MOK.der -nodes -days 36500 -subj "/CN=Descriptive name/"
2. Sign the module (vboxdrv for this example)
sudo /usr/src/linux-headers-$(uname -r)/scripts/sign-file sha256 ./MOK.priv ./MOK.der $(modinfo -n vboxdrv)
3. Register the keys to secure boot 
sudo mokutil --import MOK.der
4. Supply a password for later use after reboot

On boot
1. Select Enroll MOK
2. Select Continue
3. Select YES
4. Then you will need to enter the password you used when running 'mokutil --import'.
5. Select OK

After reboot
1. sudo modprobe vboxdrv
2. Execute virtualbox
