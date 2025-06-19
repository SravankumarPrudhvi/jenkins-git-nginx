if you face issue while trasfering from jenkins to ngin the error Host key verification failed.

The error again is:
Host key verification failed.
scp: Connection closed
Build step 'Execute shell' marked build as failure
This means Jenkins (running as the jenkins user) is trying to connect via SSH (scp) to the server at 52.66.251.159, but the SSH host key is not trusted yet by the Jenkins server.

avoid below process cause of security issue and especially in production servers it is okay for test servers

Option 1: Automatically Trust the Host (Quickest Fix)

Update your Jenkins job’s Shell build step to use this format:
scp -o StrictHostKeyChecking=no -o UserKnownHostsFile=/dev/null -r \
/var/lib/jenkins/workspace/testrun/assets \
/var/lib/jenkins/workspace/testrun/index.html \
/var/lib/jenkins/workspace/testrun/readme.txt \
root@52.66.251.159:/var/www/html
This will:

Disable SSH host key checking

Avoid interactive confirmation

Suppress warnings 
✅ Use this only for test environments. Avoid in production due to security risk.


Option 2: Manually Accept Host Key (Secure, Recommended)
Run the following manually on the Jenkins server, as the jenkins user:

bash
Copy
Edit
sudo su - jenkins
ssh root@52.66.251.159
You'll see:

vbnet
Copy
Edit
The authenticity of host '52.66.251.159' can't be established.
Are you sure you want to continue connecting (yes/no)?
Type yes and hit Enter.

That saves the SSH key in:

/var/lib/jenkins/.ssh/known_hosts
After that, your Jenkins job will succeed without the error.

🔹 Bonus: If Jenkins Says SSH Key is Missing
If you later get:

Permission denied (publickey)
Then you also need to:

Generate an SSH key for the jenkins user:

sudo su - jenkins
ssh-keygen -t ed25519
Copy the public key (~/.ssh/id_ed25519.pub) to the remote server in /root/.ssh/authorized_keys



-----------------------
# README
-----------------------
Browny is a one page bootstrap 3 based resume/portfolio template.


Template Info:
-----------------------
Name: 		Browny - Free Bootstrap One Page Portfolio Resume Tempalte
Version: 	1.0
Author: 	ThemeSINE
Website: 	https://www.themesine.com/


Changelog:
-----------------------
Version 1.0 14-05-2018
- initial release 


Credits:
-----------------------
- Twitter Bootstrap http://getbootstrap.com
- jQuery http://jquery.org
- Modernizr https://modernizr.com/
- Sticky.js http://stickyjs.com/
- JQuery easing https://github.com/gdsmith/jquery.easing
- Bootsnav http://bootsnav.danurstrap.com/
- Pexels https://www.pexels.com/
- Unsplash https://unsplash.com/

License:
-----------------------
This template is under Free License - https://www.themesine.com/license/
