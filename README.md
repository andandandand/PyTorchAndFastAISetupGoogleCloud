# PyTorch 1.0.0 setup for Google Cloud Platform

This guide explains how to set up Google Cloud Platform (GCP) to use PyTorch 1.0.0 and FastAI 1.0.2. At the end of this tutorial you will be able to use both with Jupyter Lab:

![](https://cdn-images-1.medium.com/max/1000/1*AKAQ25dYfnYnY0gKzcsWKw.png)

There are two ways to achieve this, one via the UI and the other via command line. 


## Google Cloud Platform Account
In case you don't have a GCP account yet, you can create one [here](https://cloud.google.com/),  which comes with $300 worth of usage credits for free. 

+ **Potential roadblock**: Even though GCP provides a $300 initial credit, you have to enable billing to use it. If you register with a bank account or credit card, it might take 3-5 days for the confirmation message to arrive.

Furthermore, the project on which you are going to run the image needs to be linked with your billing account. For this navigate to the [dashboard](https://console.cloud.google.com/home/dashboard) of your project with the dropdown at the top. In the left side menu pick `Compute Engine` and then `Settings`.

<!--- ![](https://raw.githubusercontent.com/andandandand/images-for-colab-notebooks/master/enable-billing.png) 

![create billing account](https://raw.githubusercontent.com/andandandand/images-for-colab-notebooks/master/create-billing-account.png)


![Verify your bank account](https://raw.githubusercontent.com/andandandand/images-for-colab-notebooks/master/verify-your-bank-account-gcp.png)-->


## Option 1: Google Cloud Marketplace


1. Go to the [marketplace page of Deep Learning images](https://console.cloud.google.com/marketplace/details/click-to-deploy-images/deeplearning
).
2. Click "launch on compute engine".
3. Select a project, if you want/need to add a new one you can do that with the "+" button on the top right. 
4. Select `Pytorch 1.0 Preview/FastAi 1.0` in the `Frameworks` section, fill out the rest and click `Deploy`.


## Option 2: Command line

If you have not yet, install Google Cloud's command line interface (CLI) following [this guide](https://cloud.google.com/sdk/docs/#install_the_latest_cloud_tools_version_cloudsdk_current_version). You must download and install the SDK on your system and initialize it before you can use `gcloud`. 

For Debian/Ubuntu there is an easier setup than the one provided in the linked guide above: `curl https://sdk.cloud.google.com | bash && exec -l $SHELL && gcloud init`
### Start an instance on Linux/macOS: 

Paste and run the following in your terminal:

`export IMAGE_FAMILY="pytorch-1-0-cu92-experimental" export ZONE="us-west1-b" export INSTANCE_NAME="pytorch-and-fastai-test" gcloud compute instances create $INSTANCE_NAME         --zone=$ZONE --image-family=$IMAGE_FAMILY         --machine-type=n1-standard-8 --image-project=deeplearning-platform-release         --maintenance-policy=TERMINATE --accelerator="type=nvidia-tesla-p100,count=1" --metadata="install-nvidia-driver=True"`

### Start an instance on Windows 

Paste and run the following in your cmd:

`gcloud compute instances create "pytorch-and-fastai-test" --zone="us-west1-b" --image-family="pytorch-1-0-cu92-experimental" --machine-type=n1-standard-8 --image-project=deeplearning-platform-release --maintenance-policy=TERMINATE --accelerator="type=nvidia-tesla-p100,count=1" --metadata="install-nvidia-driver=True"`

## How To Use Jupyter Lab
First, you need to install `gcloud` cli. 

See Option 2: Command line above. 

>There is a way to use Jupyter Lab without installing gcloud locally, it can be found [here](https://blog.kovalevskyi.com/semi-managed-jupyter-lab-with-access-to-google-cloud-resources-cc6f9e439416).

As soon as your instance is created you can use SSH to connect to it:

``gcloud compute ssh $INSTANCE_NAME -- -L 8080:localhost:8080``

* Replace $INSTANCE_NAME with **your** actual instance's name, e.g.:

``gcloud compute ssh pytorch-instance -- -L 8080:localhost:8080``

and open your browser at http://localhost:8080 

That's it! You will be able to access the preloaded notebooks and write new ones using PyTorch and FastAI. 

![](https://raw.githubusercontent.com/andandandand/images-for-colab-notebooks/master/jupyterlab-screenshot.png)

## References

+ [Setting up PyTorch and Fast.ai in GCP](https://blog.kovalevskyi.com/google-compute-engine-now-has-images-with-pytorch-1-0-0-and-fastai-1-0-2-57c49efd74bb)

+ [Launching a PyTorch Deep Learning VM Instance](https://cloud.google.com/deep-learning-vm/docs/pytorch_start_instance)

+ [Google Cloud SDK Quickstart for Debian and Ubuntu](https://cloud.google.com/sdk/docs/quickstart-debian-ubuntu)

+ [Installing the latest Cloud SDK version](https://cloud.google.com/sdk/docs/#install_the_latest_cloud_tools_version_cloudsdk_current_version)

+ [Installing Google Cloud SDK (StackOverflow question)](https://stackoverflow.com/questions/46822766/sudo-apt-get-update-sudo-apt-get-install-google-cloud-sdk-cannot-be-done)
+ https://stackoverflow.com/a/47908542/45963

