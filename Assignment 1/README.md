# Assignment 1
This is the folder for Assignment 1 (Intel), due October 25, 2020.\
By: Anthony Minaise, SID: 010246509

## Questions
1. For each member in your team, provide 1 paragraph detailing what parts of the lab that member implemented / researched. (You may skip this question if you are doing the lab by yourself).
    - I did this assignment myself.
2. Describe in detail the steps you used to complete the assignment. Consider your reader to be someone skilled in software development but otherwise unfamiliar with the assignment. Good answers to this question will be recipes that someone can follow to reproduce your development steps.
    - The first step I took was to check Canvas to see the assignment, but then I saw a video uploaded about how to do the assignment.
    - I followed what was being said in the video about downloading VMware Workstation 16 and used a license from the CMPE department. I made a VM of Ubuntu 20.04.1 that uses most of my memory and CPU and set the storage to 175GB.
    - Once the VM was ready, I installed git, gcc, and other libraries to make sure I can compile the makefile
        - I also made sure to get virtual machine manager at the same time and get things set up for Assignment 2.
    - From there, I started tinkering with the C code given. Started by defining the MSRs at the top with their bits.
    - Based on what was said on the video, it seemed like all I had to do was create 4 more global variables similar to pinbased[5] for the other 4 MSRs. I got the Intel SDM open and started copying and pasting the bits and names of each entry into new global variables using the capability_info as the instance type.
    - After that, the video indicated that the detect_vmx_features(void) did the actual work of printing out the controls for each MSR. I saw that pinbased was given, so I thought I would copy what that chunk of code was doing and duplicate it 4 more times for the other MSRs and use the global variables just created.
    - From there, it was a matter of running the actual Makefile and making sure everything prints smoothly.

