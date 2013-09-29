Kstat-kernal-module
===================
The KSTAT module intercepts the outgoing packets.
The netfilter mechanism in Linux provides a convenient way to
intercept packets.Packets are intercepted at 5 hooks: PRE_ROUTING,
POST_ROUTING, LOCAL_IN, LOCAL_OUT and FORWARD. 
Using netfilter,
first registering a hook function when the module is initialized. Then,
whenever a packet traverse to the hook you registered,hook function will
be called to process the packet.The hook function will update/
create the STAT Structure. The packet is represented in a sk_buff structure,
which is a very complex and huge structure.Returns NF_ACCEPT to let the packet
continue traversal as normal or NF_DROP to drop the packet.unregisters
the hook when exiting the module.
Report the outgoing packets as a proc file. The data is stored in the proc directory “/proc/kstat/nout”. Please refer to [6,7,9] for an
example of using proc file system.
A proc entry is maintained by the name “counter”(“/proc/kstat/counter”), which
determines after how many packets a proc entry will be updated. For example
if counter=10, then you will update proc entry after 10 packets have arrived at
the netfilter. Note that this entry can be
changed while the module is running and it impacts the granularity at which proc
file entry is updated.
Implemented  sysfs to start stop the STAT service.Loading the module is different from starting the STAT
service. When you load the module, the module is dynamically linked to the
kernel. However, STAT service is not started until a sysfs parameters is set
appropriately.

References:
1. The Linux Kernel Module Programming Guide http://www.linuxtopia.org/online_books/
linux_kernel/linux_kernel_module_programming_2.6/x121.html
2. Linux netfilter Hacking HOWTO http://www.netfilter.org/
documentation/HOWTO//netfilter-hacking-HOWTO.html & http://
www.netfilter.org/documentation/HOWTO/netfilter-hacking-HOWTO-3.html
3. How SKBs work http://vger.kernel.org/~davem/skb.html[
4. skb - Linux network buffers http://ftp.gnumonks.org/pub/doc/skb-doc.html
5. skbuff.net http://www.skbuff.net/skbuff.html
6. Access the Linux kernel using the /proc filesystem, available at http://
www.ibm.com/developerworks/linux/library/l-proc.html
7. http://www.linux.com/learn/linux-training/37985-the-kernel-newbie-corner-kerneldebugging-
using-proc-qsequenceq-files-part-1
8. Linux Kernel Module Programming Guide – Passing Command
Line Arguments to a Module, available at http://www.linuxtopia.org/
online_books/Linux_Kernel_Module_Programming_Guide/x323.html
& http://www.makelinux.net/books/lkd2/ch16lev1sec6
9. Procfs Tutorial: http://www.stillhq.com/pdfdb/000445/data.pdf
