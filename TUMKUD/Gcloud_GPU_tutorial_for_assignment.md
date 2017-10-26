# Google Cloud Tutorial for Homework 4
Many parts in this tutorial is modified from Stanford's [CS231n's tutorial](http://cs231n.github.io/gce-tutorial/)

# BEFORE WE BEGIN
## BIG REMINDER: Make sure you stop your instances!

(We know you won’t read until the very bottom once your assignment is running, so we are printing this at the top too since it is super important)

Don’t forget to stop your instance when you are done (by clicking on the stop button at the top of the page showing your instances), otherwise you will run out of credits and that will be very sad. :(

If you follow our instructions below correctly, you should be able to restart your instance and the downloaded software will still be available.

![alt text](https://github.com/ekapolc/cattern/raw/master/common/images/forgot.jpg "I forgot to shut down my instance")

# Create and Configure Your Account

For class project and assignments, we will use Google Compute Engine for developing and testing your implementations. This tutorial lists the necessary steps of working on the assignments using Google Cloud. We expect this tutorial to take about an hour. Don’t get intimidated by the steps, we tried to make the tutorial detailed so that you are less likely to get stuck on a particular step. If you have any questions, ask in Piazza.

This tutorial goes through how to set up your own Google Compute Engine (GCE) instance to work on the assignments. When you sign up for the first time, you also receive $300 credits from Google by default. Please try to use the resources judiciously. 

First, if you don’t have a Google Cloud account already, create one by going to the Google Cloud homepage and clicking on Compute. When you get to the next page, click on the blue TRY IT FREE button. If you are not logged into gmail, you will see a page that looks like the one below. Sign into your gmail account or create a new one if you do not already have an account.

![alt text](https://github.com/ekapolc/cattern/raw/master/common/images/cloud-launching-screen.png "Google account signup")

If you already have a gmail account, it will direct you to a signup page which looks like the following.

![alt text](https://github.com/ekapolc/cattern/raw/master/common/images/cloud-for-free.png "Gcloud signup")

Click the appropriate **yes** or **no** button for the first option, and check **yes** for the latter two options after you have read the required agreements. Press the blue **Agree and continue** button to continue to the next page to enter the requested information (your name, billing address and credit card information). Once you have entered the required information, press the blue **Start my free trial** button. You will be greeted by a page like this: 

![alt text](https://github.com/ekapolc/cattern/raw/master/common/images/cloud-dashboard-screen.png "Gcloud dashboard")

To change the name of your project, click on **Manage project settings** on the **Project info** button and save your changes. 

![alt text](https://github.com/ekapolc/cattern/raw/master/common/images/cloud-instance-dashboard-screen.png "cloud-instance-dashboard-screen")

Then, download the Google Cloud SDK that is appropriate for your platform from [here](https://cloud.google.com/sdk/docs/) and follow their installation instructions. **NOTE: this tutorial assumes that you have performed step #4 on the website which they list as optional**. When prompted, make sure you select asia-east1-a as the time zone.

## Requesting GPU Quota Increase ##
To start an instance with a GPU (for the first time) you first need to request an increase in the number of GPUs you can use. To do this, go to your console, click on the **Computer Engine** button and select the **Quotas** menu. You will see a page that looks like the one below.

![alt text](https://github.com/ekapolc/cattern/raw/master/common/images/google-cloud-quotas-screen-upgrade.png "google-cloud-quotas-screen-upgrade.png")

Click on the upgrade account and finish confirm the process. Before you upgrade the account, your instances will be automatically shutdown if you exceed your 300 free trial credits. However, **after upgrading, you will be automatically charged after you used up your credits**. However, this is necessary in order to get access to GPU instances. After upgrading, your screen should look similar to the one shown below.

![alt text](https://github.com/ekapolc/cattern/raw/master/common/images/google-cloud-quotas-screen.png "google-cloud-quotas-screen.png")

Click on the blue **Request Increase** button. This opens a new tab with many quota information. Select **Google Compute Engine API NVIDIA K80 GPUs**. Then, press on the **EDIT QUOTAS** button above. This will open a tab on the right side. Enter the necessary information as shown below

![alt text](https://github.com/ekapolc/cattern/raw/master/common/images/google-cloud-quotas-screen-step1.png "google-cloud-quotas-screen-step1.png")

Click Next, then enter 1 for the quota number, enter the justification as shown. Submit the Request. 

![alt text](https://github.com/ekapolc/cattern/raw/master/common/images/google-cloud-quotas-screen-step2.png "google-cloud-quotas-screen-step2.png")

Once you have your quota increase you can just use GPUs (without requesting a quota increase). After you submit the form, you should receive an email approving your quota increase shortly after (I received my email within a minute). If you don't receive your approval within a day, please inform the instructor. Once you have received your quota increase, you can start an instance with a GPU.

**NOTE: Use your GPUs sparingly because they are expensive. See the pricing [here](https://cloud.google.com/compute/pricing#gpus "title").**

## Creating a GPU Instance ##

After you recieved a confirmation email for the quota increase, you can now start a GPU instance.

Setting up a GPU instance from scratch is time consuming (took me half a day), so we provide you with a VM image that you can use. GCloud does not support publically shared images (yet), but it can be shared to specific people. In order for us to be able to share the image, post your email in the specific discusssion topic on Piazza.

After getting access to the image, use gcloud SDK to create the instance with the command

```
gcloud compute instances create homework4 --image cattern --image-project august-ensign-168512 --no-boot-disk-auto-delete --machine-type n1-standard-8 --zone asia-east1-a
```

If this completes successfully, you should see an instance in the GCloud web console.

## Setting up the machine ##

The current machine has no gpu in it. We need to do one additional tweak to add a GPU to the instance. To edit the virtual instance, go to the **Compute Engine** menu on the left column of your dashboard (the cloud website) and click on **VM instances**. You should see an instance name **homework4**. Click on the 3 dots next to homework4, select **stop** to stop the instance. Stopping the instance shut downs the machine, but you can turn it back on (You will be charged only when the machine is turned on. You can also delete the instance after you are done with the assignment afterwards.) Once the instance is stopped, the green color in front of the instance name should become grey as shown on the picture below.

![alt text](https://github.com/ekapolc/cattern/raw/master/common/images/google-cloud-start-stop-instance.png "google-cloud-start-stop-instance.png")

Click on the instance name (homework4) to edit the instance. Here, we will add a gpu and do some additional tweaking. Click on **EDIT** at the top right next to VM instance details. Under **machine type** click **customize**. Select **GPUs**, change the number of **GPUs** to **1** and **GPU type** to **NVIDIA Tesla K80**. Scroll down to Firewalls, click **Allow HTTP traffic** and **Allow HTTPS traffic**. Click save to finish. Your setting should be similar to what shown below.

![alt text](https://github.com/ekapolc/cattern/raw/master/common/images/google-cloud-edit-instance.png "google-cloud-edit-instance.png")

## Connect to Your Virtual Instance ##
Now that you have created your virtual GCE, you want to be able to connect to it from your computer. The rest of this tutorial goes over how to do that using the command line (Gcloud SDK). The easiest way to connect is using the gcloud compute command below. The tool takes care of authentication for you. On OS X, run:

```
./<DIRECTORY-WHERE-GOOGLE-CLOUD-IS-INSTALLED>/bin/gcloud compute ssh --zone=asia-east1-a <YOUR-INSTANCE-NAME>
```

where <YOUR-INSTANCE-NAME> should be homework4. See [this page](https://cloud.google.com/compute/docs/instances/connecting-to-instance) for more detailed instructions. You are now ready to work on Google Cloud. 

You should be logged in with the same name as your username on your local computer. We have setup the development environment under my user account (ekapolc). To switch user, do

```
sudo su ekapolc
```

You will notice that you are now under my user account.

## Checking your GPU ##

First, let's check your GPU. Do

```
nvidia-smi
```

You should see something like the screen below:

![alt text](https://github.com/ekapolc/cattern/raw/master/common/images/nvidia-smi.png "nvidia-smi.png")

The top table shows the GPUs available. You should see one Telsa K80, called GPU number 0. The bottom table shows the processes that use GPUs. You can verify if things are running properly on the GPU by monitoring nvidia-smi.

## Virtualenv

We have already installed Jupyter (and tensorflow, keras, etc.) on virtualenv. Virtualenv is a virtual environment for python, so that you can have different setups of python on the same machine. To activate the virtualenv do

```
source .env/bin/activate
```

under ekapolc user account, and run 

```
deactivate
```
to exit the venv. This is a python3 environment.

## Using Jupyter Notebook with Google Compute Engine ##

Many of the assignments will involve using Jupyter Notebook. Below, we discuss how to run Jupyter Notebook from your GCE instance and use it on your local browser.

### Getting a Static IP Address ###
Change the Extenal IP address of your GCE instance to be static (see screenshot below). 

![alt text](https://github.com/ekapolc/cattern/raw/master/common/images/cloud-external-ip.png "cloud-external-ip.png")

To Do this, click on the 3 line icon next to the **Google Cloud Platform** button on the top left corner of your screen, go to **Networking** and **External IP addresses** (see screenshot below).

![alt text](https://github.com/ekapolc/cattern/raw/master/common/images/cloud-networking-external-ip.png "cloud-networking-external-ip.png")

To have a static IP address, change **Type** from **Ephemeral** to **Static**. Enter your preffered name for your static IP, mine is assignment-1 (see screenshot below). And click on Reserve. Remember to release the static IP address when you are done because according to [this page](https://jeffdelaney.me/blog/running-jupyter-notebook-google-cloud-platform/) Google charges a small fee for unused static IPs. **Type** should now be set to **Static**. 

![alt text](https://github.com/ekapolc/cattern/raw/master/common/images/cloud-networking-external-ip-naming.png "cloud-networking-external-ip-naming.png")

Take note of your Static IP address (circled on the screenshot below). I used 104.196.224.11 for this tutorial.


![alt text](https://github.com/ekapolc/cattern/raw/master/common/images/cloud-networking-external-ip-address.png "cloud-networking-external-ip-address.png")

### Adding a Firewall rule ###
One last thing you have to do is adding a new firewall rule allowing TCP acess to a particular \<PORT-NUMBER\>. I usually use 7000 or 8000 for \<PORT-NUMBER\>. Click on the 3 line icon at the top of the page next to **Google Cloud Platform**. On the menu that pops up on the left column, go to **Networking** and **Firewall rules** (see the screenshot below). 

![alt text](https://github.com/ekapolc/cattern/raw/master/common/images/cloud-networking-firewall-rule.png "cloud-networking-firewall-rule.png")

Click on the blue **CREATE FIREWALL RULE** button. Enter whatever name you want: I used assignment1-rules. Enter 0.0.0.0/0 for **Source IP ranges** and tcp:\<PORT-NUMBER\> for **Allowed protocols and ports** where \<PORT-NUMBER\> is the number you used above. Click on the blue **Create** button. See the screen shot below.

![alt text](https://github.com/ekapolc/cattern/raw/master/common/images/cloud-networking-firewall-rule-create.png "cloud-networking-firewall-rule-create.png")

**NOTE:** Some people are seeing a different screen where instead of **Allowed protocols and ports** there is a field titled **Specified protocols and ports**. You should enter tcp:\<PORT-NUMBER\> for this field if this is the page you see. Also, if you see a field titled **Targets** select **All instances in the network**.

### Configuring Jupyter Notebook ###
The following instructions are excerpts from [this page](https://haroldsoh.com/2016/04/28/set-up-anaconda-ipython-tensorflow-julia-on-a-google-compute-engine-vm/) that has more detailed instructions.

The Jupyter configuration file `jupyter_notebook_config.py` is the default config file, jupyter uses when starting. We will create one by (assuming you are login as ekapolc and already activate the virtualenv)

```
rm ~/.jupyter/jupyter_notebook_config.py
jupyter notebook --generate-config
```

Using your favorite editor (vim, emacs etc...) add the following lines to the config file, (e.g.: /home/ekapolc/.jupyter/jupyter_notebook_config.py):

```
c = get_config()

c.NotebookApp.ip = '*'

c.NotebookApp.open_browser = False

c.NotebookApp.port = <PORT-NUMBER>
```

Where \<PORT-NUMBER\> is the same number you used in the prior section. Save your changes and close the file. 

### Securing your Jupyter Notebook server ###

In this section, we will add a password to your Jupyter Notebook server (otherwise anyone that knows the ip and port can access your Jupyter server and do nasty things to it).

We start by setting up a password. Do this running a simple script we provide. You will be asked for a password twice, then it will generate a hashed verion of your password.
```
wget --no-check-certificate https://raw.githubusercontent.com/ekapolc/cattern/master/TUMKUD/mkpassword.py
python mkpassword.py
```
Take note of the output (sha1:XXXXXXXXXXX). 

Update `jupyter_notebook_config.py` with
```
c.NotebookApp.password = 'sha1:XXXXXXXXXXX
```

Even if we require a password to login, someone can still get access to our password if our connection is non-encrypted. You can start the notebook to communicate via a secure protocol mode by setting the certfile option to a self-signed certificate which can be easily created by

```
openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout mykey.key -out mycert.pem
```

You will be asked with many prompts, but you can keep hitting Enter until it is done.

Then move your cert and key to .jupyter

```
mv mykey.key ~/.jupyter/
mv mycert.pem ~/.jupyter/
```

Update your `jupyter_notebook_config.py` with

```
c.NotebookApp.keyfile = '/home/ekapolc/.jupyter/mykey.key'
c.NotebookApp.certfile = '/home/ekapolc/.jupyter/mycert.pem'
```

Now you are done with the setup and ready to access your Notebook

### Launching and connecting to Jupyter Notebook ###
The instructions below assume that you have SSH'd into your GCE instance using the prior instructions, and have successfully configured Jupyter Notebook.

If you haven't already done so, activate your virtualenv by running:

```
source .env/bin/activate
```

Launch Jupyter notebook using:

```
jupyter-notebook --no-browser --port=<PORT-NUMBER> 
```

Where \<PORT-NUMBER\> is what you wrote in the prior section.

On your local browser, if you go to https://\<YOUR-EXTERNAL-IP-ADDRESS>:\<PORT-NUMBER\>, you will be asked for a password, and then you should see something like the screen below. My value for \<YOUR-EXTERNAL-IP-ADDRESS\> was 104.196.224.11 as mentioned above. You should now be able to start working on your assignments.

![alt text](https://github.com/ekapolc/cattern/raw/master/common/images/jupyter-screen.png "jupyter-screen.png")

If you do not see this, there are several pitfalls.

1. Not using https when connecting.
2. If your key and cert files are mis-configured, the Jupyter page will fail to load. Double check that you points to the correct files.

## Submission: Transferring Files From Your Instance To Your Computer ##
Once you are done with your assignments, zip the files you need. Then, copy the file to your local computer using the gcloud compute copy-file command as shown below. **NOTE: run this command on your local computer**:

```
gcloud compute copy-files [INSTANCE_NAME]:[REMOTE_FILE_PATH] [LOCAL_FILE_PATH]
```

For example, to copy my files to my desktop I ran:

```
gcloud compute copy-files instance-2:~/answers.zip ~/Desktop
```
Another (perhaps easier) option proposed by a student is to directly download the zip file from Jupyter. After creating answers.zip, you can download that file directly from Jupyter. To do this, go to Jupyter Notebook and click on the zip file (in this case answers.zip). The file will be downloaded to your local computer. 

You can refer to [this page](https://cloud.google.com/compute/docs/instances/transfer-files "Title") for more details on transferring files to/from Google Cloud.

# BIG REMINDER: Make sure you stop your instances! #
Don't forget to stop your instance when you are done (by clicking on the stop button at the top of the page showing your instances). You can restart your instance and the downloaded software will still be available. 

# BIG REMINDER2: Cleaning up your usage! #
After you are done with your assignments, you should delete the instance, delete the disk (under **Disks** in Compute Engine), and release the static IP.
