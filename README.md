# XINU Operating Systems Project - Kernel IPC Services and Process Hijacking

You may view this Lab Project handout here: https://www.cs.purdue.edu/homes/cs354/lab4/lab4.html

The objective of this lab is to enhance XINU's IPC services and utilize run-time stack manipulation to "hijack" a process by executing malware.

Part 1: Blocking send() IPC
This problem concerns the implementation of a blocking version of send(), bsend(), which has the same function definition as send(). The difference is that if the receiver's buffer is full, bsend() blocks until the receiver's buffer frees up. This is in contrast to send() which returns immediately with SYSERR indicating that sending has failed. Use your code from lab1 as the code base. Put bsend() in system/bsend.c.

Part 2: Hijacking a process by modifying its run-time stack
An important technique for modifying the run-time behavior of a process is ROP (return-oriented programming) which we utilized in Problem 3, lab3, to make a process execute code that it wasn't programmed to do. By modifying the return address pushed onto the stack of a newly created process, we induced it to make a detour to code that took time stamps to commence CPU usage measurement. In this problem, we will use the same technique to "hijack" a process by making it execute malware code. We will do so by modifying the run-time stack of a victim process while it is context-switched out so that when it eventually becomes current we induce ctxsw() to jump to the attacker's malware code. In the first attack, the attacker makes ctxsw() jump to malware function hellomalware() which prints a hello message and terminates the victim process. The attacker makes no effort to hide the attack. In the second attack, the attacker makes ctxsw() jump to its malware function quietmalware(). Instead of printing a message and terminating the victim process, quietmalware() surgically modifies the victim process's data -- a local variable in the app code -- then returns to resched() as if nothing had happened. The victim process will continue to execute, but when printing the value of the local variable, it will have been corrupted unbeknownst to the victim. Use your code base from lab1 but set QUANTUM to be 30 msec.
