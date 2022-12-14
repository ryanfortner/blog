# Raspberry Pi OS Bullseye Release

__Raspberry Pi OS has been upgraded to the latest Debian version: bullseye.__

---

<figure>
<img src="https://www.raspberrypi.com/app/uploads/2021/10/toy-story-bullseye-500x375.jpg" alt="bullseye" style="width:100%">
<figcaption align = "center"><b>Each release is traditionally named after a Toy Story character.</b></figcaption>
</figure>

[Raspberry Pi OS](https://www.raspberrypi.com/software/) is the operating system that the Raspberry Pi Foundation provides for their products. This morning, the foundation announced their new operating system based on Debian Linux, codenamed Bullseye. 

This upgrade from the previous release, Buster, contains many improvements under-the-hood, starting with the **inclusion of GTK+3**, the third version of the GTK+ user interface kit. This has been a long time coming, as GTK+2 was in Raspberry Pi OS Buster, and was getting quite old. The most obvious changes made due to the addition of GTK+3 include appearance changed to widgets, tabbed interfaces, and other controls.

Moving on, there is a (sort of) **new notification system**! Remember on Buster, when you unplugged a drive without ejecting it, a banner will appear in the top right corner? Well, now these notifications are for more than just that! Applications can now send notifications in the same way.

Along with the notifications comes an **updater plugin**, which basically uses apt (the system package manager) to check for updates and utilize the notification system to notify the user. This is a nice addition to the desktop, making it a bit more modern, as many popular Linux distros like [Ubuntu](http://ubuntu.com) contain updaters like this.

Onto the **file manager improvements**! According to RPF, the controls within the file manager have been simplified, specifically the thumbnail and window modes.

**KMS driver improvements** have been included! The KMS driver is the kernel modesetting driver, and it was previously experimental in past versions of RPiOS. Now, it's the default driver. The code for the drivers is now open-source, so now third-parties can use the code to improve compatibility with products like displays. The included Chromium browser has been updated to use this driver.

Many other changes have also been implemented, I won't go into complete detail right now, but I'll list them:

- New camera driver: now uses libcamera
- Raspberry Pi Bookshelf now includes Custom PC magazine
- Raspberry Pi Configuration has been improved, as well as the startup wizard
- Use of the Mutter window manager
- Bug fixes, other tweaks not mentioned.

Stay tuned for more updates!