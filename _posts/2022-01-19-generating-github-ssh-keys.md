# Generating GitHub SSH Keys

__Guide to generating SSH keys and adding them to GitHub.__

---

Generating a SSH key for GitHub can sometimes be tricky, here's an easy method.

For this to work, you'll need an ssh installed on your system, and a Github account. ssh is preinstalled for most distributions, so you shouldn't have to worry about it.

### 1. Generate the key

Open a terminal prompt and execute the following command, making sure to substitute the email address with your email.

```
ssh-keygen -t ed25519 -C "your_email@example.com"
```

You will now be prompted to enter the file in which to save the key, it's safe to accept the default value here by clicking *enter*.

Then, enter a secure passphrase for the key if desired.

<details>
<summary> Generating an ssh key on a legacy system </summary>

  <code>ssh-keygen -t rsa -b 4096 -C "your_email@example.com"</code>

</details>

### 2. Adding the ssh key to the keyring

Now that you've generated the key, you need to tell ssh to use it when connecting to a remote server. You can do this by executing the following commands:

```
eval "$(ssh-agent -s)"
ssh-add -k ~/.ssh/id_ed25519
```

### 3. Adding the ssh key to a GitHub account

Visit the following web page: [https://github.com/settings/ssh/new](https://github.com/settings/ssh/new), and paste the output of the following command in the key area.

```
cat ~/.ssh/id_ed25519.pub
```

### 4. Configuring SSH to use the key

This step isn't entirely necessary, but if you want the key to be used every time you run the ssh command then you might want this.

Create a file called `config` in the ~/.ssh directory, by executing the following command.

If you named your key something else, make sure to replace the key file name.

```
echo "Host *
  IgnoreUnknown UseKeychain
  AddKeysToAgent yes
  UseKeychain yes
  IdentityFile ~/.ssh/id_ed25519
" > ~/.ssh/config
```

### Summary

Congratulations, you now have a fully-functional ssh key for your github account! If you encountered an error, write a comment below and I'll see what I can do.
