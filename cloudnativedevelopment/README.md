# Cloud Native Development Workshop

We will be using Appsody from Kabanero Project for this hands on.

## 1. Sign up on Docker
https://hub.docker.com/

## 2. Get the environment

https://www.katacoda.com/courses/ubuntu/playground1804

Sign in using your gmail id.


## 3. Check the OS

```
$ cat /etc/os-release
```

output should be

NAME="Ubuntu"

VERSION="18.04.2 LTS (Bionic Beaver)"

ID=ubuntu

ID_LIKE=debian

PRETTY_NAME="Ubuntu 18.04.2 LTS"

VERSION_ID="18.04"

HOME_URL="https://www.ubuntu.com/"

SUPPORT_URL="https://help.ubuntu.com/"

BUG_REPORT_URL="https://bugs.launchpad.net/ubuntu/"

PRIVACY_POLICY_URL="https://www.ubuntu.com/legal/terms-and-policies/privacy-policy"

VERSION_CODENAME=bionic

UBUNTU_CODENAME=bionic



## 4. Install Appsody

Get installable
```
$ wget https://github.com/appsody/appsody/releases/download/0.2.8/appsody_0.2.8_amd64.deb
```

Install Appsody
```
$ sudo apt install ./appsody_0.2.8_amd64.deb

```	

## 5. Create Projects Appsody


First, choose a development stack. To see all the available stacks, run:

```
$ appsody list

```


Create a new directory for your project and run appsody init <stack> to download the template project. 

The following example uses the nodejs-express stack to create a fully functional Appsody project:

```
$ mkdir my-project
$ cd my-project
$ appsody init nodejs-express
```

Start the development container:

```
$ appsody run
```

## 6. Check your application is running

Great! Now the project is running in a docker container, and the container is linked to the project source code on your local system. 

Open New terminal
```
$ curl http://localhost:3000
```

You should see out put as
```
"Hello from Appsody!"
```

## 7. Change the code

Now let's try changing the code. Edit the file app.js to output something other than "Hello from Appsody!". 

When you save the file, Appsody picks up the change and automatically updates the container. 

Run

```
$ curl http://localhost:3000 to see the new message!
```

When you're all done, you can stop the environment.


