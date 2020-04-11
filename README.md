# docker-AWS-EFS

Tasks : Run docker container in EC2-Instance or on prem USE EFS as storage layer and persistent volume for Docker Container USE S3 as storage layer and persistent volume for Docker Container

Prerequsites: Access to AWS Account Docker running in AWS EC2 AWS EFS Configured AWS S3 bucket

Solution:

S3 as storage layer and persistent volume for Docker Container
Step 1 Installation

Amazon Linux via EPEL:
sudo amazon-linux-extras install epel sudo yum install s3fs-fuse

RHEL and CentOS 7 or newer through via EPEL: sudo yum install epel-release sudo yum install s3fs-fuse

Step 2 Configuration

Create s3fs user in aws iam

configure access :  /etc/passwd-s3fs    (or  ${HOME}/.passwd-s3fs)
ACCESS_KEY_ID:SECRET_ACCESS_KEY

chmod   600 /etc/passwd-s3fs
Step 3 Mount

s3fs mybucket /path/to/mountpoint   (-o passwd_file=${HOME}/.passwd-s3fs) (-o allow_other for on prem)   #without -o it will use /etc/passwd-s3fs  file
Step 4 Run docker container

docker run -dti (--restart unless-stopped) --name=web1 -v /s3/:/usr/local/apache2/htdocs/ -p 8080:80 httpd:2.4 (or other image)
######################################################################################

USE EFS as storage layer and persistent volume for Docker Container
Step 1 Installation:

Amazon Linux EC2 instance install EFS helper :
    sudo yum install -y amazon-efs-utils

or  install the NFS client
    sudo yum install -y nfs-utils
Step 2 Mount

sudo mount -t efs (EFS_FileSystem_ID):/  /mnt/nfs
Step 3 Run docker container

docker run -dti (--restart unless-stopped) --name=web2 -v /mnt/nfs/:/usr/local/apache2/htdocs/ -p 8081:80 httpd:2.4 (or other image)
