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

Step 1: Selecting a VPC Configuration, where you would click on the blue `Select` button, since we only need a single EC2 instance that is publicly accessible.

![](https://i.imgur.com/hsVm5Tw.png)

> Selecting it, would create a Public Subnet where instances can have inbound connection from the outside world which will allow us to SSH into the servers and access website using HTTP/HTTPS protocols, along with outbound internet access needed to update and download repository/packages for us.

Step 2: VPC with a Single Public Subnet

> Everything should be left default values, don't change anything expect giving it a VPC name, `Jupyter VPC`.

![](https://i.imgur.com/KJBAe1N.png)

After naming the `VPC`, click on `Create VPC` button. After it sucessfully creates the VPC, you should see a confirmation, in green text, and information to `Launch an Instance into Your Subnet`.

![](https://i.imgur.com/VtxX8PU.png)


Step 3: 