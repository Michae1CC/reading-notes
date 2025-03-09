
```
------------------  
netmask = 255.255.224.0    // the lecture had this as 160 which is an invalid netmask (the process described in the lecture still works though).  
  
How many networks are there here?  
130.102.154.200  
130.102.159.10  
130.102.162.1  
130.102.175.17  
130.102.191.254  
130.102.192.9  
  
  
-----------------  
  
Convert between from CIDR to net masks:  
  
192.168.12.0/21  
192.192.0.0/12  
  
-----------------  
  
Give the largest network which includes  
102.102.10.19  
102.102.10.31  
but not  
102.102.10.69  
  
Specify both CIDR and netmask  
  
-----------------  
  
How many interfaces could be connected to the following networks:  
  
10.0.0.0/12  
102.103.115.17/25  
  
------------------  
  
Each of the following machines wishes to send a broadcast message to its network.  What addresses should they use?  
  
  
192.168.201.12 (MASK=255.248.0.0)  
192.168.201.12 (MASK=255.192.0.0)  
192.168.201.12 (MASK=255.255.117.0)  
  
------------------  
  
Q9 G from 2012 exam:  
  
Fill in the following table with the name of each layer of the IP stack and the name of the addressing mechanism they use:  
   Name   | Addr mech  
5:        |  -------  
4:        |  
3:        |  
2:        |  
1:        | ---------
```

Answers:

```
Answers

-------------------------------------
netmask = 255.255.224.0    // the lecture had this as 160 which is an invalid netmask (the process described in the lecture still works though).

How many networks are there here?
130.102.154.200
130.102.159.10
130.102.162.1
130.102.175.17
130.102.191.254
130.102.192.9


=======

You can identify the network by bitwise and-ing with the netmask:


130.102.128.0
130.102.128.0
130.102.160.0
130.102.160.0
130.102.160.0
130.102.192.0

So there are 3 networks

--------------------

Convert between from CIDR to net masks:

192.168.12.0/21
192.192.0.0/12

====================================

21 = 2*16+5  so it will be two bytes plus 5 MSB (most significant bits)
255.255.248.0 

12 = 8 + 4
255.240.0.0

--------------------------------

Give the largest network which includes
102.102.10.19
102.102.10.31
but not
102.102.10.69

Specify both CIDR and netmask

=======================

We need to find the leftmost bit which is the same for the addresses we want in but different for the address we want out.

Left 3 quads are the same so look at the 4th.

19 = 00010011
31 = 00011111
69 = 01000101

So we can use the 7th bit to distinguish.
To get the netmask set all bits left of 6th to 1.
255.255.255.192

For CIDR
102.102.10.0/26

-----------------------------------------


How many interfaces could be connected to the following networks:

10.0.0.0/12
102.103.115.17/25

====================================

1. How big is the host part?
2. how many numbers could you represent in that many bits?
3. subtract 2 for the reserved.

10.0.0.0/12       ->  32-12=20   so 2^20-2
102.103.115.17/25 ->  32-25=7    so 2^7-2 

------------------------------
Broadcast address for each network?

192.168.201.12 (MASK=255.248.0.0)
192.168.201.12 (MASK=255.192.0.0)
192.168.201.12 (MASK=255.255.117.0)


=========================


192.168.201.12 (MASK=255.248.0.0)
First we find the network address:

192.168.0.0 
Now we set the rightmost 3+8+8 bits to 1.   Why 3? Because the 5MSB add up to 248 which leaves 3. 
So we would be adding:
0.7.255.255

Which gives:
192.175.255.255
===

192.168.201.12 (MASK=255.192.0.0)
Network address:
192.128.0.0

Set the 6+8+8 LSB bits to 1.
0.63.255.255

192.191.255.255

===

192.168.201.12 (MASK=255.255.117.0)

117 is not a valid quad for a netmask because it is not contiguous.
128 < 117 < 128+64
[This one was deliberate].


=================================


Application | -----
Transport   | port
Network     | ip
(Data)link  | MAC
physical    | -----
```




### CSSE2010

The course, including lecture notes and exercises can be found at

[http://ocw.mit.edu/courses/electrical-engineering-and-computer-science/6-087-practical-programming-in-c-january-iap-2010/](http://ocw.mit.edu/courses/electrical-engineering-and-computer-science/6-087-practical-programming-in-c-january-iap-2010/)

Lectures 1 through 6 are most relevant (excluding linked lists and binary trees).

CProgramming.com - online tutorial

See  [http://www.cprogramming.com/tutorial/c-tutorial.html](http://www.cprogramming.com/tutorial/c-tutorial.html)

Essential C - Stanford

[http://cslibrary.stanford.edu/101/EssentialC.pdf](http://cslibrary.stanford.edu/101/EssentialC.pdf)

Mano, M.  
Digital Design, 3rd ed.  
Prentice Hall, 2002  

  
Pages 27 to 30 cover binary logic and logic gates.  
Pages 167 to 179 cover flip-flops.

Mano, M and Kime, C  
Logic and Computer Design Fundamentals, 2nd ed  
Prentice Hall, 2001  
  

Pages 27 to 45 cover binary logic and Boolean algebra.  
Pages 111 to 130 cover combinational logic including encoders, multiplexers and adders.

Gajski, D  
Principles of Digital Design  
Prentice Hall, 1997  
  

Pages 27 to 43 cover number representations, binary addition and multiplication.  
Pages 267 to 280 cover sequential circuits including registers, shift registers and counters.

Tanenbaum, A  
Structured Computer Organization, 5th ed  
Prentice Hall, 2006  
  

Pages 51 to 73 cover computer organisation including the fetch-execute cycle, pipelining and memory organisation.  
Pages 156 to 157 cover an example ALU.  
Pages 331 to 339 introduce ISA and describe the status register.  
Pages 692 to 698 cover floating point formats.

Patt, Y and Patel, S  
Introduction to Computing Systems: From Bits and Gates to C and Beyond, 2nd ed.  
McGraw Hill, 2004  
  

Pages 97 to 101 cover the von Neumann model

Null, L and Lobur, J  
The Essentials of Computer Organization and Architecture, 2nd ed  
Jones and Bartlett Publishers, 2006  
  

Pages 243 to 268 cover Instruction Set Architecture  
Pages 302 to 310 cover Virtual Memory and Paging

Kernighan, B. and Ritchie, D.  
The C Programming Language (2nd edition)  
Prentice-Hall, 1988

This is the classic book on the C Programming Language.

Harbison, S. and Steele, G.  
C: A Reference Manual, 5th ed  
Prentice Hall, 2002.


### COMS3200

Computer Networking: A Top-Down Approach, 7th Edition (2017)  
James F. Kurose, University of Massachusetts, Amherst  
Keith W. Ross, Polytechnic University, Brooklyn