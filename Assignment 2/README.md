# Assignment 2
This is the folder for Assignment 2, due November 3, 2020.\
By: Anthony Minaise, SID: 010246509

## Questions
1. For each member in your team, provide 1 paragraph detailing what parts of the lab that member implemented / researched. (You may skip this question if you are doing the lab by yourself).
    - I did this assignment myself.
2. Describe in detail the steps you used to complete the assignment. Consider your reader to be someone skilled in software development but otherwise unfamiliar with the assignment. Good answers to this question will be recipes that someone can follow to reproduce your development steps.
    - Started by cloning the linux repo from torvalds to my Ubuntu VM
    - After the clone finished, I initially ran the "Building the Kernal" terminal lines to get things started and see how running the kernal works
        - Learned the hard way that the first time running it takes a few hours
    - Identify where the cpuid.c and vmx.c file is
        - cpuid.c can be found in linux/arch/x86/kvm 
        - vmx.c can be found in linux/arch/x86/kvm/vmx
    - Started with looking at cpuid.c to make changes there to add the case on if eax = 0x4FFFFFFF
        - Found that the very last method holds the emulation code, so I added an if else statement to handle the case, do the proper assignment to the registers, and output to the console.
        - Listened to the hint in the assignment and looked up the atomic variables to use
    - Looked at vmx.c to see if there are any necessary changes needed to accommodate the changes in cpuid.c
        - Had to guesstimate some initial changes, like using the atomic variables similar to cpuid.c, at the very least
    - Assuming all goes well, running the test program in the nested VM should output something when eax = 0x4FFFFFFF
3. Comment on the frequency of exits – does the number of exits increase at a stable rate? Or are there more exits performed during certain VM operations? Approximately how many exits does a full VM boot entail?
    - I couldn't test out my changes in an nested VM since building the kernel took too long each time I tried. My computer holding my hypervisor and VMs was constantly spinning at 100% memory and spiked to 100% CPU all the time, so I might have given too many resources to my VM and that could be the reason why it took so long.
        - It ended up taking around 7 hours to build the kernel the first time around.
    - My initial guess would be that the number of exits would not increase at a stable rate because operations that are triggered after a VM exits.
    - As of now, I do not have an approximation for the number of exits.