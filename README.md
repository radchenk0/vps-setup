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


## Resources

* [Initial Server Setup with Ubuntu](https://www.digitalocean.com/community/tutorials/initial-server-setup-with-ubuntu-18-0)
