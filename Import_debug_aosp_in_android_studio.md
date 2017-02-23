Import and debug aosp code into android studio
=================================

#Download and build#

[Official document] (https://source.android.com/source/requirements.html)

[Reference](http://blog.csdn.net/dd864140130/article/details/51718187)

###Download###

###Build###
To build the lastest aosp code, we need openjdk 8:
    
    $ sudo apt-get install openjdk-8-jdk
Install other packages 

    $ sudo apt-get install git-core gnupg flex bison gperf build-essential   zip curl zlib1g-dev gcc-multilib g++-multilib libc6-dev-i386   lib32ncurses5-dev x11proto-core-dev libx11-dev lib32z-dev ccache   libgl1-mesa-dev libxml2-utils xsltproc unzip
Configuring USB Access
    
    $ wget -S -O - http://source.android.com/source/51-android.rules | sed "s/<username>/$USER/" | sudo tee >/dev/null /etc/udev/rules.d/51-android.rules; sudo udevadm control --reload-rules



