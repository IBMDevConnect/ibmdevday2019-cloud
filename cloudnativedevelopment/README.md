# Cloud Native Development Workshop

We will be using Appsody from Kabanero Project for this hands on.

## 1. Sign up on Docker
https://hub.docker.com/

## 2. Get the environment

https://www.katacoda.com/courses/ubuntu/playground1804

Sign in using your gmail id.

## 3. Check the Operating System in the Katakoda environment

```
$ cat /etc/os-release
```

output should be

```
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
```


## 4. Install Appsody

Get installable
```
$ wget https://github.com/appsody/appsody/releases/download/0.5.9/appsody_0.5.9_amd64.deb
```

Install Appsody
```
$ sudo apt install ./appsody_0.5.0_amd64.deb

```	

## 5. Add Kabanero Collection to appsody

Use the appsody CLI to add the Collection repo.
```
$ appsody repo add kabanero https://github.com/kabanero-io/collections/releases/download/0.3.5/kabanero-index.yaml
```

## 6. Set the kabanero repo as default.

```
$ appsody repo set-default kabanero
```

## 5. Create Projects Appsody

First, choose a development stack. To see all the available stacks, run:

```
$ appsody list kabanero

```


Create a new directory for your project and run appsody init <stack> to download the template project. 

The following example uses the nodejs-express stack to create a fully functional Appsody project:

```
$ mkdir my-project
$ cd my-project
$ appsody init java-spring-boot2
```

Start the development container:

```
$ appsody run
```

## 6. Check your application is running

Great! Now the project is running in a docker container, and the container is linked to the project source code on your local system. 

Open New terminal
```
$ curl http://localhost:8080
```

You should see out put as
```

<!DOCTYPE html>
<html>
  <head>
    <title>Hello from Appsody!</title>
  </head>
  <body>
    <h3>Hello from Appsody!</h3>
    <p>Next steps with Spring Boot 2:
      <ul>
        <li><a target="_blank" rel="noopener" href="https://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/">Spring Boot Reference Guide</a></li>
        <li><a target="_blank" rel="noopener" href="https://spring.io/guides">Spring Guides</a></li>
        <li><a target="_blank" rel="noopener" href="/actuator">Spring Boot Actuator endpoints</a>:
          <ul>
            <li><a target="_blank" rel="noopener" href="/actuator/health">Health</a></li>
            <li><a target="_blank" rel="noopener" href="/actuator/liveness">Liveness</a></li>
            <li><a target="_blank" rel="noopener" href="/actuator/metrics">Metrics</a></li>
            <li><a target="_blank" rel="noopener" href="/actuator/prometheus">Prometheus</a></li>
          </ul>
        </li>
      </ul>
    </p>
  </body>
</html>



```
## 7. Change the code

Now let's try changing the code. Edit the index.html file to output something other than "Hello from Appsody!". 

![codechange](image1.png)

Change from heading
<h3>Hello from Appsody!</h3>
To 
<h3>Hello from Appsody - HCL Workshop!</h3>

When you save the file, Appsody picks up the change and automatically updates the container. 

Run

```
$ curl http://localhost:8080 to see the new message!
```

When you're all done, you can stop the environment.

Observe the change of heading
<h3>Hello from Appsody - HCL Workshop!</h3>


