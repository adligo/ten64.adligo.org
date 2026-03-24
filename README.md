# ten64.adligo.org

### Abstract

Ten64 is a number serialization format similar to hexadecimal.  The main differences are the use of [sextets (six bits)](#sextet) instead of [octets (aka bytes)](#octet).  Similar to [hexadecimal](#hexadecimal), ten64 can be used to create binary strings of arbitrary length.  However, it is often used to encode one or more [Modern Western (aka Arabic, Vedic)](#modern-western-numeral-system) integers, interpreting the binary as a big-endian integer.

## Draft RFC

- [rfc.xml](rfc.xml)
- [https://author-tools.ietf.org/](https://author-tools.ietf.org/)
-
## Discussion Channel

- [https://discord.gg/2tdHn55Xur](https://discord.gg/2tdHn55Xur)

# Introduction

This section is non-normative.

As computer systems scale in both volume and streaming throughput, standard JavaScript Object Notation [JSON](#json-rfc-8259) often introduce precision issues. This is particularly prevalent due to double-precision [floating-point](#floating-point) constraints.  Although double floating point precision is specified in the JSON spec. Parsers have interpreted this differently, most notably [Jackson](#fasterxml-jackson) and [Gson](#gson).
In addition, streaming formats like [base64](#base64-rfc-4648) are often fairly verbose to map binary data to octets.  This incurs a large cost when one bit is encoded as four bytes.  To solve these and related issues, Ten64 is introduced.

# TODO Below


It does NOT use the [Base64](#base64-rfc-4648) alphabet but a binary alphabet similar to hexadecimal 0-9,a-z,A-Z, '@' and'_' along with some additional special characters most notably '#','.' and ';','-', and '!'.  It is designed to be human and machine readable but is really designed to optimized the reading and writing of numbers for streaming and storage computer systems.  The @ and _ symbol and other alphabet symbols were chosen because they are NOT mathematical symbols, so theoretically this system could also be used embed numbers into programming languages in the future.
It will use a big ending binary system as follows;

## Special Characters;
```
#   Optional explicit beginning of #Ten64 binary stream / chunk
.   The Decimal or Number Space Separator
,   The Separator for Number Lists
;   Optional explicit end of #Ten64 number or number list / binary stream / chunk
-   The negative indicator
    Whitespace Characters including Line Feeds, Tabs, Spaces, Return Sequences, Etc
```

## Primary Virtual Binary Alphabet;
| [Primary ASCII](#ascii) / [UTF8 Character](#utf8) | [Western Numeral Value](#modern-western-numeral-system) |  Binary Sextet  |
|---------------------------------------------------|----------------------------|-------------------------|
| 0                                                 | 0                          | 000000                  |
| 1                                                 | 1                          | 100000                  |
| 2                                                 | 2                          | 010000                  |
| 3                                                 | 3                          | 110000                  |
| 4                                                 | 4                          | 001000                  |
| 5                                                 | 5                          | 101000                  |
| 6                                                 | 6                          | 011000                  |
| 7                                                 | 7                          | 111000                  |
| 8                                                 | 8                          | 000100                  |
| 9                                                 | 9                          | 100100                  |
| a                                                 | 10                         | 010100                  |
| b                                                 | 11                         | 110100                  |
| c                                                 | 12                         | 001100                  |
| d                                                 | 13                         | 101100                  |
| e                                                 | 14                         | 011100                  |
| f                                                 | 15                         | 111100                  |
| g                                                 | 16                         | 000010                  |
| h                                                 | 17                         | 100010                  |
| i                                                 | 18                         | 010010                  |
| j                                                 | 19                         | 110010                  |
| k                                                 | 20                         | 001010                  |
| l                                                 | 21                         | 101010                  |
| m                                                 | 22                         | 011010                  |
| n                                                 | 23                         | 111010                  |
| o                                                 | 24                         | 000110                  |
| p                                                 | 25                         | 100110                  |
| q                                                 | 26                         | 010110                  |
| r                                                 | 27                         | 110110                  |
| s                                                 | 28                         | 001110                  |
| t                                                 | 29                         | 101110                  |
| u                                                 | 30                         | 011110                  |
| v                                                 | 31                         | 111110                  |
| w                                                 | 32                         | 000001                  |
| y                                                 | 33                         | 100001                  |
| x                                                 | 34                         | 010001                  |
| z                                                 | 35                         | 110001                  |
| A                                                 | 36                         | 001001                  |
| B                                                 | 37                         | 101001                  |
| C                                                 | 38                         | 011001                  |
| D                                                 | 39                         | 111001                  |
| E                                                 | 40                         | 000101                  |
| F                                                 | 41                         | 100101                  |
| G                                                 | 42                         | 010101                  |
| H                                                 | 43                         | 110101                  |
| I                                                 | 44                         | 001101                  |
| J                                                 | 45                         | 101101                  |
| K                                                 | 46                         | 011101                  |
| L                                                 | 47                         | 111101                  |
| M                                                 | 48                         | 000011                  |
| N                                                 | 49                         | 100011                  |
| O                                                 | 50                         | 010011                  |
| P                                                 | 51                         | 110011                  |
| Q                                                 | 52                         | 001011                  |
| R                                                 | 53                         | 101011                  |
| S                                                 | 54                         | 011011                  |
| T                                                 | 55                         | 111011                  |
| U                                                 | 56                         | 000111                  |
| V                                                 | 57                         | 100111                  |
| W                                                 | 58                         | 010111                  |
| X                                                 | 59                         | 110111                  |
| Y                                                 | 60                         | 001111                  |
| Z                                                 | 61                         | 101111                  |
| @                                                 | 62                         | 011111                  |
| _                                                 | 63                         | 111111                  |


## All virtual binary sequences are interpreted as integers
  The following sequences explode as follows;
  ```
  #0.1;   expands to a list of integers 0, 1 which can also be interpreted as the decimal 0.1 depending on the context.
  #0.1.2; expands to a segmented number / list of integers 0, 1, 2.
  #01.11; expands the a list of integers 64, 65 which can also be interpreted as the decimal 64.65 depending on the context.
  #-1.8;  expands to a list of integers -1, 8 and can also be interpreted as a decimal -1.8 depending on the context.
  ```
## Segmented Numbers
  There are several Use-Cases for segmented numbers including Dates, Datetimes, MiliTimestamps, NanoTimestamps, IANA OIDs, ThreeDPoints and more.

## BigDecimals
  Big Decimals (i.e. Java or Javascript type) can be easily represented with Ten64, which encodes the EXACT number of Decimal Places.  This provides and advantage over JSON which uses IETF Double Precision Floating Point numbers, which can cause precision issues in transit.

## Dates
  Dates can be greatly condensed using the Segmented Number base class.  Dates should be standardized as the year ten64 followed by a dot, and one ten64 character for the month and one ten64 character for the day.  For example;
 ```
   #Vv.21;  expands to 2023-02-01
 ```

## Datetimes
  Datetimes add the additional timezone, (military time) hour and minute to the date.  The timezone, (military time) hour and minute each only take one ten64 character and can be encoded as follows;
  ```
    #Vv.216jR;  expands to 2023-02-01 CST 7:53 PM
  ```

## Lists
  Ten64 supports lists of number separated by commas, each number MAY include whitespace characters on either side of the number;
  ```
    #1,2,3,4;  expands to a number list of 1,2,3,4
    #1.2.3,4.5.6,7.8.9;  expands to a list of 3d points
  ```

## MiliTimestamp
  MiliTimestamps add a single ten64 character for seconds and separate milliseconds with an additional dot.
  ```
    #Vv.216jR1.2; expands to 2023-02-01 CST 7:53:01.2
    or with milliseconds expanded PM 2023-02-01 CST 7:53:01.002 PM
  ```

## NanoTimestamp
  NanoTimestamps add an additional dot, as the MiliSeconds can be multiple characters (with values 0-1000), and then multiple ten64 characters representing the additional (0-1,000,000,000) nanoseconds that are NOT tracked as milliseconds.
  ```
    #Vv.216jR1.2.7;  expands to 2023-02-01 CST 7:53:01.2.7 PM
    or with nanoseconds expanded 2023-02-01 CST 7:53:01.002000007 PM
  ```
 ## Points
   Ten64 can encode 2d, 3d and Nd points as segmented numbers.

## Modern Western Numeral System

Arabic, Vedic, and Ghubari and many other numeral systems were considered for use in this RFC.  I finally settled down on modern the name <b>The Modern Western Numeral System</b>.  It appears that numeral systems have influenced each other over the ages, And we will likely continue carbon dating each glyph 0-9 for some time, as we have recently carbon dated the glyph '0'.  In addition, even if we do find an older carbon date of a particular glyph, It could take a considerable amount of time to determine if that carbon dating references a different culture than the main culture which recorded the glyph.

```aiignore
Before we go on to analytically review the
Hindu-Indian Brahmagubta and Islamo-Arabic
Ghubari origin of the modern mathematical
numeral system which is now regarded as the
Western Numeral System
```
- [Origin of Modern Mathematical Numeral pg 46](#origin-of-modern-mathematical-numeral)


# Citations

##### Base64 RFC 4648

Josefsson, S., "The Base16, Base32, and Base64 Data Encodings," RFC 4648, October 2006, <https://datatracker.ietf.org/doc/html/rfc4648>.

##### FasterXML Jackson

FasterXML, "Jackson: Main Portal page for the Jackson project," GitHub, <https://github.com/fasterxml/jackson>.

##### Floating-Point

IEEE, "IEEE Standard for Floating-Point Arithmetic," IEEE 754-2019, July 2019, <https://ieeexplore.ieee.org/document/8766229>.

##### Gson

Google, "Gson: A Java serialization/deserialization library to convert Java Objects into JSON and back," GitHub, <https://github.com/google/gson>.


##### Hexadecimal

https://en.wikipedia.org/wiki/Hexadecimal "Wikipedia Contributors. (2026, March). Hexadecimal. Wikipedia."

##### JSON RFC 8259
Bray, T., "The JSON Data Interchange Format," RFC 8259, STD 90, December 2017, <https://www.rfc-editor.org/info/rfc8259>.

##### Octet

https://en.wikipedia.org/wiki/Octet_(computing) "Wikipedia Contributors. (2026, March). Octet. Wikipedia."

##### Origin of Modern Mathematical Numeral

Musa, A., "Origin of Modern Mathematical Numeral – 0, 1, 2, 3, 4, 5, 6, 7, 8, 9: the Hindu-Indian-Brahmagubta, The Islamo-Arabic or the West?", Mubi North Education Authority, Adamawa State – Nigeria, p. 46.

##### Sextet

https://en.wikipedia.org/wiki/Units_of_information "Wikipedia Contributors. (2026, March). Sextet. Wikipedia."

# Informational references.

Boucenna, A., "Origin of the numerals," June 2006, arXiv:math/0606699, <https://arxiv.org/abs/math/0606699>.

Katz, B., "Carbon Dating Reveals the History of Zero Is Older Than Previously Thought," Smithsonian Magazine, September 2017, <a href="https://www.smithsonianmag.com/smart-news/dating-ancient-indian-text-gives-new-timeline-history-zero-180964896/" target="_blank" >https://www.smithsonianmag.com/smart-news/dating-ancient-indian-text-gives-new-timeline-history-zero-180964896/</a>



