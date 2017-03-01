# Push to github without typing user name / password

Enviroment: Ubuntu 16.04

Solution : using ssh.  Steps：
* Create your ssh keys 

              $ cd ~/.ssh
              $ ssh-keygen -t rsa -C "your_mail_adress_used_github@xxx.com"

The public key in file named id_rsa.pub and the private key in file named id_rsa

* Add the public key into your github account: copy the content of the file id_rsa.pub, open your github page > account > settings > SSH and GPG keys > New SSH keys, then paste.

![account]({{ site.url }}/images/github_account_setting.png){:height="480px"}

![setting]({{ site.url }}/images/github_account_setting_SSH.png){:height="480px"}

* Test on your computer

             $ ssh -T git@github.com 

* return to your repository on your github page, make the repository use ssh url: 
![repository_url_http_ssh] ({{ site.url }}/images/github_repository_use_ssh.png) {:height="480px"}
Click on "use ssh", then copy the url in ssh formate, paste into your local repository's git configuration file on your computer

* open your local_repository/.git/config,  
   
    [remote "origin"] 	   
       url = git@github.com:zzpanqing/zzpanqing.github.io.git // url copied from github
 



