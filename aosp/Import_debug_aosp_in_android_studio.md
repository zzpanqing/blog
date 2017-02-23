Import and debug aosp code into android studio
=================================

#Enviroment

* Ubuntu 16.04
* Laptop ASUS UX310UQK
    * 16Go RAM
    * 800Go Hard Disk

#Download and build#

[Official document] (https://source.android.com/source/requirements.html)

[Reference](http://blog.csdn.net/dd864140130/article/details/51718187)

###Download###
[Installing Repo] (https://source.android.com/source/downloading.html#installing-repo)

1.Make sure you have a bin/ directory in your home directory
    
    $ mkdir ~/bin
    $ PATH=~/bin:$PATH
    
2.Download the Repo tool and ensure that it is executable:
    
    $ curl https://storage.googleapis.com/git-repo-downloads/repo > ~/bin/repo
    $ chmod a+x ~/bin/repo
You may need to see the hiden folder .repo, use this command
    
    $ ls -ld .?*

3.Create a folder to hold the file to be downloaded
   
    $ mkdir WORKING_DIRECTORY
    $ cd WORKING_DIRECTORY
4.In WORKING_DIRECTORY, initialize repo with the branch that you want to checked out
    
    $ repo init -u https://android.googlesource.com/platform/manifest -b android-7.1.1_r14
-b is used to specify the name of the branch, in the line above, I specified the branch which code support Nexus 5X.
The full list of branches can be found [here](https://source.android.com/source/build-numbers.html#source-code-tags-and-builds)

5.Downloading the Android Source Tree

    $ repo sync -j8
-j is used to specify the number of threads used during the download.

###Build###
To build the lastest aosp code under Ubuntu 16.04, we need openjdk 8:
    
    $ sudo apt-get install openjdk-8-jdk
Install other packages 


    $ sudo apt-get install git-core gnupg flex bison gperf build-essential   zip curl zlib1g-dev gcc-multilib g++-multilib libc6-dev-i386   lib32ncurses5-dev x11proto-core-dev libx11-dev lib32z-dev ccache   libgl1-mesa-dev libxml2-utils xsltproc unzip
Configuring USB Access
    
    $ wget -S -O - http://source.android.com/source/51-android.rules | sed "s/<username>/$USER/" | sudo tee >/dev/null /etc/udev/rules.d/51-android.rules; sudo udevadm control --reload-rules
Set up environment
    
    $ source build/envsetup.sh

Choose a target

    $ lunch aosp_arm64-eng
    
The target name is formated as BUILD-BUILDTYPE, in the example above, BUILD is aosp-arm64, BUILDTYPE is eng. BUILD specify the  environment in which runs the builded image. 
* arm means the image will run on the ARM processor.
* arm64 means the image will run on the ARM-64 processor.
BUILDTYPE is specified as -eng, means engineer, which is for developer, you have the root permission

Build with 8 threads
    
    $ make -j8
The number of threads depends on the CPU core number, my laptop has 2 core, and 2 threads per core, so -j8 is good for me.
    
I waited 2h30 to get the build success message:

    ### make completed successfully (02:36:45(hh:mm:ss)) ###

Then we can run emulator to be sur that the build is done.

    $ emulator
    
To run emulator later we need type:
    
    $ source build/envsetup.sh
    $ lunch // choose the one you did before make
    $ emulator
    
# Import and debug#
### Prepare ###

The tool idengen provided in AOSP is used to create the android studio / Intellij project file of aosp 

    $ soruce build/envsetup.sh
    $ mmm development/tools/idegen/
    $ sudo ./development/tools/idegen/idegen.sh
    
After the 3 commands, we get 3 files:
* android.ipr : project file,  can be opened in Android Studio.
* android.iml : description of modules
* android.iws : workspace settings

### Import ###
Android Studio -> File -> Open -> android.ipr

To make the sur that Android Studio look only in the code of AOSP, we should set the JDK and SDK.

* Add JDK 1.8(No Library) : Android Studio -> File -> Project Structure



An h1 header
============

Paragraphs are separated by a blank line.

2nd paragraph. *Italic*, **bold**, and `monospace`. Itemized lists
look like:

  * this one
  * that one
  * the other one

Note that --- not considering the asterisk --- the actual text
content starts at 4-columns in.

> Block quotes are
> written like so.
>
> They can span multiple paragraphs,
> if you like.

Use 3 dashes for an em-dash. Use 2 dashes for ranges (ex., "it's all
in chapters 12--14"). Three dots ... will be converted to an ellipsis.
Unicode is supported. ☺



An h2 header
------------

Here's a numbered list:

 1. first item
 2. second item
 3. third item

Note again how the actual text starts at 4 columns in (4 characters
from the left side). Here's a code sample:

    # Let me re-iterate ...
    for i in 1 .. 10 { do-something(i) }

As you probably guessed, indented 4 spaces. By the way, instead of
indenting the block, you can use delimited blocks, if you like:

~~~
define foobar() {
    print "Welcome to flavor country!";
}
~~~

(which makes copying & pasting easier). You can optionally mark the
delimited block for Pandoc to syntax highlight it:

~~~python
import time
# Quick, count to ten!
for i in range(10):
    # (but not *too* quick)
    time.sleep(0.5)
    print i
~~~



### An h3 header ###

Now a nested list:

 1. First, get these ingredients:

      * carrots
      * celery
      * lentils

 2. Boil some water.

 3. Dump everything in the pot and follow
    this algorithm:

        find wooden spoon
        uncover pot
        stir
        cover pot
        balance wooden spoon precariously on pot handle
        wait 10 minutes
        goto first step (or shut off burner when done)

    Do not bump wooden spoon or it will fall.

Notice again how text always lines up on 4-space indents (including
that last line which continues item 3 above).

Here's a link to [a website](http://foo.bar), to a [local
doc](local-doc.html), and to a [section heading in the current
doc](#an-h2-header). Here's a footnote [^1].

[^1]: Footnote text goes here.

Tables can look like this:

size  material      color
----  ------------  ------------
9     leather       brown
10    hemp canvas   natural
11    glass         transparent

Table: Shoes, their sizes, and what they're made of

(The above is the caption for the table.) Pandoc also supports
multi-line tables:

--------  -----------------------
keyword   text
--------  -----------------------
red       Sunsets, apples, and
          other red or reddish
          things.

green     Leaves, grass, frogs
          and other things it's
          not easy being.
--------  -----------------------

A horizontal rule follows.

***

Here's a definition list:

apples
  : Good for making applesauce.
oranges
  : Citrus!
tomatoes
  : There's no "e" in tomatoe.

Again, text is indented 4 spaces. (Put a blank line between each
term/definition pair to spread things out more.)

Here's a "line block":

| Line one
|   Line too
| Line tree

and images can be specified like so:

![example image](example-image.jpg "An exemplary image")

Inline math equations go in like so: $\omega = d\phi / dt$. Display
math should get its own line and be put in in double-dollarsigns:

$$I = \int \rho R^{2} dV$$

And note that you can backslash-escape any punctuation characters
which you wish to be displayed literally, ex.: \`foo\`, \*bar\*, etc.



