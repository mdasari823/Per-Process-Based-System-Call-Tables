*******************************************************************************
*********************************** README ************************************
*******************************************************************************

Notes:
======
	* This document gives implementation details and installation, use case
	  scenarios and commands for execution.
	
	* Please refer to README.pdf for the design details

Implementation Details:
=======================

The complete source involves the following files:
	
	- kernel/
		- fork.c, exit.c, sys.c
	- arch/x86/
		- process.c, unistd_64.c, syscalls_64.c
	- include/linux/
		- svector.h, sched.h, init_task.h, syscalls.h
	- hw3/*

Compilation and Installation:
=============================

To compile and install the modules, issue the following commands
	- Everything is written in shell script
		$make
		$./install.sh
		$./uninstall.sh

Execution:
==========

For all the executions, follow below commands:
	- goto hw3/
	- Issue the below command
		$./install.sh

1. Default syscall vector for all processes, inherted to children(8):

	- goto hw3/userland/
	- Issue the below command
		$./mkdir-test 
		$./mkdir-test fsl_os_svector1
		$./mkdir-test fsl_os_svector1 fsl_os_svector2
	- You can see the default vector creates folder but custom vectors just
	  prints a string (Overridden).

2. clone(2) CLONE_SYSCALLS flag support(6):

	- goto hw3/userland/
	- Issue the below command
		$./clone-test
		$./clone-test fsl_os_svector1
	- Issue above command with and without CLONE_SYSCALLS flag

3. un/loadable syscall vectors(10):

	- Shown in 1
	- exit all user processes
	- rmmod fsl_os_svector1 or 2 for unloading
	- For EC: show error handling
		- unload a vector
		- try to register this vector
		- should give error/invalid svector

4. ioctl(2) support: change syscall vectors(8):

	- Syscall is used instead of ioctl
	- Already shown in 1
	- Changed from fsl_os_svector1 to fsl_os_svector2
	- For EC: show System call inheritance
		- On fork, svector is inherited to child by default
		- We can even change the svector of child also
		- Run below command and see if its creates directory
			$ ./rfc fsl_os_svector1 fsl_os_svector2
		- Should not create directory

5. ioctl(2) support: list syscall vectors, vectors for PID(5):
	
	- goto hw3/userland/
	- Issue the below command
		$./list_vectors
	- For EC: load/uload svectors and show the list vectors
		- Show when there are no vectors also
	- For PID based listing, Issue below commands
		$./rfc fsl_os_svector1 fsl_os_svector2
		$./list_pid_sv
	- For EC: show that you can list the child process's svectors also
	- For EC: show the error handling part, give invalid process id

6. Useful examples of new syscall vectors(10):
	
	- Show the fsl_os_svector3 module

7. new version of clone(2) to start with different syscall vector(8): 

	- goto hw3/userland
	- Issue the following command
		$./clone2-test VECTOR_ID 
	- Parent process creates folder but child does not becuase, child
	  has registered to to overridden svector

8. Misc:

	- Doxygen documentation
	- Show crucial error handling: invalid pids, invalid svectors
	- Exploiting rw semaphore than slow mutex
	- System call volume monitoring on per-process basis
	- Simplest design ever, which requires only 5 lines of code to be added
	  Linux kernel
	- Highly flexible, scalable(critical feature) and Low overheads
	- More than 2 svectors
	- Show system call inheritance, wrapping, changing child process's
	  svectors multiple times
	- Using netlink sockets when kernel can also intiate the communication

9. Check the code cleanup and coding guidelines using below command:

	$./checkpatch.pl --terse --file --no-tree FILENAME

