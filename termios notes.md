#### Terminal Shortcuts and Modes

`Ctrl`+`S`: `XOFF` ("transmit off"), stop sending output (in case your teletype terminal needs to catch up with typing 
output)

`Ctrl`+`Q`: `XON` ("transmit on"), resume sending output

`Ctrl`+`Y`: suspend program to background (use `fg` to bring to foreground), like `Ctrl`+`Z` but it waits
for the program to read input before it suspends it

`Ctrl`+`C`: `SIGINT`, interrupt signal

`Ctrl`+`Z`: `SIGTSTP`, terminal stop signal

`Ctrl`+`\`: `SIGQUIT`, quit signal

`Ctrl`+`V`: "Verbatim", sends next character verbatim (e.g. `Ctrl`+`V` followed by `Ctrl`+`C` will send `0x03` instead of `SIGINT`)

`Ctrl`+`M`: Should produce `0x0C` (because it's the 13th letter), but the terminal converts `'\r'` (`0x0C`) to `'\n'` (`0x0A`) when the `ICRNL` flag is on


#### local modes (`c_lflag`):

`ECHO`: echo what you type

`ICANON`: canonical mode, wait for enter to read input

`ISIG`: respond to `SIGINT`/`SIGTSTP`

`IEXTEN` 


#### input modes (`c_iflag`):

`ICRNL`: converts `'\r'` (`0x0C`) to `'\n'` (`0x0A`)

`IXON`: whether to accept `XOFF`/`XON`

`BRKINT`: send `SIGINT` on a break condition

`INPCK`: enables parity checking

`ISTRIP`: Strips the 8th bit of every byte (like you're not holding `Ctrl` on an old keyboard)


#### output modes (`c_oflag`):

`OPOST`: output processing, prints '\r\n' when asked to print '\n'


#### control modes (`c_cflag`):

`CS8`: Sets byte size to 8 bits. A mask, not flag.


#####  `c_cc` array:

`VMIN`: minimum number of bytes that read needs before returning

`VTIME`: timeout for read (units of 100 ms)


`c_iflag`: input modes (low-level input handling)

`c_oflag`: output modes (low-level output handling)

`c_lflag`: local modes (high-level input handling)

`c_cflag`: control modes (serial port behavior)
