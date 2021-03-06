## Giang Bui's Team

#### C version

* Does not to integer overflow detection (part of the assignment requirements -- however no actual operations are done on the integers)
  * Never converts the input integer to an actual int, but has a fixed-length buffer of size 12 which leads me to believe that they took the number of digits possible in an int32 + the minus sign (11 chars total, + 1 for null terminator)
* The check for the output filename incorrectly uses the regex `^[A-z]+$` which matches everything in the ASCII table between A and z. This includes some symbols such as backslashes. On a Windows machine this could potentially be exploited to perform a directory traversal
* Output file is not checked if it's a symlink, or the executable itself.

#### Java version

- Input/output file paths
  * are not checked for symlinks
- They write the salt to an output file and write each byte with a newline appended (`salt[i] + "\n"`). This results in the byte being converted to a string, which *really* reduces the keyspace if the salt would ever need to be brute forced. Entropy is reduced anyways since the salt is read back in from the `salt.bin` file and bytes are read instead of reading each line and converting to a byte. i.e. instead of reading bytes in the range of 0-255, they read any character in `[\.0-9\-\\n]` (ASCII digits, dot, and newline).
- Password and salt are concated directly together and then hashed. Instead, the following operation should be performed: `hash(password + hash(salt))`
- IF a fixed-length hash size was used then hashes would have a high chance of collision for the same reason as the salt.


-----
## Dynamic Duo

#### C version
* validatePassword function is vulnerable to timing attacks (lander note: not relevant)
* multiplication does not check for overflow
* addition function does not check for overflow
* validateInt can be broken with input such as "000-000"
* uses `crypt()` which is insecure
* use of `rand()` in a cryptographic context
	* `rand()` is never seeded, resulting in the same salt every time
* `generateSalt()` takes a size, which in the context of password hashing is the same size as the buffer. The buffer is then written to an output file using `fprintf(storage, "%s\n", salt)`. The problem is that `generateSalt()` fills the entire buffer with characters, and does not leave a null terminator. While from what I tested this does not show to be exploitable, it could be under the right conditions.

#### Java Version
* password verification function compares passwords before hashing

-----
##Team TBA 

####C version
* allows entry of 2147483647 for integer which multiplies and adds to 0 without error
  - Should check `errno` after call to `strtol` to see if an error was encountered
  - Use `<limits.h>` instead of your own defines to ensure that the limits are consistent on every platform
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
* Password verification leaks memory (potential DOS)
![memory leak](file:///Users/ben/Pictures/Screenshots/Screen%20Shot%202015-11-12%20at%2011.15.29%20AM.png)

#### Java version 
* program crashes if input file is too large and does not contain new lines
	* `ln -s /dev/zero input.txt`
* Regex is applied to filename before opening file, symlinks can bypass this check easily

-----

## Team Tim Unger, Will Czifro, Sam Gronhovd

####C version
* can't compile right now


#### Java version
* program accepts pipe characters for name input
* program crashes when trying to read large input file with no newlines (/dev/zero)
* does not resolve file path of input file before validating (requirements easily bypassed by symlinking)
* 
