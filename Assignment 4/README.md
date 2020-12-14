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
    - Anthony Minaise - Went over the lectures and notes on nested and shadow paging. Analyzed the exit counts and what was different between the two types of paging.
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
    - We expected shadow paging to have more exits than nested paging. Shadow paging may have have more exits, but it can give better performance when the guest page table is static which makes the cost of more VM exits to be less than the total cost of nested transactions.
4. What changed between the two runs (ept vs no-ept)?
    - No-ept on has more exits for memory access versus ept doesn't care about the instructions no-ept since there because there is a safety net on the second translation for each access to memory. 
