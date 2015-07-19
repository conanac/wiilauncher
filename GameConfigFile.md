# gameconfig.txt file format #

Specify a 6 character game id followed followed by a colon, optionally replacing
game id characters with ? to match multiple regions/versions, and on the lines after
it you specify what settings to change or functions to execute. Repeat for as many
games as you want to add. Here's what is currently available:

# _value_ hooktype #
Specify hook type value:
  * 0 - No Hooks
  * 1 - VBI
  * 2 - Wii Pad
  * 3 - GC Pad
  * 4 - GXDraw
  * 5 - GXFlush
  * 6 - OSSleepThread
  * 7 - AXNextFrame

## Example: ##
```
hooktype = 6
```

This will result in the OSSleepThread hook being used.


---


# _function_ poke(address, value) #
Poke address with 32-bit value

## Example: ##
```
poke(80123458, 01234567)
```

Writes the 32-bit value 0x01234567 to the address 80123458.


---


# _function_ pokeifequal(address1, value1, address2, value2) #
If 32-bit value of address1 equals value1, then poke address2 with value2 (also 32-bit)

## Example: ##
```
pokeifequal(80123458, 01234567, 80234568, 98765432)
```

If address 80123458 contains 32-bit value 01234567, then write the 32-bit value 98765432 to address 80234568.


---


# _function_ searchandpoke(searchvalues, address1, address2, offset, value) #
Search for
the 32-bit values searchvalues between address1 and address2, and if found poke the
given offset off of the located address with the given value. If address1 is 0, use
the address first located by a prior invocation of this function or if no such address
exists, use a copy of address2 with all but the first 4-bits zeroed out for address1.
If address2 is zero then don't search, compare the search values against the values at
address1 and if equal write the given value at the given offset (if address1 was 0 and
was not able to be given a value, then nothing will be done).

## Examples: ##
```
searchandpoke(60000000 60000000 60000000, 80000000, 81800000, 0, 38600001)
searchandpoke(38600001 60000000 60000000, 0, 0, 4, 4E800020)
```

This will search for the value 0x600000006000000060000000 between addresses 80000000 and 81800000.
If found, it will write the 32-bit value 0x38600001 to the located address. Then for the
next searchandpoke the first located address will be reused (no search will be performed,
just an if equal to comparision against the given search values) and the value 0x4E800020
will be written at an offset of 0x4 off of that address.
If 0x600000006000000060000000 was found at more than one address then all but the
first instance will have been changed to 0x386000016000000060000000. The first instance
will have been changed to 0x386000014E80002060000000. To change the first instance to
0x386000014E80002060000000 and leave the rest of the values untouched, this could be
used instead:

```
searchandpoke(60000000 60000000 60000000, 80000000, 81800000, 0, 60000000)
searchandpoke(60000000 60000000 60000000, 0, 0, 0, 38600001)
searchandpoke(38600001 60000000 60000000, 0, 0, 4, 4E800020)
```

Or if you want to change all instances of 0x600000006000000060000000 within the given
memory range to 0x386000014E80002060000000 then this could be used:

```
searchandpoke(60000000 60000000 60000000, 80000000, 81800000, 0, 38600001)
searchandpoke(38600001 60000000 60000000, 80000000, 81800000, 4, 4E800020)
```


---


# _value_ codeliststart #
Specify code list start

## Example: ##
```
codeliststart = 80345678
```

Loads the code list to address 80345678.


---


# _value_ codelistend #
Specify code list end

## Example: ##
```
codelistend = 80358900
```

Prevents the code list from writing over address 80358900 or any address after it.


---


# _value_ videomode #
  * 0 - No override
  * 1 - Force NTSC
  * 2 - Force PAL60
  * 3 - Force PAL50

## Example: ##
```
videomode = 2
```

This forces the video mode to PAL60.


---


# _value_ language #
  * 0 - Default
  * 1 - Japanese
  * 2 - English
  * 3 - German
  * 4 - French
  * 5 - Spanish
  * 6 - Italian
  * 7 - Dutch
  * 8 - S. Chinese
  * 9 - T. Chinese
  * 10 - Korean

## Example: ##
```
language = 1
```

This forces the game language setting to Japanese.