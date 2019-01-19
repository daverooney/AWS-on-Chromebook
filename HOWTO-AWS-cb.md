## Chromebook prep

Secure Shell Extension [here](https://chrome.google.com/webstore/detail/secure-shell-extension/iodihamcpbpeioajjeobimgagajmlibd?utm_source=chrome-ntp-icon)


## SSH prep

Ec2 console > NEtwork & Security > Key Pairs

Create Key pair
  - named "CB-SSH" in this example, substitute your own name
  - auto downloads CB-SSH.pem
  - this contains both private and public keys, but we need to do ninja stuff to make them usable
  
If you *do not* have an existing VPC you want to use:
- EC2 console > NEtwork & Security > Elastic IPs
- Allocate new address
- scope: VPC, AWS Pool
- done

# Option 1 - AWS Cloud9

Cloud9, create New Environment

- Name "CB-SSH", skip desc
- Create new instance for env, t2.micro is fine
- 30 minutes inactivity is fine

If you have existing VPC, go ahead.

If not, Create New VPC
- VPC with Public and Private Subnets
- default CIDR block ok
- name SSH-VPC
- defaults ok
- for Elastic Allocation ID, pick the EIP we generated above




# Option 2 - PythonAnywhere

Create free beginner account [here](https://www.pythonanywhere.com/registration/register/beginner/)

Log in, click on Files section

Click Upload a File, select CB-SSH.pem

Near top, click Open Bash console here

lock down permissions on the key:  chmod 400 CB-SSH.pem

use ssh-keygen to export public key only:
- *ssh-keygen -y -f CB-SSH.pem > CB-SSH.pub
- the "-y" means export public key
- the "-f" specifies the input key file name
- the > dumps the results in to the filename provided

Clean up the naming on the private key
- mv CB-SSH.pem CB-SSH
- no extension - some ssh programs are super picky about this

Go back to Files interface
- Find CB-SSH and CB-SSH.pub
- right-click on DL icon, chose "Save Link As..." for both
- 



