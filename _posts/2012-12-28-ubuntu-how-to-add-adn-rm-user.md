---
layout: post
title: "ubuntu how to add adn rm user "
description: ""
category: Ubuntu 
tags: [os]
---

* Add an account

To add a user account, use the following syntax, and follow the prompts to give the account a password and identifiable characteristics such as a full name, phone number, etc.

```
sudo adduser username
```

* To delete a user account and its primary group, use the following syntax:

Deleting an account does not remove their respective home folder. It is up to you whether or not you wish to delete the folder manually or keep it according to your desired retention policies.
Remember, any user added later on with the same UID/GID as the previous owner will now have access to this folder if you have not taken the necessary precautions.
You may want to change these UID/GID values to something more appropriate, such as the root account, and perhaps even relocate the folder to avoid future conflicts:

```
sudo deluser username
```

* To temporarily lock or unlock a user account, use the following syntax, respectively:

```
sudo passwd -l username
```

```
sudo passwd -u username
```