## Sample Output:
<pre><code>
[   98.575360] CMPE 283 Assignment 1 Module Start
[   98.575361] Pinbased Controls MSR: 0x3f00000016
[   98.575362]   External Interrupt Exiting: Can set=Yes, Can clear=Yes
[   98.575363]   NMI Exiting: Can set=Yes, Can clear=Yes
[   98.575363]   Virtual NMIs: Can set=Yes, Can clear=Yes
[   98.575363]   Activate VMX Preemption Timer: Can set=No, Can clear=Yes
[   98.575364]   Process Posted Interrupts: Can set=No, Can clear=Yes
[   98.575365] Entry Controls MSR: 0xf3ff000011ff
[   98.575365]   Load debug controls: Can set=Yes, Can clear=No
[   98.575365]   IA-32e mode guest: Can set=Yes, Can clear=Yes
[   98.575366]   Entry to SMM: Can set=No, Can clear=Yes
[   98.575366]   Deactivate dual-monitor treatment: Can set=No, Can clear=Yes
[   98.575367]   Load IA32_PERF_GLOBAL_CTRL: Can set=Yes, Can clear=Yes
[   98.575367]   Load IA32_PAT: Can set=Yes, Can clear=Yes
[   98.575367]   Load IA32_EFER: Can set=Yes, Can clear=Yes
[   98.575368]   Load IA32_BNDCFGS: Can set=No, Can clear=Yes
[   98.575368]   Conceal VM entries from Intel PT: Can set=No, Can clear=Yes
[   98.575368]   Load IA32_RTIT_CTL: Can set=No, Can clear=Yes
[   98.575369]   Load CET state: Can set=No, Can clear=Yes
[   98.575369]   Load PKRS: Can set=No, Can clear=Yes
[   98.575370] Exit Controls MSR: 0x3fffff00036dff
[   98.575370]   Save debug controls: Can set=Yes, Can clear=No
[   98.575371]   Host address-space size: Can set=Yes, Can clear=Yes
[   98.575371]   Load IA32_PERF_GLOBAL_CTRL: Can set=Yes, Can clear=Yes
[   98.575371]   Acknowledge interrupt on exit: Can set=Yes, Can clear=Yes
[   98.575372]   Save IA32_PAT: Can set=Yes, Can clear=Yes
[   98.575372]   Load IA32_PAT: Can set=Yes, Can clear=Yes
[   98.575372]   Save IA32_EFER: Can set=Yes, Can clear=Yes
[   98.575373]   Load IA32_EFER: Can set=Yes, Can clear=Yes
[   98.575373]   Save VMX-preemption timer value: Can set=No, Can clear=Yes
[   98.575373]   Clear IA32_BNDCFGS: Can set=No, Can clear=Yes
[   98.575374]   Conceal VMX from PT: Can set=No, Can clear=Yes
[   98.575374]   Clear IA32_RTIT_CTL: Can set=No, Can clear=Yes
[   98.575374]   Load CET state: Can set=No, Can clear=Yes
[   98.575375]   Load PKRS: Can set=No, Can clear=Yes
[   98.575375] Primary Controls MSR: 0xfff9fffe0401e172
[   98.575376]   Interrupt-window exiting: Can set=Yes, Can clear=Yes
[   98.575376]   Use TSC offsetting: Can set=Yes, Can clear=Yes
[   98.575376]   HLT exiting: Can set=Yes, Can clear=Yes
[   98.575377]   INVLPG exiting: Can set=Yes, Can clear=Yes
[   98.575377]   Save debug controls: Can set=Yes, Can clear=Yes
[   98.575378]   RDPMC exiting: Can set=Yes, Can clear=Yes
[   98.575378]   RDTSC exiting: Can set=Yes, Can clear=Yes
[   98.575378]   CR3-load exiting: Can set=Yes, Can clear=No
[   98.575379]   CR3-store exiting: Can set=Yes, Can clear=No
[   98.575379]   CR8-load exiting: Can set=Yes, Can clear=Yes
[   98.575379]   CR8-store exiting: Can set=Yes, Can clear=Yes
[   98.575380]   Use TPR shadow: Can set=Yes, Can clear=Yes
[   98.575380]   NMI-window exiting: Can set=Yes, Can clear=Yes
[   98.575380]   MOV-DR exiting: Can set=Yes, Can clear=Yes
[   98.575381]   Unconditional I/O exiting: Can set=Yes, Can clear=Yes
[   98.575381]   Use I/O bitmaps: Can set=Yes, Can clear=Yes
[   98.575381]   Monitor trap flag: Can set=Yes, Can clear=Yes
[   98.575382]   Use MSR bitmaps: Can set=Yes, Can clear=Yes
[   98.575382]   MONITOR exiting: Can set=Yes, Can clear=Yes
[   98.575382]   PAUSE exiting: Can set=Yes, Can clear=Yes
[   98.575383]   Activate secondary controls: Can set=Yes, Can clear=Yes
[   98.575384] Secondary Controls MSR: 0x153cfe00000000
[   98.575384]   Virtualize APIC accesses: Can set=No, Can clear=Yes
[   98.575384]   Enable EPT: Can set=Yes, Can clear=Yes
[   98.575385]   Descriptor-table exiting: Can set=Yes, Can clear=Yes
[   98.575385]   Enable RDTSCP: Can set=Yes, Can clear=Yes
[   98.575385]   Virtualize x2APIC mode: Can set=Yes, Can clear=Yes
[   98.575386]   Enable VPID: Can set=Yes, Can clear=Yes
[   98.575386]   WBINVD exiting: Can set=Yes, Can clear=Yes
[   98.575386]   Unrestricted guest: Can set=Yes, Can clear=Yes
[   98.575387]   APIC-register virtualization: Can set=No, Can clear=Yes
[   98.575387]   Virtual-interrupt delivery: Can set=No, Can clear=Yes
[   98.575387]   PAUSE-loop exiting: Can set=Yes, Can clear=Yes
[   98.575388]   RDRAND exiting: Can set=Yes, Can clear=Yes
[   98.575388]   Enable INVPCID: Can set=Yes, Can clear=Yes
[   98.575388]   Enable VM functions: Can set=Yes, Can clear=Yes
[   98.575389]   VMCS shadowing: Can set=No, Can clear=Yes
[   98.575389]   Enable ENCLS exiting: Can set=No, Can clear=Yes
[   98.575389]   RDSEED exiting: Can set=Yes, Can clear=Yes
[   98.575390]   Enable PML: Can set=No, Can clear=Yes
[   98.575390]   EPT-violation #VE: Can set=Yes, Can clear=Yes
[   98.575390]   Conceal VMX from PT: Can set=No, Can clear=Yes
[   98.575391]   Enable XSAVES/XRSTORS: Can set=Yes, Can clear=Yes
[   98.575391]   Mode-based execute control for EPT: Can set=No, Can clear=Yes
[   98.575392]   Sub-page write permissions for EPT: Can set=No, Can clear=Yes
[   98.575392]   Intel PT uses guest physical addresses: Can set=No, Can clear=Yes
[   98.575392]   Use TSC scaling: Can set=No, Can clear=Yes
[   98.575393]   Enable user wait and pause: Can set=No, Can clear=Yes
[   98.575393]   Enable ENCLV exiting: Can set=No, Can clear=Yes
</code></pre>
