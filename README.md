# Calculating 100 digits of pi on the PDP-8
This is an implementation of the Spigot algorithm for calculating the digits of pi.
Written in DEC FORTRAN IV for use in DEC OS/8 on a PDP-8 (real or emulated).

## Overview
The FORTRAN IV code is essentially a 1-1 port of the Pascal program presented by Stanley Rabinowitz and Stan Wagon in the 
paper ["A Spigot Algorithm for the Digits of Pi"](https://www.maa.org/sites/default/files/pdf/pubs/amm_supplements/Monthly_Reference_12.pdf).

I am *not* a mathematician, so I don't understand why this algorithm works :-) but that doesn't prevent me (or anyone else) from 
implementing it!

## Compiling
I am using the [PiDP-8/I](https://obsolescence.wixsite.com/obsolescence/pidp-8) for my PDP-8 work as I don't have the money nor the space for a real PDP-8, unfortunately.
Anyway, procedures should be the same for compiling and executing the program.

### Get the source code on your PiDP-8/I system
Some familiarity with the handling of the PiDP-8/I and simh in general is assumed.
I won't describe the details, but several options can be used:
* Use the E8 editor on the PiDP-8/I and simply copy/and paste the code, and save with Ctrl-Z.
* Use `scp`to push the files to your host running simh, and then attach the files to the PTR device and use PIP to transfer the files to OS/8.
* Use USB sticks with the files, and use the PiDP-8/I options to mount the file as a paper tape; and
as before, use PIP to transfer files.

Whatever method you use, you should end up with the two files `SPIGOT.FT` and `PRTDIG.FT` on the OS/8 file system.

### Compile using FORTRAN IV
First compile the subroutine for printing:
```bash
.R F4                
*PRTDIG,PRTDIG,PRTDIG<PRTDIG.FT

.
```
And next compile the main program:
```bash
.R F4                
*SPIGOT,SPIGOT,SPIGOT<SPIGOT.FT

.
```
You should end up with a `SPIGOT.RL` and a `PRTDIG.RL` file.
### Create a load module
Use the FORTRAN IV loader for collecting the modules.
Please NOTE that the '$' sign at the end of the line is **not** 
to be typed in literally! You must press the ESC key instead of the ENTER key after the last 'L' in the line, which will output the '$' sign and execute the LOAD command.
```bash
.R LOAD
*SPIGOT.LD<PRTDIG.RL,SPIGOT.RL$
.
```
You should have a file named `SPIGOT.LD` on your OS/8 disk.
### Run the program
To run the program, jyst type:
```bash
.EXE SPIGOT.LD

SPIGOT PDP-8 FORTRAN IV -- BUILD 230325.1230
0
3
1
4
```
A new digit appears with a few seconds interval. For a 100 digits, this is about 9 minutes
if the simh is throttled for 333K instructions per second (close to real PDP-8 CPU speed).
The program actually also prints to the `LPT:` device, so you can capture this 
if you want to. You can use simh and Ctrl-E to exit and make an `att lpt myprinter` followed by `go` to 
collect the data in a file.
