---
title: "Solutions"
weight: 2
---

## EXERCISE 1: Package NodeJS App

```bash
npm install
```

## EXERCISE 2: Create a new server

1. Log in to your DigitalOcean account.
2. Click "Create" in the top-right corner and select "Droplets".
3. Choose an image (for example, Ubuntu 20.04 LTS).
4. Select a plan and datacenter region.
5. Add your SSH key or create a new one.
6. Click "Create Droplet".

## EXERCISE 3: Prepare server to run Node App

For SSH and SCP I use the Setapp app "SSH Config Editor" to manage my SSH keys and hosts. It just creates an alias for
the SSH command and makes it easier to manage.

```bash
ssh droplet
apt update
apt install -y nodejs npm
```
## EXERCISE 4: Copy App and package.json

```bash
scp app/* droplet:/home
scp app/package.json droplet:/home
ssh droplet
tar xf your_app_tar_file
```

## EXERCISE 5: Run Node App

```bash
npm install
nohup node server.js > output.log 2>&1 &

```
## EXERCISE 6: Access from browser - configure firewall

If it would be a regular webserver with no additional UI:

```bash
sudo ufw allow 3000
```

Now, I am able to access the application in browser.
