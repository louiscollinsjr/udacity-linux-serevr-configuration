#### udacity-linux-server-configuration

#### 1. Get Amazon Lightsail Server

#### 2. Connect using your own SSH client

You can connect to the AWS instance using the following address and user name:

- Pulbic IP: 54.145.197.176
- User name: grader
- Application URL: https://www.trott.app
- SSH Connection: ssh -i ~/.ssh/[key file] grader@54.145.197.176 -p 2200



`[keyfile] is the full path of the provided key for the grader account.`



### Secure your server.

- [x] **3. Update all currently installed packages.**

```
$ sudo apt-get update
$ sudo apt-get upgrade
```
