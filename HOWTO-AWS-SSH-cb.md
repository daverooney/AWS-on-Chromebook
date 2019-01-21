## Chromebook prep

Install the Secure Shell App [here](https://chrome.google.com/webstore/detail/secure-shell-app/pnhechapfaindjhompbnflcldabbghjo)


## AWS prep

Ec2 console > NEtwork & Security > Key Pairs

Create Key pair
  - named "CB-SSH" in this example, substitute your own name
  - auto downloads CB-SSH.pem
  - this contains both private and public keys, but we need to do ninja stuff to make them usable

## Key prep

### Option 1 - AWS Cloud9

Cloud9, create New Environment

- Name "CB-SSH", skip desc
- Create new instance for env, t2.micro is fine
- 30 minutes inactivity is fine

If you have existing VPC, go ahead.

If not, Create New VPC.  ([reference here](https://docs.aws.amazon.com/cloud9/latest/user-guide/vpc-settings.html#vpc-settings-create-vpc))
- VPC Wizard
- VPC with Single Public Subnet
- default CIDR block ok
- name SSH-VPC
- defaults ok
- back in c9 config, refresh VPC list and select the new one
- "Next Step"
- review and then "Create Environment"
- wait a few minutes for creation

Once connected, open File menu and click Upload Local Files...

Click "Select files" and pick the .pem file. Close the upload interface.

lock down permissions on the key:  chmod 600 CB-SSH.pem

use ssh-keygen to export public key only:
- *ssh-keygen -y -f CB-SSH.pem > CB-SSH.pub*
- the "-y" means export public key
- the "-f" specifies the input key file name
- the > dumps the results in to the filename provided
- hat tip to Matt Burns [here](http://www.mattburns.co.uk/blog/2012/11/15/connecting-to-ec2-from-chromes-secure-shell-using-only-a-pem-file/)

Clean up the naming on the private key
- cp CB-SSH.pem CB-SSH
- no extension - some ssh programs are super picky about this
- in the left-hand pane, right-click (two-finger click) the CB-SSH.pub and CB-SSH files and click Download
- back in the command line shell, type "rm CB*" and hit enter to delete the keys from the C9 server


### Option 2 - PythonAnywhere

Create free beginner account [here](https://www.pythonanywhere.com/registration/register/beginner/)

Log in, click on Files section

Click Upload a File, select CB-SSH.pem

Near top, click Open Bash console here

lock down permissions on the key:  chmod 600 CB-SSH.pem

use ssh-keygen to export public key only:
- *ssh-keygen -y -f CB-SSH.pem > CB-SSH.pub*
- the "-y" means export public key
- the "-f" specifies the input key file name
- the > dumps the results in to the filename provided
- hat tip to Matt Burns [here](http://www.mattburns.co.uk/blog/2012/11/15/connecting-to-ec2-from-chromes-secure-shell-using-only-a-pem-file/)

Clean up the naming on the private key
- cp CB-SSH.pem CB-SSH
- no extension - some ssh programs are super picky about this

Go back to Files interface
- Find CB-SSH and CB-SSH.pub
- right-click on DL icon, chose "Save Link As..." for both
- click trash icon for all 3 files to clear keys from PA server


## Launch instance
  
public IP, in public subnet
  
SG that allows tcp22 from 0.0.0.0
  
Choose existing keypair CB-SSH

Wait for launch to finish

## Time to connect

select instance in Ec2 console and copy Public DNS value

click Connect button to show username and public DNS for chosen instance

launch ssh console
- type in username and paste in public DNS name
- near Identity, click Import...
- *Super Secret secret of Success*: hold down Ctrl while selecting *both* CB-SSH and CB-SSH.pub
- some blogs say you can use .pem instead of the extension-less private key. I think they're enemy agents.
- if you see CB-SSH in the Identity field, you win
- for extra credit, put extra SSH Arguments in to do fancy things like tunnelling
- press Enter button
- Win!  Happy Hacking!


## cleanup

- if PythonAnywhere, just log out
- if Cloud9, close tab to get back to environment listing.
  - select your CB-SSH environment, click Delete
  - type "Delete" to confirm
- remove SSH-VPC if created and no longer wanted
- feel free to shutdown or terminate your SSH test target EC2 instance


