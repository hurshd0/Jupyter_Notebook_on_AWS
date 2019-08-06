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

Since, there is no cost for creating VPCs, you can create each for your project or deploying application.

1. Before we start note your **AWS Region** in top-right, mine is `Ohio`, yours could be any one of the following, if you want to change, stick with the closest one.

![](https://i.imgur.com/lNkc2I4.png)

2. 


