---
layout: post
title:  "How to Generate a Vanity Bitcoin Address on AWS"
date:   2015-05-25 00:00:00
categories: bitcoin
---
To generating vanity Bitcoin address like _<strong>1Mason</strong>VyvcWgW6KtWVbhW4hdjEKgii3tZ7 you use a [brute force algorythm](http://en.wikipedia.org/wiki/Brute-force_search). Using my 2.6 GHz Macbook Pro I can generate about 150 thousand keys per second. The address generator also comes with a program called [OCLVanitygen](https://github.com/samr7/vanitygen/blob/master/oclvanitygen.c) that runs on the [GPU](http://en.wikipedia.org/wiki/Graphics_processing_unit). With [OCLVanitygen](https://github.com/samr7/vanitygen/blob/master/oclvanitygen.c) I can generate about 1 million keys per second. This means you can geneate a vanity address with 5 charaters in about 6 hours.

I wanted to be able to generate longer addresses without waiting any longer and without buying new hardware. There are [online vanity address generators](https://bitcoinvanitygen.com/) but you can never be sure that those are safe to use.

Since we'll only need this processing power for a few hours max we can rent hardware to generate our address on instead of buying it.
AWS provides virtual machines that have NVIDIA GPUs with up to 1,536 CUDA cores and 4 GB of video memory. You can rent these for 65 cents per hour so it'll cost about 8 bucks if you run it overnight. Don't foget to destroy these instances when you're done though. If you leave them on for a month it could cost nearly $500!

The first thing we’ll need to do is spin up a AWS GPU instance:

1. Login to the [AWS Management Console](https://console.aws.amazon.com/), create an account if you don’t already have one.
2. Click “EC2”
3. Click “Launch Instance”
4. Select Ubuntu Server 14.04 LTS (HVM), SSD Volume Type
5. Select “GPU instances” “g2.2xlarge“ then click “Review and Launch”
6. Click “Launch”
7. Select “Create a new Key pair”, enter a name for the key pair and click “Download Key Pair.” Move the key to your `.ssh` folder.
8. Check the checkbox and click “Launch Instances”
9. Click on the Instance ID and note your instance’s public IP address
10. Open Terminal and type `chmod 400 .ssh/aws-key.pem`
11. Type: `ssh -i .ssh/aws-key.pem ubuntu@<your instance public ip address from step>`

Next we’ll have to build the CUDA drivers which are used by the address generator:


2. First you'll need to download the drivers:

   Go to the [Cuda Downloads page](https://developer.nvidia.com/cuda-downloads) and copy the link for Ubuntu 14.04 "Network Installer" and wget that file:
   
   `wget http://developer.download.nvidia.com/compute/cuda/7_0/Prod/local_installers/rpmdeb/cuda-repo-ubuntu1404-7-0-local_7.0-28_amd64.deb`
3. Run: `sudo dpkg -i cuda-repo-ubuntu1404-7-0-local_7.0-28_amd64.deb`
4. Run: `sudo apt-get update && sudo apt-get install cuda`
5. Finally make symlinks to the `include` and `lib` directories:
  `sudo ln -s /usr/local/cuda-7.0/include/* /usr/local/include/`
  `sudo ln -s /usr/local/cuda-7.0/lib/* /usr/local/lib/`    

1. CUDA drivers require you to rebuild the linux kernel from source. Run:

    `sudo apt-get update`
    and
    
    `sudo apt-get install linux-image-extra-`\``uname -r`\`
    
    choose “keep the local version currently installed” when prompted about the grub menu list


The last step is we need to build OCLvanitygen:

1. Install git: `sudo apt-get install git`
2. Clone the repo: `git clone https://github.com/samr7/vanitygen.git`
3. Install dependencies: `sudo apt-get install libssl-dev libpcre3-dev`
4. And build: `cd vanitygen && make oclvanitygen`

Once you have oclgen built you can run

`./oclvanitygen 1Test`

And see it start to crunch away! Happy generating!

Thanks to all my sources!

[http://tleyden.github.io/blog/2014/10/25/cuda-6-dot-5-on-aws-gpu-instance-running-ubuntu-14-dot-04/](http://tleyden.github.io/blog/2014/10/25/cuda-6-dot-5-on-aws-gpu-instance-running-ubuntu-14-dot-04/)


[https://github.com/BVLC/caffe/issues/1092](https://github.com/BVLC/caffe/issues/1092)

[https://devtalk.nvidia.com/default/topic/547588/error-installing-nvidia-drivers-on-x86_64-amazon-ec2-gpu-cluster-t20-gpu-/](https://devtalk.nvidia.com/default/topic/547588/error-installing-nvidia-drivers-on-x86_64-amazon-ec2-gpu-cluster-t20-gpu-/)

[http://askubuntu.com/questions/451221/ubuntu-14-04-install-nvidia-driver](http://askubuntu.com/questions/451221/ubuntu-14-04-install-nvidia-driver)
