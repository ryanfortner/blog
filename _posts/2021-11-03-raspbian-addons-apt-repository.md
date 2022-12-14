# Raspbian Addons APT repository

__Raspbian Addons is a large collection of open-source software you can install on your Raspberry Pi.__

---

Greetings blog, today I'd like to write about [Raspbian Addons](https://github.com/raspbian-addons/raspbian-addons), the Debian repository for Raspberry Pies that everyone's been waiting for.

You can read more about the project [here](https://github.com/raspbian-addons/raspbian-addons/blob/master/docs/DOCUMENTATION.md), but I'll give a brief overview. Apt is the system package manager for Debian systems (what RPi OS is based on), and is used to install and manage software. The Raspberry Pi Foundation and even Debian themselves provide repositories, however, Debian is known for older software in their repos. Raspbian Addons aims to fix that. It's an Apt repository that's managed by me and contributors in our free time, and contains new and updated open source software that *just works*!

Installation is simple. If you're interested, fire up a Pi, make sure you have a working Internet connection, and open a terminal window. Type the following command to begin installation.

```
python3 <(curl -fSsL https://scripts.raspbian-addons.org/utils/repo.py)
```

The script will ask you which mirror you'd like to use. Pick the one that's closest to your location, as you'll get more reliable speeds. Once the repository is installed, you can now install software from it! Just run `sudo apt install <packagename>` (a full list of included packages can be found [here](https://apt.raspbian-addons.org/debian/pool/)).
