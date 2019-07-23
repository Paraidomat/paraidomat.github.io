# Running GNS3 in an AWS EC2 Instance

In this ~~Gist~~ Document I want to show you how to get started with GNS3 on an
Amazon AWS EC2 instance.

## Disclaimer

Attention: This ~~may be~~ **is** ~~potentially~~ unsafe!
The Server in the configuration shown is in any case reachable by the public internet and there is no authentication on the GNS3 server configured. But after you've made it through this document it will be easy to add a better security policy.
I myself just added a VPN-Client on the AWS Server and connected it to my router at home.

Attention: This will also eventually cost you a lot of money, should you forget to stop your instances!

## Introduction

I'm assuming that you all are familiar with the basics of networking, basic Ubuntu/Linux and GNS3 as well as having access to at least one Cisco IOS image or a virtual machine of a networking OS you can work with. Before trying to run GNS3 in a client/server architecture i would highly suggest to install and try to run GNS3 locally so you have a good starting point and a basic knowledge of GNS3.

## Creating your Amazon AWS account

In order to create your Amazon AWS account just register a new one on the [Amazon AWS Webpage](https://aws.amazon.com). You will need to provide your contact data and credit card information in order to do this. Amazon will call you on your provided phone number in order to validate your account.

After you've created your account you need to wait a little while until you can start your first machines.

When you receive the mail that notifies you of your account activation we finally can get to work. :)

## Creating your first Machine

After you have successfully created your account you can start your first Amazon EC2 instance. In order to do that you need to go through the following steps:

- Log in to your Amazon AWS account.
- Search for the AWS service `EC2` and click on it. You should be redirected to [the EC2 Dashboard](https://console.aws.amazon.com/ec2/v2).
- Click on the blue `Launch Instance` button. You should now see the `Instance Wizard`.
- Choose the Free tier eligable `Ubuntu Server 16.04 LTS (HVM)`
- Now we choose the Free tier eligable `t2.micro` Server, which is enough to get GNS3 up and running. As an added bonus the installation will cost you nothing.
- Click on the `6. Configure Security Group` button in the navigation bar to the top.
- Choose `Create a *new* security group`
  * Set the Security group name to something like `GNS3`
  * Set a useful description
  * Add another rule with the following data:
     
| Type            | Protocol | Port Range | Source    | Description |
| --------------- | -------- | ---------- | --------- | ----------- |
| Custom TCP Port | TCP      | 3080       | 0.0.0.0/0 | GNS3 API    |

Attention: Please note that this opens this server instance to the "big bad internet".
You should change these rules to your need to be secure.
   
- Click on `Review and launch`
- Click on `Launch` in the bottom right corner.
- Let the Wizard create a new SSH key pair and give it a useful name. 
   * After you've set the name click on the `Download key pair`.
   * Take note of Amazons warning regarding key files:
     > You have to download the **private key file** (\*.pem file) before you 
     > can continue. 
     > **Store it in a secure and accessible location.**
     > You will not be able to download the file again after it's created.
- On the `Launch Status` page click on the `View Instances` button, once the lauch is complete.
- You will be taken back to the EC2 dashboard where you will see your new instance. HOORAY!

## Logging in to your Server using SSH

- Open the EC2 Dashboard and locate your running instance and click on it.
- On the bottom you will presented with the Public DNS (IPv4) name of your Instance. It will look something like `ec2-XXX-XXX-XXX-XXX.compute-X.amazonaws.com`.
- Use your favorit SSH tool and the key file you've downloaded to log in to your server. 
  On a Linux OS you will need to use the 
  `ssh -i $PATH_TO_KEYFILE ubuntu@ec2-XXX-XXX-XXX-XXX.compute-X.amazonaws.com`
  command.
  Accept the fingerprint and you should be taken to the servers bash CLI.

## Install GNS3 Server

In order to install the GNS3 Server you can use GNS3's [documentation](https://docs.gns3.com/1QXVIihk7dsOL7Xr7Bmz4zRzTsJ02wklfImGuHwTlaA4/index.html) with one little change:

Instead of installing the `gns3-gui` package you will need to install the `gns3-server` package:

```
sudo add-apt-repository ppa:gns3/ppa
sudo apt-get update
sudo apt-get install gns3-server
```

While installing GNS3 you will be asked if non-superusers should be able to run GNS3 and you want to choose `<yes>` there.

## Start the GNS3 Server 

If you want to run the GNS3 Server you just have to run the command: `gns3server`. Notice, that this time the hyphen is gone.

### Changing the Log file

You will notice that the `gns3server` is quite... chatty? If you do not want to see that many log messages on your SSH session just create a log file at your favorit position and use the `--log $LOGFILE_PATH` optional argument to make the logs disappear.

## Connect your GNS3 Client to the Server

- Start the GNS3 Server on the EC2 instance.
- Start your local GNS3 client. Note that the Client and the Server need to have the same version.
- Click on `Edit -> Preferences` (or press `[ctrl]+[shift]+[p]`)
- In the navigation on the left hand side click on `Server`
- Untick the `Enable local server` checkbox.
- Add the following data to the form:
   
| Field Name | Data |
| ---------- | ---- |
| Host | `ec2-XXX-XXX-XXX-XXX.compute-X.amazonaws.com` |
| Port | `3080 TCP` |
   
- Select `Apply`
- You should see that the client connects to the server.
- On the right hand side you should see in the `Servers Summary` window, that the internal hostname of the EC2 instance
  appears.
- Add IOS Images using the Preferences and start the machines, as you would with a local GNS3 installation.

If you will try to console in to your routers or devices you will notice that it won't work. This is due to the fact that the security group doesn't allow access on port 5xxx. You can circumcent this in two ways:

- Change the security group.
  * **Pro:** You can just telnet to the Instance on Port 5xxx and you're good to go.
  * **Con:** The routers/devices will be exposed to the public internet.
- Create one or more extra SSH sessions to the EC2 instance and just go with `telnet localhost 5xxx`.
  * **Pro:** Routers/Devices will not be exposed to the public internet.
  * **Con:** It's a little cumbersome.
   
## Okay cool! But what if I need more POWER?!

Okay - That's the whole point of doing all of this, right? **Right!**

So: In order to get more powerful instances you will need to shut down your instance and change the instance type in the EC2 Dashboard. In order to achieve this:

- Right click on the instance in the dashboard. 
- Click on `Instance Settings -> Change Instance Type`
- Start the machine and the GNS3 Server 
- Done!

Should you not be able to change the instance type to something more powerful you should check your instance limits in the EC2 Dashboard. You may need to create a ticket with Amazon AWS Support in order to increase the limits (from 0). At least that what I needed to do. 
