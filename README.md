# xv6-microkernel
All the resources are provided by MIT 6.828 - Operating System Engineering - 2018 
https://pdos.csail.mit.edu/6.828/2018/schedule.html
* Lab 1: Booting a PC
  * Part 1: PC Bootstrap
  * Part 2: The Boot Loader
  * Part 3: The Kernel
# Lab nodes
* Physical Address Space(figure below)
  * Low Memory = RAM (reserved starts from the 0x00000000 to the ram storeage)
  * the second area about 384KB was reserved by the hardware uses(BIOS is inside this area)
  * BIOS = PC's basic input-output system(64KB region updatable)
    * BIOS is a software stored on a flash memory chip. In a PC, the BIOS is embedded on the motherboard.
    * controlls PC's booting process
    * BIOS initializes hardware divices and checking the memory installed.
    * BIOS loads the operating system from disk/CD/newtwork
    * Lastly, BIOS pass the control of the machine to the perating system
  * there will be unused memory because 32-bit architecture and second hole for 64-bit architecture becuase the BIOS need to be extended inorder to read 16-bit and 32-bit respectively for 32-bit and 64-bit archt.


* The ROM(read-only-memory) BIOS 
  




# The PC's Physical Address Space
We will now dive into a bit more detail about how a PC starts up.
A PC's physical address space is hard-wired to have the following general layout:<br /> 
<pre>
+------------------+  <- 0xFFFFFFFF (4GB) 
|      32-bit      |
|  memory mapped   | 
|     devices      | 
|                  | 
/\/\/\/\/\/\/\/\/\/\
 
/\/\/\/\/\/\/\/\/\/\ 
|                  | 
|      Unused      | 
|                  | 
+------------------+  <- depends on amount of RAM 
|                  |
|                  | 
| Extended Memory  | 
|                  | 
|                  | 
+------------------+  <- 0x00100000 (1MB) 
|     BIOS ROM     |
+------------------+  <- 0x000F0000 (960KB) 
|  16-bit devices, |
|  expansion ROMs  | 
+------------------+  <- 0x000C0000 (768KB)
|   VGA Display    | 
+------------------+  <- 0x000A0000 (640KB) 
|                  |
|    Low Memory    | 
|                  | 
+------------------+  <- 0x00000000 
</pre>
The first PCs, which were based on the 16-bit Intel 8088 processor, were only capable of addressing 1MB of physical memory. The physical address space of an early PC would therefore start at 0x00000000 but end at 0x000FFFFF instead of 0xFFFFFFFF. The 640KB area marked "Low Memory" was the only random-access memory (RAM) that an early PC could use; in fact the very earliest PCs only could be configured with 16KB, 32KB, or 64KB of RAM!

The 384KB area from 0x000A0000 through 0x000FFFFF was reserved by the hardware for special uses such as video display buffers and firmware held in non-volatile memory. The most important part of this reserved area is the Basic Input/Output System (BIOS), which occupies the 64KB region from 0x000F0000 through 0x000FFFFF. In early PCs the BIOS was held in true read-only memory (ROM), but current PCs store the BIOS in updateable flash memory. The BIOS is responsible for performing basic system initialization such as activating the video card and checking the amount of memory installed. After performing this initialization, the BIOS loads the operating system from some appropriate location such as floppy disk, hard disk, CD-ROM, or the network, and passes control of the machine to the operating system.

When Intel finally "broke the one megabyte barrier" with the 80286 and 80386 processors, which supported 16MB and 4GB physical address spaces respectively, the PC architects nevertheless preserved the original layout for the low 1MB of physical address space in order to ensure backward compatibility with existing software. Modern PCs therefore have a "hole" in physical memory from 0x000A0000 to 0x00100000, dividing RAM into "low" or "conventional memory" (the first 640KB) and "extended memory" (everything else). In addition, some space at the very top of the PC's 32-bit physical address space, above all physical RAM, is now commonly reserved by the BIOS for use by 32-bit PCI devices.

Recent x86 processors can support more than 4GB of physical RAM, so RAM can extend further above 0xFFFFFFFF. In this case the BIOS must arrange to leave a second hole in the system's RAM at the top of the 32-bit addressable region, to leave room for these 32-bit devices to be mapped. Because of design limitations JOS will use only the first 256MB of a PC's physical memory anyway, so for now we will pretend that all PCs have "only" a 32-bit physical address space. But dealing with complicated physical address spaces and other aspects of hardware organization that evolved over many years is one of the important practical challenges of OS development.
