# Buffer Overflow Lab 

Log into your VM through PuTTY

Disable the Address Space Randomization feature 

``sudo sysctl -w kernel.randomize_va_space=0``

Configure /bin/sh

``sudo ln -sf /bin/zsh /bin/sh``

Compile the shellcode

``make``

*This command runs a set of commands to create binaries*


Move to the code folder

``cd /home/seed/Buffer_Overflow_Setuid_Labsetup/code``


Turn off StackGuard 

``gcc -DBUF_SIZE=100 -m32 -o stack -z execstack -fno-stack-protector stack.c``
``sudo chown root stack``
``sudo chmod 4755 stack``


Create badfile

``touch badfile``


Debug stack.c

``gdb stack-L1-dbg``

*This opens a debug terminal*


Inside the terminal

- `b bof` (Sets a breakpoint at bof)
- `run` (Starts the execution of the program)
- `next` (Moves on from the first breakpoint)
- `p $ebp` (Gets the ebp value, SAVE THIS)
- `p &buffer` (Gets the buffer address, SAVE THIS) 
- ctrl+c (Leaves the debug terminal)


Edit the exploit.py file

`nano exploit.py`


Copy/Paste the 32-bit shellcode into the shellcode variable

Change the start number to something between 0 and the max range (EX: start = 420)

Change the ret to the buffer address plus the start value

Change the offset to (ebp address - buffer address) + L (4 for 32-bit)

Save and exit

Run exploit.py

``./exploit.py``

Launch attack

``./stack-L1``

*The output should look like this, resulting in a root shell!*
<img width="480" alt="image" src="https://user-images.githubusercontent.com/71207177/205788807-ecb511d3-01fa-4ee8-92fb-788faf609172.png">

