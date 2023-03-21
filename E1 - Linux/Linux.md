# Linux

## EXERCISE 1: Linux Mint Virtual Machine

You can create a Linux Mint Virtual Machine using VirtualBox or VMware. Download Linux Mint ISO from https://www.linuxmint.com/download.php and follow the steps to install it in a virtual environment.

Once you have Linux Mint running, open the terminal and check the following:

Distribution: `cat /etc/*-release`
Package manager: Linux Mint uses `apt` or `apt-get`
CLI editor: By default, `nano` is usually installed, but you can check by typing `nano`, `vi`, or `vim` and see which one opens.
Software center: Linux Mint comes with "Software Manager."
Shell: Check the shell for your user with `echo $SHELL

## EXERCISE 2: Bash Script - Install Java

```bash
#!/bin/bash

sudo apt update
sudo apt install -y default-jdk

java_version=$(java -version 2>&1 | head -n 1 | cut -d '"' -f 2)

if [ -z "$java_version" ]; then
    echo "Java is not installed."
    exit 1
elif [ "$(echo "$java_version" | cut -d '.' -f 1)" -lt 11 ]; then
    echo "An older version of Java is installed: $java_version"
    exit 1
else
    echo "Java version 11 or higher is installed: $java_version"
    exit 0
fi
```

## EXERCISE 3: Bash Script - User Processes

```bash
#!/bin/bash

ps aux | grep "$USER"
```

## #EXERCISE 4: Bash Script - User Processes Sorted

```bash
#!/bin/bash

echo "Choose sorting option: (1) memory or (2) CPU"
read choice

if [ "$choice" = "1" ]; then
    ps aux --sort=-%mem | grep "$USER"
elif [ "$choice" = "2" ]; then
    ps aux --sort=-%cpu | grep "$USER"
else
    echo "Invalid choice"
fi
```

## EXERCISE 5: Bash Script - Number of User Processes Sorted

```bash
#!/bin/bash

echo "Choose sorting option: (1) memory or (2) CPU"
read choice
echo "Enter the number of processes to display:"
read num

if [ "$choice" = "1" ]; then
    ps aux --sort=-%mem | grep "$USER" | head -n "$num"
elif [ "$choice" = "2" ]; then
    ps aux --sort=-%cpu | grep "$USER" | head -n "$num"
else
    echo "Invalid choice"
fi
```
## EXERCISE 6: Bash Script - Start Node App

```bash
#!/bin/bash

sudo apt update
curl -sL https://deb.nodesource.com/setup_14.x | sudo -E bash -
sudo apt install -y nodejs

node_version=$(node -v)
npm_version=$(npm -v)
echo "Node version: $node_version"
echo "NPM version: $npm_version"

wget https://node-envvars-artifact.s3.eu-west-2.amazonaws.com/bootcamp-node-envvars-project-1.0.0.tgz
tar xzf bootcamp-node-envvars-project-1.0.0.tgz

export APP_ENV=dev
export DB_USER=myuser
export DB_PWD=mysecret

cd bootcamp-node-envvars-project
npm install
node server.js &
```
## EXERCISE 7: Bash Script - Node App Check Status

```bash
#!/bin/bash
# Add this to the previous script after starting the Node app

sleep 5
node_process=$(pgrep -fl "node server.js")

if [ -z "$node_process" ]; then
  echo "Node app did not start successfully."
else
  echo "Node app is running:"
  echo "$node_process"
  app_port=$(netstat -tuln | grep $(pgrep node))
  echo "Listening on port: $(echo $app_port | awk '{print $4}' | cut -d ':' -f 2)"
fi
```

## EXERCISE 8: Bash Script - Node App with Log Directory

```bash
#!/bin/bash
# Replace the `node server.js &` line in Exercise 6 script with the following

if [ -z "$1" ]; then
  echo "No log directory provided. Using default."
else
  log_directory="$1"
  if [ ! -d "$log_directory" ]; then
    mkdir -p "$log_directory"
  fi
  export LOG_DIR=$(realpath "$log_directory")
fi

node server.js > "${LOG_DIR:-.}/app.log" 2>&1 &
```

## EXERCISE 9: Bash Script - Node App with Service User

```bash
#!/bin/bash

# Add this to the beginning of the script in Exercise 8

if ! id -u myapp >/dev/null 2>&1; then
  sudo useradd -m myapp
fi

# Replace the `node server.js > "${LOG_DIR:-.}/app.log" 2>&1 &` line in Exercise 8 script with the following

sudo -u myapp node server.js > "${LOG_DIR:-.}/app.log" 2>&1 &
```
