# THIS FILE IS OBSOLETE #
# Google Cloud Tutorial (Part 2 With GPUs) #
This tutorial assumes that you have already gone through the first Google Cloud tutorial [here](https://github.com/ekapolc/cattern/blob/master/TUMKUD/Gcloud_Tutorial.md). The first tutorial takes you through the process of setting up a Google Cloud account, launching a VM instance, accessing Jupyter Notebook from your local computer, and transferring files to your local computer. While you created a VM instance without a GPU in the first tutorial, this one walks you through the necessary steps to create an instance with a GPU, and use provided disk images to work on the assignments (which has the setups to properly use the GPU). If you haven't already done so, we advise you to go through the first tutorial to be comfortable with the process of creating an instance with the right configurations and accessing Jupyter Notebook from your local computer.

## Requesting GPU Quota Increase ##
To start an instance with a GPU (for the first time) you first need to request an increase in the number of GPUs you can use. To do this, go to your console, click on the **Computer Engine** button and select the **Quotas** menu. You will see a page that looks like the one below.

![alt text](https://github.com/ekapolc/cattern/raw/master/common/images/google-cloud-quotas-screen.png "google-cloud-quotas-screen.png")

Click on the blue **Request Increase** button. This opens a new tab with many quota information. Select **Google Compute Engine API NVIDIA K80 GPUs**. Then, press on the **EDIT QUOTAS** button above. This will open a tab on the right side. Enter the necessary information as shown below

![alt text](https://github.com/ekapolc/cattern/raw/master/common/images/google-cloud-quotas-screen-step1.png "google-cloud-quotas-screen-step1.png")

Click Next, then enter 1 for the quota number, enter the justification as shown. Submit the Request. 

![alt text](https://github.com/ekapolc/cattern/raw/master/common/images/google-cloud-quotas-screen-step2.png "google-cloud-quotas-screen-step2.png")

Once you have your quota increase you can just use GPUs (without requesting a quota increase). After you submit the form, you should receive an email approving your quota increase shortly after (I received my email within a minute). If you don't receive your approval within a day, please inform the instructor. Once you have received your quota increase, you can start an instance with a GPU. To do this, while launching a virtual instance as described in the **Launch a Virtual Instance** section [here](https://github.com/ekapolc/cattern/blob/master/TUMKUD/Gcloud_Tutorial.md), select the number of GPUs to be 1 (K80 GPU). As a reminder, you can only use up to the number of GPUs allowed by your quota.

**NOTE: Use your GPUs sparingly because they are expensive. See the pricing [here](https://cloud.google.com/compute/pricing#gpus "title").**

## Creating a GPU Instance ##

After you recieved a confirmation email for the quota increase, you can now start a GPU instance.

Setting up a GPU instance from scratch is time consuming (took me half a day), so we provide you with a VM image that you can use. GCloud does not support publically shared images (yet), but it can be shared to specific people. In order for us to be able to share the image, post your email in the specific discusssion topic on Piazza.

After getting access to the image, use gcloud console tool to create the instance with the command

```
gcloud compute instances create <instance-name> --image cattern --image-project august-ensign-168512 --no-boot-disk-auto-delete --machine-type n1-standard-8 --zone asia-east1-a
```

If this completes successfully, you should see an instance in the GCloud web console.

## Setting up the machine ##

The machine comes pre-installed with CUDA, CuDNN, and other nvidia tools. However, the driver need to be re-installed for every now instance. To do so run

```
wget --no-check-certificate https://raw.githubusercontent.com/ekapolc/cattern/master/TUMKUD/driverinstall.sh
sudo sh ./driverinstall.sh
```
