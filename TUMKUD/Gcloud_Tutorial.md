# Google Cloud Tutorial
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

## Launch a Virtual Instance with no GPU ##
This part of the tutorial goes over the step on how you can setup a virtual instance from scratch.

To launch a virtual instance, go to the **Compute Engine** menu on the left column of your dashboard and click on **VM instances**.  Then click on the blue **CREATE** button on the next page. This will take you to a page that looks like the screenshot below. **(NOTE: Please carefully read the instructions in addition to looking at the screenshots. The instructions tell you exactly what values to fill in).**

![alt text](https://github.com/ekapolc/cattern/raw/master/common/images/cloud-create-instance-screen.png "cloud-create-instance-screen.png")

Make sure that the Zone is set to be **us-east1-a** (especially for assignments where you need to use GPU instances). Under **Machine type** pick the **1 vCPU** option. Click on the **customize** button under **Machine type** and make sure that the number of cores is set to 8 and the number of GPUs is set to **None**. Click on the **Change** button under **Boot disk**, choose **OS images**, check **Ubuntu 16.04 LTS** and click on the blue **select** button. Check **Allow HTTP traffic** and **Allow HTTPS traffic**. Click on **disk** and then **Disks** and uncheck **Delete boot disk when instance is deleted** (Note that the "Disks" option may be hiding under an expandable URL at the bottom of that webform). If you select Delete boot disk, if you delete an instance the boot disk will also be deleted. You can lose your work this way, so we recommend unchecking the option. Click on the blue **Create** button at the bottom of the page. You should have now successfully created a Google Compute Instance, it might take a few minutes to start running. Your screen should look something like the one below. When you want to stop running the instance, click on the blue stop button above.

![alt text](https://github.com/ekapolc/cattern/raw/master/common/images/cloud-instance-started.png "cloud-instance-started.png")

Take note of your \<YOUR-INSTANCE-NAME\>, in this case, my instance name is instance-2. 

## Connect to Your Virtual Instance and Download the Assignment ##
Now that you have created your virtual GCE, you want to be able to connect to it from your computer. The rest of this tutorial goes over how to do that using the command line. First, download the Google Cloud SDK that is appropriate for your platform from [here](https://cloud.google.com/sdk/docs/) and follow their installation instructions. **NOTE: this tutorial assumes that you have performed step #4 on the website which they list as optional**. When prompted, make sure you select us-east1-a as the time zone. The easiest way to connect is using the gcloud compute command below. The tool takes care of authentication for you. On OS X, run:

```
./<DIRECTORY-WHERE-GOOGLE-CLOUD-IS-INSTALLED>/bin/gcloud compute ssh --zone=us-east1-a <YOUR-INSTANCE-NAME>
```

See [this page](https://cloud.google.com/compute/docs/instances/connecting-to-instance) for more detailed instructions. You are now ready to work on Google Cloud. 

Run the following command to download a typical setup script for your GCE:

```
wget --no-check-certificate https://raw.githubusercontent.com/ekapolc/cattern/master/TUMKUD/setup_googlecloud.sh
wget --no-check-certificate https://raw.githubusercontent.com/ekapolc/cattern/master/TUMKUD/requirements.txt
```

To install the usual things like jupyter (**NOTE:** you do not need to do this for Thai word segmentation assignment), run the provided shell script: **(Note: you will need to hit the [*enter*] key at all the "[Y/n]" prompts)**

```
./setup_googlecloud.sh
```

You will be prompted to enter Y/N at various times during the download. Press enter for every prompt. If you had no errors, you can proceed to work with your virtualenv as normal.

I.e. run 

```
source .env/bin/activate
```

in your assignment directory to load the venv, and run 

```
deactivate
```
to exit the venv.

**NOTE**: The instructions above will run everything needed using Python 3. If you would like to use Python 2 instead, edit setup_googlecloud.sh to replce the line 

```
virtualenv -p python3 .env 
```

with 

```
virtualenv .env
```

before running 

```
chmod 777 setup_googlecloud.sh
./setup_googlecloud.sh
```

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

On your GCE instance check where the Jupyter configuration file is located:

```
ls ~/.jupyter/jupyter_notebook_config.py
```
Mine was in /home/timnitgebru/.jupyter/jupyter_notebook_config.py

If it doesn’t exist, create one:

```
# Remember to activate your virtualenv ('source .env/bin/activate') so you can actually run jupyter :)
jupyter notebook --generate-config
```

Using your favorite editor (vim, emacs etc...) add the following lines to the config file, (e.g.: /home/timnitgebru/.jupyter/jupyter_notebook_config.py):

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

(Obviously replace ekapolc with your user name.)
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
Another (perhaps easier) option proposed by a student is to directly download the zip file from Jupyter. After creating assignment1.zip, you can download that file directly from Jupyter. To do this, go to Jupyter Notebook and click on the zip file (in this case assignment1.zip). The file will be downloaded to your local computer. 

You can refer to [this page](https://cloud.google.com/compute/docs/instances/transfer-files "Title") for more details on transferring files to/from Google Cloud.

# BIG REMINDER: Make sure you stop your instances! #
Don't forget to stop your instance when you are done (by clicking on the stop button at the top of the page showing your instances). You can restart your instance and the downloaded software will still be available. 

# BIG REMINDER2: Cleaning up your usage! #
After you are done with your assignments, you should delete the instance, delete the disk (under **Disks** in Compute Engine), and release the static IP.
