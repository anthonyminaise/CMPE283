# Assignment 4
This is the folder for Assignment 4, due December 13, 2020.\
By: Anthony Minaise, SID: 010246509 
/ Haasitha Pidaparthi, SID: 012669254 

The purpose of this assignment is to illustrate the difference in performance when using nested paging versus shadow paging, and to illustrate the different exit frequencies and types.

## Instructions to run with shadow paging
* Remove the ‘kvm-intel’ module from the kernel.
* Reload the kvm-intel module using ept=0 to disable nested paging and force shadow paging.
```
haasi@haasi-vm:~$ sudo rmmod kvm-intel
[sudo] password for haasi: 
haasi@haasi-vm:~$ sudo insmod arch/x86/kvm/kvm-intel.ko ept=0
```

## Questions
1. For each member in your team, provide 1 paragraph detailing what parts of the lab that member implemented / researched. 
    - Haasitha Pidaparthi - I went over the lecture video on nested and shadow paging. Compared the exit counts for both nested and shadow paging.
2. Include a sample of your print of exit count output from dmesg from “with ept” and “without ept”.
   - With EPT
   ```
   haasi@haasi-vm:~$ ./text3
   CPUID(0x4FFFFFFE), exit number=1, number of exits=235728
   ```
   - Without EPT
   ```
   haasi@ubuntu:~$ ./text3
   CPUID(0x4FFFFFFE), exit number=1, number of exits=423724
   ```
3. What did you learn from the count of exits? Was the count what you expected? If not, why not?

4. What changed between the two runs (ept vs no-ept)?
