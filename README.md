# aws_lts_kvm_conf_on_kvm (working with "aws_lts"  as a virtual machine in KVM for Linux )
# Note: The instructions  below are applicalble for LINUX (I have tried on CENTOS 7.5)

Step1: Download the qcow2 image from aws linux.
wget -c https://cdn.amazonlinux.com/os-images/2.0.20180622.1/kvm/amzn2-kvm-2.0.20180622.1-x86_64.xfs.gpt.qcow2

#Step2: create and save file by name user-data

#user-data

#cloud-config
# vim:syntax=yaml
users:
# A user by the name ec2-user is created in the image by default.
  - default
# Following entry create user1 and assigns password specified in plain text.
# Please not use of plain text password is not recommended from security best
# practises standpoint
  - name: user1
    groups: sudo
    sudo: ['ALL=(ALL) NOPASSWD:ALL']
    plain_text_passwd: <password>
    lock_passwd: false
# Following entry creates user2 and attaches a hashed passwd to the user. Hashed
# passwords can be generated with:
# python -c 'import crypt,getpass; print crypt.crypt(getpass.getpass())'
#  - name: user2
#    passwd: < hashed password here >
#    lock_passwd: false
# Following entry creates user3, disables password based login and enables an SSH public key
#  - name: user3
#    ssh-authorized-keys:
#            - < ssh public key here >
#    lock_passwd: true

chpasswd:
  list: |
    ec2-user:password

Step3: 
# create and save file by name user-data
 #meta-data
 local-hostname: lotus.local
 
Step4: 
# For creating startup iso, you will need the following command.

For Linux, use a tool such as genisoimage. Navigate into the seedconfig folder and execute the following command: 
$ genisoimage -output seed.iso -volid cidata -joliet -rock user-data meta-data

For macOS, use a tool such as hdiutil. Navigate one level up from the seedconfig folder and execute the following command: 
$ hdiutil makehybrid -o seed.iso -hfs -joliet -iso -default-volume-name cidata seedconfig/

Step5: 
# In KVM you must configure ypur CD-ROM as boot priority 1, and then IDE disk1 as 2.

Step6. 
# Finally,  start your virtual machine and when prompted for username please enter: ec2-user and Password as: password
for root : sudu su -

