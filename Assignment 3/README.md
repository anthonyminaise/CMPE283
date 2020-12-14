# Assignment 3
This is the folder for Assignment 3, due December 13, 2020.\
By: Anthony Minaise, SID: 010246509 
/ Haasitha Pidaparthi, SID: 

## Questions
1. For each member in your team, provide 1 paragraph detailing what parts of the lab that member implemented / researched. (You may skip this question if you are doing the lab by yourself).
    - I worked on this with Haasitha Pidaparthi. For the most part, we pair programmed and researched the changes in cpuid.c and vmx.c. As for the starting files, we used Hassitha's files from assignment 2 as the base code and added onto it. We refered to the Intel SDM exit reasons list and compared that to the kvm exit reason list found in vmx.c. We both tried to run the changes on our own VMs to be safe.
2. Describe in detail the steps you used to complete the assignment. Consider your reader to be someone skilled in software development but otherwise unfamiliar with the assignment. Good answers to this question will be recipes that someone can follow to reproduce your development steps.
    - Make sure you have all the right packages and requiremnts in your outer VM for nested VM capabilities (assuming you have a Linux outer VM to start with in some VMM like VMware Workstation)
    ```
    sudo apt-get install qemu-kvm libvirt-bin virtinst bridge-utils cpu-checker
    ```
    - Clone https://github.com/torvalds/linux and make sure to change the following files and modify the functionality for total exits and info based on the assignment requirements found in this repo.
        - linux/arch/x86/kvm/cpuid.c
        - linux/arch/x86/kvm/vmx/vmx.c    
    - Run the following (partly from Assignment 2)
    ```
    sudo bash
    git clone https://github.com/anthonyminaise/linux.git
    cd linux
    apt-get install build-essential kernel-package fakeroot libncurses5-dev libssl-dev ccache bison flex libelf-devuname -a       (and note down your kernel version, for example “4.15.0-112-generic”)
    cp /boot/config-4.15.0-112-generic    ./.config       (substitute your version obtained from the previous step here though)
    make oldconfig       (and then just use the default for everything, don’t change anything – you can do this by holding down enter)
    make && make modules && make install && make modules-install     (will take a long time the first time)
    make -j
    reboot
    uname -a (to verify using newer kernel)
    ```
    - Create your inner VM with a Linux image, I personally used Ubuntu (ubuntu-20.04.1-desktop-amd64.iso)
        - This can be done either on the command line using `virt-install` or through the GUI Virtual Machine Manager given by Linux
    - Start up the newly created inner VM
    - From there, make a program call the CPUID `x4FFFFFFE` and keep track of the exits
3. Comment on the frequency of exits – does the number of exits increase at a stable rate? Or are there more exits performed during certain VM operations? Approximately how many exits does a full VM boot entail?
    - Doesn't seem like the number of exits increase at a stable rate?
    - ______ exits
4. Of the exit types defined in the SDM, which are the most frequent? Least?
    - [answer]
