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

  - **Key:** `Name`
  - **Value:** `Jupyter EC2 Server`

![](https://i.imgur.com/1OcCZgp.png)

:point_right: Click, `Next: Configure Security Group` button

- Step 6: Configure Security Group

:rotating_light: :warning: **PAY ATTENTION TO THIS STEP** :warning: :rotating_light:

We will be creating a **new** security group, so make sure you follow below settings:

  - Assign a security group: `Create a new security group` radio button selected
  - Security group name: `jupyter-ec2-sg`
  - Add Rule: `HTTP` and `HTTPS`
  - Source: `Anywhere` for all the rows
 
 Exactly like this:
 
 ![](https://i.imgur.com/uamTqUq.png)
 
 Once, you have configured exactly like have, move onto next step, which is **Reviewing and Launching** our EC2 instance.
 
 :point_right: Click `Review and Launch` button
 
- Step 7: Review Instance Launch

:point_right: Click `Launch` button after you have verified the settings are correct, and matches based on steps we took.

- Step 8: Create a new Key Pair or Select an existing Key Pair

:rotating_light: :warning: **PAY ATTENTION TO THIS STEP** :warning: :rotating_light:

Unless this is not your first time, you will be prompted to create a new **Key Pair**.

  - Create a new key pair and make sure you name it `[name]-[region]-kp`, example, `jupyterec2-ohio-kp`, some thing like that.
  - :warning: **Download Key Pair, and Store in non-shared folder/drive** :warning:, don't loose it.
  
- Step 9. Wait

Once, you have sucessfully launched the EC2 instance you should see something like this.

:point_right: Select, `View Instances` button

3. Once your are on **EC2 Dashboard** wait till Instance State is indicated with green light :green_heart:.

## IV. Provision the Jupyter Server

1. SSH into EC2

SSH means "secure shell", where you can remotely connect your server and type commands into.

For: 
  - Mac / Linux users: use Terminal
	
      - Step 1. Locate your ec2 ip in `Description` tab under `Public DNS(IPv4)`.
    ![](https://i.imgur.com/e2ox50X.png)
     - Step 2. Note
       - Denotes: 
         -  `<your_ec2_ip>`
         -  `<your_private_key>.pem`
    - Step 3. Navigate to your SSH key folder and type following commands to SSH into EC2
      ```bash
      $ cd path/to/my/ssh-key-folder
      $ chmod 400 <your_private_key>.pem
      $ ssh ubuntu@<your_ec2_ip> -i <your_private_key>.pem
      ```
      
  - Windows users: use PowerShell or PuTTy
  
  :warning: For Windows users, before you connect your private key that you created needs to be converted to PuTTY format using PuTTY gen.

  Follow the **Prerequisites** part of this guide: https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/putty.html

2. Install Anaconda3 Linux

- Step 1. Verify the installs and upgarde the packages
```bash
$ sudo apt-get upgrade -y
```

- Step 2. Follow the tutoral link below to install **Anaconda** on Ubuntu

https://www.digitalocean.com/community/tutorials/how-to-install-anaconda-on-ubuntu-18-04-quickstart

For more information on Anaconda Environments:

[A Guide to Conda Environments](https://towardsdatascience.com/a-guide-to-conda-environments-bc6180fc533)

- Step 3. :orange_book: Generate Jupyter Notebooks Config file :page_facing_up:

In the shell run the following command:

`jupyter notebook --generate-config`

Which will generate a configuartion file we need to custom deploy and run our jupyter notebook server. If it fail's, re-try the installations, as you might not have done correctly.

Config. file should be loated under:

`/home/ubuntu/.jupyter`

do, `ls -l` to locate:

`jupyter_notebook_config.py`

Full path is: `/home/ubuntu/.jupyter/jupyter_notebook_config.py`

- Step 4. Generate Password to Secure Jupyter Notebook

Since we will be accessing our jupyter notebook environment remotely, we need to password protect it from unauthorized access.

:star: Running the following command will generate an encrypted string, which make sure to save it.

`ipython -c "from notebook.auth import passwd; passwd()"`

Will give output like this:
```bash
Out[1]: 'sha1:6f9dfc6a43be:26332e572903d30b6a490ff2ff0c36ca26649f39'
```
 :point_up_2: Copy paste and save it somewhere safe.
 
 - Step 5. Generate SSL Certificates
   - Below commands will navigate to home folder, make hidden `certs` directory, and generate
     SSL certificates that are required to secure the Jupyter Notebook.
      ```
      cd ~  
      mkdir .certs  
      cd .certs   
      openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout mykey.key -out mycert.pem   
      pwd  
      ```
      **Note down the path to certs**
      If you get lost read docs here: 
https://jupyter-notebook.readthedocs.io/en/stable/public_server.html#using-ssl-for-encrypted-communication

- Step 6. Configuring Jupyter Notebook Configuration

We will add the hashed password generated above, in addition to certificates.

Open it using, `nano` editor,

`nano jupyter_notebook_config.py`

Add following lines of code underneath the `# Configuration file for jupyter-notebook`.

```
# Set options for certfile, ip, password, and toggle off
# browser auto-opening
c.NotebookApp.certfile = u'/absolute/path/to/your/certificate/mycert.pem'
c.NotebookApp.keyfile = u'/absolute/path/to/your/certificate/mykey.key'

# Set ip to '*' to bind on all interfaces (ips) for the public server
c.NotebookApp.ip = '*'

# Generated hashed password
c.NotebookApp.password = u'sha1:<<PASTE THE HASH HERE>>'

# Turns off auto-opening of browser
c.NotebookApp.open_browser = False

# It is a good idea to set a known, fixed port for server access
c.NotebookApp.port = 8888
```
Save it with `Ctrl + O`, `Y` 

To further secure we will enable `UFW` firewall in ubuntu:

```
sudo ufw allow 80
sudo ufw allow 443
sudo ufw allow 8888
```
> Finally run the notebook with `jupyter notebook&` and acess with really long DNS NAME: `ec2-public-ip4-domain-name:8888` to access the notebook
     
   - Step 7. To sweeten the deal, let's be lazy and **AutoStart the Notebook** everytime the instance boots up,   
   
   - To auto start Jupyter Notebook you would have to use `Systemctl` config like this:
```console
	[Unit]
	Description=Jupyter Notebook

	[Service]
	Type=simple
	PIDFile=/run/jupyter.pid
	ExecStart=/home/ubuntu/anaconda3/bin/jupyter-notebook --config=/home/ubuntu/.jupyter/jupyter_notebook_config.py
	User=ubuntu
	Group=ubuntu
	WorkingDirectory=/home/ubuntu
	Restart=always
	RestartSec=10
	#KillMode=mixed

	[Install]
	WantedBy=multi-user.target
```

> NOTE: you need a root access

Once you have filled the paths and satisfied,

- Navigate to `cd /etc/systemd/system`
- Do `sudo nano jupyter.service` and copy paste above config
- Save the confif from nano with `Ctrl + O`, than do,
- Enable: `sudo systemctl enable jupyter.service`
- Check Status: `systemctl status jupyter.service`
- Go to `ec2-public-ip4-domain-name:8888` to access the notebook

## INSTALL VS CODE, WINDOWS 10

[Visual Studio Code - Code Editing. Redefined](https://code.visualstudio.com/)

![](https://code.visualstudio.com/assets/docs/remote/ssh/architecture-ssh.png)

Follow Remote Connection Instruction here if you get lost: https://code.visualstudio.com/docs/remote/ssh

#### 1. Enable SSH following link here if your `ssh` command is not working in PowerShell [Installation of OpenSSH For Windows Server 2019 and Windows 10](https://docs.microsoft.com/en-us/windows-server/administration/openssh/openssh_install_firstuse)

#### 2. 
On Windows, in your [HOME] folder, make a `.ssh` directory and add your `pem` file than convert it to `.openssh` using [`PuTTY`](https://code.visualstudio.com/docs/remote/troubleshooting#_reusing-a-key-generated-in-puttygen) along with `config` file which 
should look like this:
```
# Read more about SSH config files: https://linux.die.net/man/5/ssh_config
Host ec2.long.dns.name.com
    HostName ec2.long.dns.name.com
    User ubntu
    IdentityFile C:\Users\$HOME\.ssh\ec2-server-ssh-key.openssh
```

IF YOU GET LOST: https://code.visualstudio.com/docs/remote/ssh

- [OPTONAL] ;) If you want to use your domain name i.e. `example.com:8888` to access notebook:

  Follow instructions here: 

   https://www.digitalocean.com/community/tutorials/how-to-use-certbot-standalone-mode-to-retrieve-let-s-encrypt-ssl-certificates-on-ubuntu-16-04

   https://janakiev.com/blog/jupyter-notebook-server/

   ```console
    sudo add-apt-repository ppa:certbot/certbot
    sudo apt-get install certbot -y
    ```
    
  
    
    







