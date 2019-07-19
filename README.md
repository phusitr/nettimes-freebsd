---Step1---
#####################################################
# nettimes-freebsd #
# Compile new kernel for support GPS/PPS signal
####################################################
[root@freebsd /usr/ports/net/ntp]# cd /usr/src/sys/i386/conf/
[root@freebsd /usr/src/sys/i386/conf]# cp GENERIC PPSGENERIC


---Step2---
Now open up PPSGENERIC in Vim (or whatever text editor you are using) and add the 
following line to enable PPS support.
# PPS Support
options         PPS_SYNC


---Step3---
[root@freebsd /usr/src/sys/i386/conf]# cd /usr/src/
[root@freebsd /usr/src]# make buildkernel KERNCONF=PPSGENERIC
...
some time later...
...
ld -Bshareable  -d -warn-common -o zlib.ko.debug zlib.kld
objcopy --only-keep-debug zlib.ko.debug zlib.ko.symbols
objcopy --strip-debug --add-gnu-debuglink=zlib.ko.symbols zlib.ko.debug zlib.ko
--------------------------------------------------------------
>>> Kernel build for PPSGENERIC completed on Mon Jan 25 18:41:50 EST 2010
--------------------------------------------------------------
[root@freebsd /usr/src]# make installkernel KERNCONF=PPSGENERIC


---Step5---
Open /etc/ttys and comment out the following lines:

# Serial terminals
# The 'dialup' keyword identifies dialin lines to login, fingerd etc.
# ttyu0   "/usr/libexec/getty std.9600"   dialup  off secure
# ttyu1   "/usr/libexec/getty std.9600"   dialup  off secure
# ttyu2   "/usr/libexec/getty std.9600"   dialup  off secure
# ttyu3   "/usr/libexec/getty std.9600"   dialup  off secure


---Step6---
We create this link in /etc/devfs.conf. Add the following to that file.
link  cuau0 gps1





