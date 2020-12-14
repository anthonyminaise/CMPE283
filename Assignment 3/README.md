# Assignment 3
This is the folder for Assignment 3, due December 13, 2020.\
By: Anthony Minaise (SID: 010246509), Haasitha Pidaparthi (SID: 012669254)

Your assignment is to modify the CPUID emulation code in KVM to report back additional information when special CPUID leaf nodes are requested.

 - Anthony Minaise - I worked on this with Haasitha Pidaparthi. For the most part, we pair programmed and researched the changes in cpuid.c and vmx.c. As for the starting files, we used Haasitha's files from assignment 2 as the base code and added onto it. We refered to the Intel SDM exit reasons list and compared that to the kvm exit reason list found in vmx.c. We both tried to run the changes on our own VMs to be safe.
- Haasitha Pidaparthi - I worked on this Assignment with Anthony. We both started off by individually doing our own reasearch for this assignment before collaborating. I went through the code from Assignment 2 and referred the SDM to figure out the appropriate exit reasons and KVM exit handllers needed to run the cpuid.c and vmx.c files. Additionally, I tested the code with a sample test file to make sure the program was running. 
- Note: we felt like the part where it checks the KVM enabled exits should have went in vmx.c, but we put it in the cpuid.c in the end since we were most comfortable with that location.

## Steps to Build Assignment 3
This assignment build is similar to Assignment 2

Make sure you have all the right packages and requiremnts in your outer VM for nested VM capabilities (assuming you have a Linux outer VM to start with in some VMM like VMware Workstation)

```
sudo apt-get install qemu-kvm libvirt-bin virtinst bridge-utils cpu-checker
```

### Build Kernel
* Clone the Linux repository using the following link: git clone https://github.com/torvalds/linux.git
```
haasi@haasi-vm:~$ git clone https://github.com/torvalds/linux.git
Cloning into 'linux'...
remote: Enumerating objects: 7749332, done.
remote: Total 7749332 (delta 0), reused 0 (delta 0), pack-reused 7749332
Receiving objects: 100% (7749332/7749332), 2.90 GiB | 5.67 MiB/s, done.
Resolving deltas: 100% (6431938/6431938), done.
Checking connectivity... done.
Checking out files: 100% (70695/70695), done.
```
* Follow the sequence of instruction
```
haasi@haasi-vm:~$ sudo bash
[sudo] password for haasi: 
root@haasi-vm:/home/haasi# apt-get install build-essential kernel-package fakeroot libncurses5-dev libssl-dev ccache bison flex libelf-dev
root@haasi-vm:/home/haasi# uname -a
Linux haasi-vm 5.4.0-52-112-generic #57-Ubuntu SMP Thu Oct 15 10:57:00 UTC 2020 x86_64 x86_64 x86_64 GNU/Linux
root@haasi-vm:/home/haasi# cp /boot/config-5.4.0-52-112-generic ./.config
root@haasi-vm:/home/haasi# cd linux/
root@haasi-vm:/home/haasi/linux# make oldconfig
root@haasi-vm:/home/haasi/linux# make && make modules && make install && make modules-install
root@haasi-vm:/home/haasi/linux# make -j
root@haasi-vm:/home/haasi/linux# reboot
```
Create your inner VM with a Linux image, I personally used Ubuntu (ubuntu-20.04.1-desktop-amd64.iso)
- This can be done either on the command line using `virt-install` or through the GUI Virtual Machine Manager given by Linux

### Changes to the Source Code 
For CPUID leaf node %eax=0x4FFFFFFE:
Return the number of exits for the exit number provided (on input) in %ecx
This value should be returned in %eax

For leaf nodes 0x4FFFFFFE, if %ecx (on input) contains a value not defined by the SDM, return 0 in all %eax, %ebx, %ecx registers and return 0xFFFFFFFF in %edx. For exit types not enabled in KVM, return 0s in all four registers.

Make sure to change the following files and modify the functionality for total exits and info based on the assignment requirements found in this repo.
- linux/arch/x86/kvm/cpuid.c
- linux/arch/x86/kvm/vmx/vmx.c    

1. In cpuid.c (linux/arch/x86/kvm), add the following changes to the end of the file
```
else if (eax == 0x4ffffffe) { // Assignment 3
    // Input not defined by the SDM
	if (ecx>=0 && ecx<=68 && ecx!=35 && ecx!=38 && ecx!=42 && ecx!=65) {
		// Exit types not enabled in KVM, then return all 4 registers as 0
		if (eax==3 || eax==4 || eax==5 || eax==6 || eax==11 || eax==16 || eax==17 || eax==33 || eax==34 || eax==51 || eax==63 || eax== 64 || eax==66 || eax== 67 || eax== 68) {
			printk(KERN_INFO "Exit type (%u) is not enabled in KVM.", ecx); 
			eax=0;
			ebx=0;
			ecx=0;
			edx=0;
		} else {
			// Return leaf exit counter
			eax = atomic64_read(&leaf_exit_counter[ecx]);
		    printk(KERN_INFO "Exit Number=%u; Exit Counter=%u", ecx, eax);
		}
	} else {
		printk(KERN_INFO "Exit type (%u) is not defined in SDM.", ecx); 
		eax=0;
		ebx=0;
		ecx=0;
		edx=0xFFFFFFFF;
	}
}
```
2. vmx.c (linux/arch/x86/kvm/vmx) - add leaf exit counter to vmx_handle_exit() function

### Run the code
* Start up the newly created inner VM
* Make a sample test program that calls the CPUID `x4FFFFFFE` on eax and loop through all the exit numbers on ecx to check the exit counts

## Questions
1. Comment on the frequency of exits – does the number of exits increase at a stable rate? Or are there more exits performed during certain VM operations? Approximately how many exits does a full VM boot entail?
    - The number of exits for each run seems to stay relatively the same. Certain VM operations such as I/O instructions produced more exits than other operations. Running the full VM approxiamtely had 150,000 exits.

2. Of the exit types defined in the SDM, which are the most frequent? Least?
    - The most frequent exit types were external interrupt, cpuid, and wrsmr. The least frequent exit types were rsm, invd, etc. 
