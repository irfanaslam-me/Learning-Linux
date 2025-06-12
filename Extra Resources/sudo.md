# **sudo** Complete Guide 💻

## Introduction

we know there are almost all of the commands that need to run as **sudo**. which requires system change or needs access as **root**
for example, if I want to update the system as a user **irfan** it will not allow me or will generate an error if I use the command `apt update`
So I need to execute the following command

 ```bash
sudo apt update
```

by default **sudo** is installed in Linux servers we can verify using the command 
```bash
which sudo
```

## sudo group


```bash
cat /etc/sudoers
```
we can check if our user is the member of the group sudo by executing the command

```bash
groups  irfan
```



Let's add **irfan** to the **admins** group using:

```bash
usermod -aG admins irfan
```

However, if attempted as **irfan**, it'll generate an error. So, first, let's make **irfan** a **sudo** user. Log in as **root** and execute:

```bash
usermod -aG sudo irfan
```
After adding to the group, verify user groups with:

```bash
groups irfan
```
Now, **irfan** is a **sudo** user. Let's add them to the **admin** group. First, create the **admins** group:

```bash
sudo groupadd admins
```
Then, add **irfan** to the group:

```bash
usermod -aG admins irfan
```
To add **irfan** to multiple groups, use:
```bash
usermod -aG admins,sudo irfan
```
Verify group addition with:

```bash
groups irfan
```
> This user will be effective in the groups, After signing out or the next login.


## Changing Home Directory
To change the home directory path for the user **irfan**:

```bash
sudo usermod -d /home/myhome --move-home irfan
```

Verify changes post-login with:

```bash
pwd
echo $HOME
```

## Changing Username
To change **irfan's** username to **learnwithirfan**:

```bash
usermod -l learnwithirfan irfan
```
Verify the change using:

```bash
cat /etc/passwd
```

## Managing Account Status

#### To disable the user account:

```bash
usermod -L learnwithirfan
```


#### To enable the user account again:

```bash
usermod -U learnwithirfan
```

To set an expiration date for the account:

```bash
usermod learnwitirfan -e 2024-12-30
```
Verify changes using chage:

```bash
chage -l learnwithirfan
```
