
-----
## Dynamic Duo

#### C version
* validatePassword function is vulnerable to timing attacks
* multiplication does not check for overflow
* addition function does not check for overflow
* validateInt can be broken with input such as "000-000"



#### Java Version
* password verification function compares passwords before hashing

-----
##Team TBA 

####C version
* allows entry of 2147483647 for integer which multiplies and adds to 0 without error
* all entries can be blank and the program reports no errors (errors show up in error log)
* program segfaults if "input.txt" is not present and no input filename is entered. 
* negative input of -2147483646 will crash the program
* any 10 digit negative number (11 chars w/ minus sign) will crash the program

####Java version
_not available_

-----
## Team Kraken Koders

### C version
* rolled own encryption (not even encryption, actually just obfuscation)
	* encryption used instead of hashing
	* encryption function no better than simple substitution cipher
	* would be vulnerable to chosen plaintext attack

* get name function does not work as expected; First name prompt expects two words separated by space
* integer multiplication overflows without error
* Leaks a lot of memory
![Memory leak](file:///Users/ben/Pictures/Screenshots/Screen%20Shot%202015-11-11%20at%2012.28.28%20PM.png)
* File reading function is broken due to initialized value
	![never ending loop](file:///Users/ben/Pictures/Screenshots/Screen%20Shot%202015-11-11%20at%201.18.41%20PM.png)
	* "bool" not initialized, only set if file is null initially.  User is trapped in loop if correct file name is entered the first time. 
* 
#### Java version
* entering a valid but hidden file completely breaks the program. It becomes stuck in an infinite loop
* If file path does not contain "users", the program gets stuck in an infinite loop
* Program crashes if user enters decimal number at integer prompt
	![bad integer](file:///Users/ben/Pictures/Screenshots/Screen%20Shot%202015-11-11%20at%201.54.50%20PM.png)
* verifyPassword function vulnerable to timing attack
* storePassword does not store password in a recoverable way. Writes a bunch of junk to file and stores the MD5 amongst the random junk. 
* non-existent input files are accepted causing the program to abort without writing results

-----

##Cyber Bacon

#### C version
* segfaults when input file does not exist
![segfault](file:///Users/ben/Pictures/Screenshots/Screen%20Shot%202015-11-11%20at%202.42.11%20PM.png)
* does not accept negative integers
![invalid integer](file:///Users/ben/Pictures/Screenshots/Screen%20Shot%202015-11-11%20at%202.40.42%20PM.png)
* uses MD5, a broken hash function since ~2005

#### Java version 