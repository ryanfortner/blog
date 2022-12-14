# Disable Snap on Ubuntu Linux

__How to disable Snap on Ubuntu 22.04, and some other useful tips.__

---

Canonical, the maintainers/developers of Ubuntu Linux, a derivative of Debian, are slowly transitioning many previously-native applications to Snap packages. This is quite controversial in the Ubuntu user community, as Snap packages can be easier to manage since they are container-based, however they take up more storage and can be much slower than native applications due to their sandboxed environment.

An example of one of these controversial changes is the transition to forced Snap packages for web browsers, such as Chromium and Firefox, in newer releases of the distribution. Here is how to completely disable and uninstall Snap.

### Remove all currently installed Snap packages
Before removing snap you need to get rid of all its currently installed packages. You can do this by listing all the installed packages:
```
snap list
```
and then removing all listed packages. For me, these commands did the trick:
```
sudo snap remove --purge firefox
sudo snap remove --purge snap-store
sudo snap remove --purge gnome-3-38-2004
sudo snap remove --purge gtk-common-themes
sudo snap remove --purge snapd-desktop-integration
sudo snap remove --purge bare
sudo snap remove --purge core20
sudo snap remove --purge snapd
```
You can then proceed to purge snapd from the system altogether.
```
sudo apt purge snapd
```
(Optional): you can also trick the system into thinking snap doesn't exist, blocking any packages from installing snap as a dependency.
```
echo "Package: snapd
Pin: release a=*
Pin-Priority: -10" | sudo tee /etc/apt/preferences.d/nosnap.pref >/dev/null
```
### Install Firefox natively (without snap)
If you would like to re-install firefox natively on your system for better performance, follow the steps above first, then enter these commands.
```
sudo add-apt-repository ppa:mozillateam/ppa

echo "Package: firefox*
Pin: release o=LP-PPA-mozillateam
Pin-Priority: 1001" | sudo tee /etc/apt/preferences.d/mozillateamppa >/dev/null

sudo apt-get update && sudo apt-get install firefox
```
### Install gnome-software instead of Snap Store
Follow the instructions to remove Snap first, then enter these commands.
```
sudo apt-get update
sudo apt-get install --install-suggests gnome-software

# optional flatpak support
sudo apt-get install flatpak
flatpak remote-add --if-not-exists flathub https://flathub.org/repo/flathub.flatpakrepo
```
