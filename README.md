# ten64.adligo.org
This will house the Text Encoded Numbers as Base 64 binary project, with an accompanying RFC.   It will NOT use the regular Base64 alphabet but a virtual binary alphabet similar to hexidecimal 0-9,a-z,A-Z,@ along with some additional special characters most notably '#','.' and ';','-'.  It is designed to be human and machine readable but is really designed to optimized the reading and writing of numbers for streaming and storage computer systems.  The @ symbol and other alphabet symbals were chosen becase they are NOT mathmatical symbals, so theoretically this system could also be used embed numbers into programming languages in the future.
  It will use a big ending virtual binary system as follows;

## Special Characters;
```
#   Optional explicit begining of #Ten64 binary stream / chunk
.   The Decimal / List Seperator
;   Optional explicit end of #Ten64 binary stream / chunk
-   The negative indicator
```

## Primary Virtual Binary Alphabet;
| Primary ASCII / UTF8 Character   |  Arabic Numeral Value |  Virtual Binary Sextet  | 
|----------------------------------|-----------------------|-------------------------|
| 0                                |  0                    | 000000                  |
| 1                                |  1                    | 100000                  |
| 2                                |  2                    | 010000                  |
| 3                                |  3                    | 110000                  |
| 4                                |  4                    | 001000                  |
| 5                                |  5                    | 101000                  |
| 6                                |  6                    | 011000                  |
| 7                                |  7                    | 111000                  |
| 8                                |  8                    | 000100                  |
| 9                                |  9                    | 100100                  |
| a                                |  10                   | 010100                  |
| b                                |  11                   | 110100                  |
| c                                |  12                   | 001100                  |
| d                                |  13                   | 101100                  |
| e                                |  14                   | 011100                  |
| f                                |  15                   | 111100                  |
| g                                |  16                   | 000010                  |
| h                                |  17                   | 100010                  |
| i                                |  18                   | 010010                  |
| j                                |  19                   | 110010                  |
| k                                |  20                   | 001010                  |
| l                                |  21                   | 101010                  |
| m                                |  22                   | 011010                  |
| n                                |  23                   | 111010                  |
| o                                |  24                   | 000110                  |
| p                                |  25                   | 100110                  |
| q                                |  26                   | 010110                  |
| r                                |  27                   | 110110                  |
| s                                |  28                   | 001110                  |
| t                                |  29                   | 101110                  |
| u                                |  30                   | 011110                  |
| v                                |  31                   | 111110                  |
| w                                |  32                   | 000001                  |
| y                                |  33                   | 100001                  |
| x                                |  34                   | 010001                  |
| z                                |  35                   | 110001                  |
| A                                |  36                   | 001001                  |
| B                                |  37                   | 101001                  |
| C                                |  38                   | 011001                  |
| D                                |  39                   | 111001                  |
| E                                |  40                   | 000101                  |
| F                                |  41                   | 100101                  |
| G                                |  42                   | 010101                  |
| H                                |  43                   | 110101                  |
| I                                |  44                   | 001101                  |
| J                                |  45                   | 101101                  |
| K                                |  46                   | 011101                  |
| L                                |  47                   | 111101                  |
| M                                |  48                   | 000011                  |
| N                                |  49                   | 100011                  |
| O                                |  50                   | 010011                  |
| P                                |  51                   | 110011                  |
| Q                                |  52                   | 001011                  |
| R                                |  53                   | 101011                  |
| S                                |  54                   | 011011                  |
| T                                |  55                   | 111011                  |
| U                                |  56                   | 000111                  |
| V                                |  57                   | 100111                  |
| W                                |  58                   | 010111                  |
| Y                                |  59                   | 110111                  |
| X                                |  60                   | 001111                  |
| Y                                |  61                   | 101111                  |
| Z                                |  62                   | 011111                  |
| @                                |  63                   | 111111                  |


## All virtual binary sequences are interpreted as integers
  The following sequences explode as follows;
  ```
  #0.1;   expandes to a list of integers 0, 1 which can also be interperted as the decimal 0.1 depending on the context.
  #0.1.2; expandes to a segmented number / list of integers 0, 1, 2.
  #01.11; expands the a list of integers 64, 65 which can also be interperted as the decimal 64.65 depending on the context.
  #-1.8;  expands to a list of integers -1, 8 and can also be interpreted as a decimal -1.8 depending on the context.
  ```
## Segmented Numbers
  There are seveal Use-Cases for segmented numbers incluinding Dates, Datetimes, MiliTimestamps, NanoTimestamps, Ican OIDs, ThreeDPoints and more.
  
## Dates
  Dates can be greatly condenced using the Segmented Number base class.  Dates should be standardized as the year ten64 followed by a dot, and one ten64 character for the month and one ten64 character for the day.  For example;
 ```
   #Vv.21;  expands to 2023-02-01
 ```
  
## Datetimes
  Datetimes add the additional timezone, (military time) hour and minute to the date.  The timezone, (military time) hour and minute each only take one ten64 character and can be encoded as follows;
  ```
    #Vv.216jR;  expands to 2023-02-01 CST 7:53 PM
  ```
## MiliTimestamp
  MiliTimestamps add a single ten64 character for seconds and seperate miliseconds with an additional dot. 
  ```
    #Vv.216jR12;  expands to 2023-02-01 CST 7:53:01.2 PM
  ```
  
## NanoTimestamp
  NanoTimestamps add an additional dot, as the MiliSeconds can be multiple characters (with values 0-1000), and then multiple ten64 characters representing the additional (0-1,000,000) nanoseconds that are NOT tracked as miliseconds. 
  ```
    #Vv.216jR12.7;  expands to 2023-02-01 CST 7:53:01.2.7 PM  or with nanoseconds expanded 2023-02-01 CST 7:53:01.00200007 PM
  ```
  
