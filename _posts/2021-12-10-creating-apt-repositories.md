# Creating APT Repositories

__Guide to creating Debian/APT repositories.__

---

Maintaining my repository, [Raspbian Addons](https://raspbian-addons.org), has given me a great opportunity to learn how to create and use APT repositories for use on Debian systems! In this blog post, I'll show you two methods. Both are super simple to set up and use.

## Method #1: Simple One-Directory Repo

The first method I'll start with is the simplest kind, called PPAs (personal package archive). They are contained within one directory. I use PPAs for some of my simple apt repos, for example, [box64-debs](https://github.com/ryanfortner/box64-debs). So let's get started.

### 1. Set up and clone GitHub repo

For this guide, we'll be using GitHub Pages to host since it's free, but you can technically use any http server. Create a GitHub repo [here](https://github.com/new). For this guide, I'll call the repo `example-ppa`, using `master` as the main branch. Make sure GitHub Pages is enabled for the root directory.

Clone the GitHub repository locally so we can work with it, and then copy any .deb files you'd like to have in your repository there (make sure to substitute 'github-username' with your username you signed up with).

```bash
git clone git@github.com:github-username/example-ppa.git
cd example-ppa
cp /path/to/package.deb .
```

### 2. Generate a GPG Key

GPG keys are encryption techniques that are used in APT repository to verify the integrity of packages, in simpler terms, make them more secure.

Generate a new key:

```bash
sudo apt install gnupg -y
gpg --full-gen-key
```

You'll now be prompted to enter a few values. Select RSA for the first prompt.

```
Please select what kind of key you want:
   (1) RSA and RSA (default)
   (2) DSA and Elgamal
   (3) DSA (sign only)
   (4) RSA (sign only)
Your selection? 1
```

Select 4096 bits:

```
RSA keys may be between 1024 and 4096 bits long.
What keysize do you want? (3072) 4096
```

Select 'key does not expire'

```
Please specify how long the key should be valid.
0 = key does not expire
<n> = key expires in n days
<n>w = key expires in n weeks
<n>m = key expires in n months
<n>y = key expires in n years
Key is valid for? (0) 0
Key does not expire at all
Is this correct? (y/N) y
```

Enter your name and email, make sure to remember the email you put for later.

```
Real name: My Name
Email address: ${EMAIL}
Comment:
You selected this USER-ID:
"My Name <my.name@email.com>"

Change (N)ame, (C)omment, (E)mail or (O)kay/(Q)uit? O
```

Now, `gpg` will ask for a passphrase. **Make sure to write this passphrase down somewhere safe or you won't be able to add new packages to your repository.**

```
We need to generate a lot of random bytes. It is a good idea to perform
some other action (type on the keyboard, move the mouse, utilize the
disks) during the prime generation; this gives the random number
generator a better chance to gain enough entropy.
We need to generate a lot of random bytes. It is a good idea to perform
some other action (type on the keyboard, move the mouse, utilize the
disks) during the prime generation; this gives the random number
generator a better chance to gain enough entropy.
gpg: key B58FBB4C23247554 marked as ultimately trusted
gpg: directory '/home/assafmo/.gnupg/openpgp-revocs.d' created
gpg: revocation certificate stored as '/home/assafmo/.gnupg/openpgp-revocs.d/31EE74534094184D9964EF82B58FBB4C23247554.rev'
public and secret key created and signed.

pub rsa4096 2019-05-01 [SC]
31EE74534094184D9964EF82B58FBB4C23247554
uid My Name <my.name@email.com>
sub rsa4096 2019-05-01 [E]
```

If you'd like to back up the key just in case you switch machines, execute the following command.

```bash
gpg --export-secret-keys "my.name@email.com" > private-key.asc
# import it with `gpg --import private-key.asc`
```

### 3. Create the public key file

Now, we're going to generate the public key, which will be used by the ppa user's system to verify packages.

From within the `example-ppa` repo you created earlier:

```bash
gpg --armor --export "my.name@email.com" > KEY.gpg
```

### 4. Create the required APT files

APT repositories require a few files for them to work. These include the `Packages`, `Packages.gz`, `Release`, `Release.gpg`, `.list`, and `InRelease` files. Let's generate them now.

From within the `example-ppa` repo (make sure to substitute `my.name@email.com` with the email you created the gpg key with):

```bash
dpkg-scanpackages --multiversion . > Packages
gzip -k -f Packages
apt-ftparchive release . > Release
gpg --default-key "my.name@email.com" -abs -o - Release > Release.gpg
gpg --default-key "my.name@email.com" --clearsign -o - Release > InRelease
```

Generate the .list file, used by apt in the /etc/apt/sources.list.d directory:

```bash
echo "deb https://github-username.github.io/example-ppa ./" > example-ppa.list
```

### 5. Push changes and test!

You're done! Push the changes to GitHub and your PPA is ready.

```bash
git add .
git commit -m "Initial commit"
git push origin master
```

You can now install the PPA:

```bash
curl -s --compressed "https://github-username.github.io/example-ppa/KEY.gpg" | sudo apt-key add -
sudo curl -s --compressed -o /etc/apt/sources.list.d/example-ppa.list "https://ryanfortner.github.io/example-ppa/example-ppa.list"
sudo apt update
# now you can install the packages
sudo apt install package-x package-y package-z
```

See the next method for more advanced apt repositories.

## Method 2: `reprepro` Repositories

`reprepro` is an easy-to-use tool for power users to create Debian repositories. Debian, Raspberry Pi, Raspbian Addons, and more use `reprepro` for their repositories. Here's how to set one up!

For this method, you will also need a GPG key, follow the instructions from above to generate one. 

### 1. Create necessary folders and files

First we'll create a directory on a remote server. You can use GitHub pages for this like the method above, but again, a simple http server will also do the trick. reprepro has many different configuration options, but I'll be showing a super simple example in this guide.

Install reprepro on your system: 

```bash
sudo apt install reprepro -y
```

Create repository directory:

```bash
mkdir example-repo
cd example-repo
```

Create `conf` dir for the repository configuration, and the `distributions` file with the details of the repository. You can change these to your liking.

```bash
mkdir conf
cd conf
echo "Origin: example-repo.com
Label: apt repository
Codename: buster
Architectures: armhf amd64 arm64
Components: main
Description: my test repository
SignWith: yes
Pull: buster" > distributions
```

### 2. Add packages!

Next step is to add packages to your repo, make sure you're one directory up from the `conf` directory, and replace the `buster` codename with the one you specified in the `distributions` file if you changed it:

```bash
cd ..
reprepro --ask-passphrase -Vb . includedeb buster /path/to/package.deb
```

To remove packages:

```bash
reprepro remove buster packagename
```

### Automatic repository signing with `expect`

See [here](https://github.com/raspbian-addons/scripts/tree/master/reprepro) for my attempts on this, and [this forum post](https://askubuntu.com/questions/560573/reprepro-is-there-any-chance-to-enter-the-passphrase-via-bash-script) also.

Thanks to [@Itai-Nelken](https://github.com/Itai-Nelken) for helping me with both of these methods!