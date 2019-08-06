#  :cloud: Create scalable Jupyter Notebook on Cloud :orange_book:

## Made on August, 5, 2019

Simple Step-by-Step Guide to make portable Jupyter Notebook server that is accessible from anywhere and easily scalable on AWS EC2.

## Pre-requisites:

1. Create [AWS Account](https://aws.amazon.com/), you would need a valid Credit Card.

> NOTE: If you are student Sign-Up through [AWS Educate](https://aws.amazon.com/education/awseducate/)

2. If on Windows, install <a href="https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html">PuTTY <img src="https://upload.wikimedia.org/wikipedia/commons/b/b6/PuTTY_icon_128px.png" width=32 height=32/></a>

---

## I. Register for AWS

1. Start with registering for AWS account, if you have already registered, sign-in.

You should now be on **AWS Management Console**, as of now it should look like this:

![](https://i.imgur.com/PGQ7uJk.png)

---

## II. Create VPC

Goal here is to create a safe space to launch our Jupyter Notebook Server, to do so, we will create a our Virtual Private Cloud (VPC) where we will launch the EC2 instance (i.e. Virtual Machine).

Our AWS Architecture should look something like this:

![](AWS_Jupyter_Server.png)



Since, there is no cost for creating VPCs, you can create each for your project or deploying application.

1. Before we start note your **AWS Region** in top-right, mine is `Ohio`, yours could be any one of the following, if you want to change, stick with the closest one.

![](https://i.imgur.com/lNkc2I4.png)

2. Search for `VPC` in **Find Services** search box:

![](https://i.imgur.com/m0bOolS.png)

It should take you to something like this:

![](https://i.imgur.com/VUyU6MY.png)

3. Easiest way from here on is clicking on `Launch VPC Wizard` button, that will take you to:

- Step 1: Selecting a VPC Configuration, where you would click on the blue `Select` button, since we only need a single EC2 instance that is publicly accessible.

![](https://i.imgur.com/hsVm5Tw.png)

> Selecting it, would create a Public Subnet where instances can have inbound connection from the outside world which will allow us to SSH into the servers and access website using HTTP/HTTPS protocols, along with outbound internet access needed to update and download repository/packages for us.

- Step 2: VPC with a Single Public Subnet

> Everything should be left default values, don't change anything expect giving it a VPC name, `Jupyter VPC`.

![](https://i.imgur.com/KJBAe1N.png)

After naming the `VPC`, click on `Create VPC` button. After it sucessfully creates the VPC, you should see a confirmation, in green text, and information to `Launch an Instance into Your Subnet`.

![](https://i.imgur.com/VtxX8PU.png)


- Step 3: Click `Ok` and navigate back to **AWS Management Console**.

---

## III. Provision an EC2 Instance

1. Search for `EC2` in the search box from the **AWS Management Console**, which should take you to, **EC2 Dashboard**:

![](https://i.imgur.com/Bk2hdXo.png)

2. Again the easiest way is to click on the big blue button, `Launch Instance`,

- Step 1. Choose an Amazon Machine Image (AMI)

Since, we would be working with `Linux`, Ubuntu is my preference for installing necessary packages and Anaconda distributions.

![](https://i.imgur.com/ACTN4HC.png)

> Make sure the radio-button is selected to `64-bit (x86)`

- Step 2. Choose an Instance Type

Leave everything here default (as is), and click on the button highlighted in the red box below:

![](https://i.imgur.com/3mOWanT.png)

> Leave the selection to default `t2.micro` as it is `free-tier`, and since we would just be installing packages and configuring the instance we don't need a powerful server yet. For more information on pricing check out this link: 
https://aws.amazon.com/ec2/pricing/on-demand/

- Step 3: Configure Instance Details

> Play close attention to highlted red boxes in image below, you should match those settings.

![](https://i.imgur.com/2oP65hC.png)

**Settings that needs to be changed:**

  - a. Under `Network` drop-down box, select our `Jupyter VPC`
  - b. Enable `Auto-assign Public IP`
  - c. Advanced Details / Boostrap Scripts
 :point_right: Not done yet, *scroll all the way down to bottom of the page*, and un-collapse `Advanced Details` tab, which should drop down text-box called `User data`.
 
![](https://i.imgur.com/7O4LDL3.png)

:point_right: Copy paste below commands into `User data` text-box.

```bash
#!/bin/bash
curl https://repogen.simplylinux.ch/txt/bionic/sources_f2be9cfad2f632b6f9d6fc0eea6ad4a35d70929d.txt | sudo tee /etc/apt/sources.list
sudo apt-get update -y
sudo apt-get install git -y
sudo apt-get install ufw -y
sudo ufw allow ssh
sudo apt autoremove -y
```
> This will run the installations we need prior to SSH'ng to EC2 instance.

:point_right: Click, `Next: Add Storage` button

- Step 4. Add Storage *(Optional)*

**Optional Settings you can change:**

- Update Size column from 8 GB to 30 GB of SSD, leave everything else to defaults.

> Free tier eligible customers can get up to 30 GB of SSD storage, so what are you waiting for.

![](https://i.imgur.com/HwMVyiB.png)

:point_right: Click, `Next: Add Tags` button

- Step 5. Add Tags *(Optional)* 

- I prefer organizing multiple instances with proper name tag, so let's add Key, Value tags like below:

**Key:** `Name`
**Value:** `Jupyter EC2 Server`

![](https://i.imgur.com/1OcCZgp.png)

:point_right: Click, `Next: Configure Security Group` button

- Step 6: Configure Security Group

:rotating_light: :warning: **PAY ATTENTION TO THIS STEP** :warning: :rotating_light:


