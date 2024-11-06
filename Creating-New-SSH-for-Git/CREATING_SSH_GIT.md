### After creating a fresh notebook on Gradient, I tried to clone a repo that I made in github.

### I wanted to set up an ssh key for use with github. I started up the ssh agent...
eval "$ssh-agent -s)"
#output: Agent pid 587

### the output showed that the ssh agent was active, so I asked it to create a new ssh key for me using best-in-class algorythm ed25519
ssh-add ~/.ssh/id_ed25519
#output: /root/.ssh/id_ed25519: No such file or directory

### This meant the system did not have an .ssh key folder, so I asked it to create a new keygen
ssh-keygen -t ed25519 -C "<my-email>"
#output: Generating public/private ed25519 key pair.
#output: Enter file in which to save the key (/root/.ssh/id_ed25519):

### I accepted the default (/root/.ssh/id_ed25519) by simply pressing enter
#output: Created directory '/root/.ssh'.
#output: Enter passphrase (empty for no passphrase):

### I decided on a passphrase and entered it. I also entered it a second time.
#output: Enter same passphrase again:
#output: Your identification has been saved in /root/.ssh/id_ed25519
#output: Your public key has been saved in /root/.ssh/id_ed25519.pub
#output: Your key fingerprint is: <my-key> <my-email>

### Then it also provided a randomart image. (I'm not sure what I do with this... :D ). I also updated permissions of the .ssh file to only let me read/write using:
chmod 700 ~/.ssh
chmod 600 ~/.ssh/*

### I copied the contents of the .pub file and went to github.
### I clicked my icon in upper right > Settings > SSH and GPG keys > New SSH key.
### I added a name for the key, selected Authentication Key, and added my public key to "Key" text box. Then clicked "Add SSH key".

### It added successfully!
### I went back to my remote terminal and tested if the ssh key is correctly configured for GitHub:
ssh -T git@github.com
#output: Hi username! You've successfully authenticated, but GitHub does not provide shell access.
### Success!

### I tried to clone the repo again
git clone git@github.com:jeffreysanglin/signing_anyway
#output: The authenticity of host 'github.com (140.82.113.4)' can't be established.
ED25519 key fingerprint is SHA256:+DiY3wvvV6TuJJhbpZisF/zLDA0zPMSvHdkr4UvCOqU.
This key is not known by any other names

### I searched "github ed25519" public ssh and found it on this page:
https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/githubs-ssh-key-fingerprints
### Then, I confirmed that I had received the correct fingerprint for Ed25519.
### Next, I went back to the terminal and typed "yes".
#output: Warning: Permanently added 'github.com' (ED25519) to the list of known hosts.

### I attempted to push a new .gitignore file to the repo. After getting and fixing a "private email" error, it was successful?
#output: Your branch is up to date with 'origin/main'.