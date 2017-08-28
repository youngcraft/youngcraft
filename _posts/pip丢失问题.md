
33
down vote
accepted
I had the same message on linux.

/usr/bin/pip: No such file or directory
but then checked which pip was being called.

$ which pip
/usr/local/bin/pip 
On my debian wheezy machine I fixed it doing following...

/usr/local/bin/pip uninstall pip  
apt-get remove python-pip  
apt-get install python-pip  
====================================
This was due to mixup installing with apt-get and updating with pip install -U pip.

These also installed libraries at 2 different places which caused problems for me.

/usr/lib/python2.7/dist-packages  
/usr/local/lib/python2.7/dist-packages