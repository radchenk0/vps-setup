## Disclaimer
This guide doesn't pretend to be production ready, but could shed some light and give a direction what to do. Nevertheless, it expects basic linux knowledge from a reader.

## Intoduction

When you set up a box there are a few configuration steps that you should take early on as part of the basic setup. This will increase the security and usability of your server and will give you a solid foundation for subsequent actions.

## _Creating a new user_

Because of the heightened privileges of the __root__ account, we are discouraged from using it on a regular basis.
Thus we are setting up an alternative user account with a reduced scope of influence for day-to-day work.

__Connecting to your box__
```sh
ssh root@remote_host
```

__Creating a New User with a strong password__
```sh
adduser new_user
```
__Granting administrative privileges__
```sh
usermod -aG sudo new_user
```
> Note: Now, when logged in as your new_user, you can type `sudo` before commands to perform actions with superuser privileges.

## _Creating the RSA key pair_

So far, so good, but we are still open for brute-force attacks
To enhance your server’s security, it's strongly recommended setting up SSH keys instead of using password authentication

__Generating key pair__
```sh
# Host machine
ssh-keygen
```
__Copying public key using `ssh-copy-id`__
```sh
# Host machine
ssh-copy-id new_user@remote_host
```
or
    
```sh 
# Host machine
ssh-copy-id -i ~/.ssh/id_rsa.pub new_user@remote_host
```
__Alternative: copying public key manually__
The idea is to put a content of `id_rsa.pub` into the ~/.ssh/authorized_keys on the remotre box

```sh
# Host machine
cat ~/.ssh/id_rsa.pub
# Copy output
```

```sh
# Remote box
mkdir -p ~/.ssh
touch authorized_keys
nano authorized_keys
# Paste pub key you've copied from your local machine and save the authorized_keys file
```

__Ensuring permissions set__
~/.ssh directory and authorized_keys file should have the following perm set:
```sh
# Remote box
chmod -R go= ~/.ssh
```
> Note: This recursively removes all “group” and “other” permissions for the ~/.ssh/ directory

__Disabling password authentication on a remote box__
```sh
sudo nano /etc/ssh/sshd_config
```
Inside the file, search for a directive called PasswordAuthentication. This may be commented out. Uncomment the line and set the value to “no”.
Save and exit.

__Applying changes__
To actually implement these changes, we need to restart the ssh service
```sh
sudo systemctl restart ssh
```

> Note: As a precaution, open up a new terminal window and test that the SSH service is functioning correctly before closing this session:
``
ssh new_user@remote_host
``

## Resources

* [Initial Server Setup with Ubuntu](https://www.digitalocean.com/community/tutorials/initial-server-setup-with-ubuntu-18-0)
* [How to Set Up SSH Keys on Ubuntu](https://www.digitalocean.com/community/tutorials/how-to-set-up-ssh-keys-on-ubuntu-1804)
