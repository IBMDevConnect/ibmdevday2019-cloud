# The Ultimate Kubernetes Workshop Series: Introduction to OpenShift

We will be using OpenShift on IBM Cloud for this hands on.

## 1. Sign up on IBM Cloud, using your professional id
https://ibm.biz/BdqEVV

## 2. Deploy app on IBM Cloud, through CI/CD
http://bit.ly/hangman12

## 3. Get the environment for the lab
Request Organizer to get the Openshift Cluster Environment
URL: https://osfoundations.mybluemix.net

## 4. Access commandline through remote shell
```
If you need a cloudshell you can use https://workshop.shell.cloud.ibm.com/ 
```


## 5. Once you get your openshift cluster, we will be doing following lab

https://github.com/IBM/openshift101/tree/master/workshop/exercise-01

S2I Lab

```
Here Source Code is https://github.com/sagar-jadhav/node-hello

And the Builder Image here is nodejs version 8

```

Run from the command line from web

```
oc new-app -i=nodejs:8 https://github.com/sagar-jadhav/node-hello --name=node-hello

```
Last Workshop we did  Lab

```

https://github.com/nheidloff/openshift-on-ibm-cloud-workshops/blob/master/1-understanding-openshift/Part4.md#part-4-deploy-an-application-on-openshift-on-the-ibm-cloud
```

## 5. Check your application is running

Great! Congratulations! You have completed this workshop!

When you're all done, you inform the instructor to stop the environment.