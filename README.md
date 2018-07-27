 # aws_lts_kvm_conf_on_kvm (working with "aws_lts"  as a virtual machine in KVM for Linux )

 Note: The instructions  below are applicalble for LINUX (I have tried on CENTOS 7.5)

Step1: Download the qcow2 image from aws linux.
wget -c https://cdn.amazonlinux.com/os-images/2.0.20180622.1/kvm/amzn2-kvm-2.0.20180622.1-x86_64.xfs.gpt.qcow2

Step2: Create and save file by name user-data

Step3: Create and save file by name meta-data
 
Step4: For creating startup iso, you will need the following command. Please make sure you have user-data and meta-data file in same folder to generate the iso.

For Linux, use a tool such as genisoimage. Navigate into the seedconfig folder and execute the following command: 
$ genisoimage -output seed.iso -volid cidata -joliet -rock user-data meta-data

For macOS, use a tool such as hdiutil. Navigate one level up from the seedconfig folder and execute the following command: 
$ hdiutil makehybrid -o seed.iso -hfs -joliet -iso -default-volume-name cidata seedconfig/

Step5: In KVM you must configure ypur CD-ROM as boot priority 1, and then IDE disk1 as 2.

Step6. Finally,  start your virtual machine and when prompted for username please enter: ec2-user and Password as: password
for root : sudo su -

Hope this helps.

References:
https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/amazon-linux-2-virtual-machine.html
