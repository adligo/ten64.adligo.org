# ten64.adligo.org

### Abstract

Ten64 is a number serialization format similar to hexadecimal.  The main differences are the use of [sextets (six bits)](#sextet) instead of [octets (aka bytes)](#octet).  Similar to [hexadecimal](#hexadecimal), ten64 can be used to create binary strings of arbitrary length.  However, it is often used to encode one or more [Modern Western Integers (aka Arabic, Vedic)](#modern-western-numeral-system) , interpreting the big-endian binary as a one or more  [Modern Western Integers](#modern-western-numeral-system).

The motivation for Ten64 is to encode numbers in a compact and human-readable format similar to [Base58](#base58).  However, Ten64 is designed to be optimized for use with numbers commonly used in identifiers, such as [DOIDs](#doid-repo), [IANA OIDs](#iana-oids), [UUIDs](#uuid), dates, time(stamp)s, coodinates, and more.  Unlike [Base58](#base58), and more like [hexadecimal](#hexadecimal) Ten64 aligns the [Modern Western Numerals](#modern-western-numeral-system) with the respective big-ending binary (i.e.; 0 → 0, 1 → 1, 2 → 11, etc).

## Authors

- S Morgan
- R Ismo

## Draft RFC

- [rfc.xml](rfc.xml)
- [https://author-tools.ietf.org/](https://author-tools.ietf.org/)

## Discussion Channel

- [https://discord.gg/2tdHn55Xur](https://discord.gg/2tdHn55Xur)

# Implementations

This Github project will contain the Java implementation.  A Typescript implementation will live at;

- [https://github.com/adligo/ten64.ts.adligo.org](https://github.com/adligo/ten64.ts.adligo.org)

# Introduction

This section is non-normative.

As computer systems scale in both volume and streaming throughput, standard JavaScript Object Notation [JSON](#json-rfc-8259) often introduce precision issues. This is particularly prevalent due to double-precision [floating-point](#floating-point) constraints.  Although double floating point precision is specified in the [JSON RFC](#json-rfc-8259). Parsers have interpreted this differently, most notably [Jackson](#fasterxml-jackson) and [Gson](#gson).
In addition, streaming formats like [base64](#base64-rfc-4648) are often fairly verbose to map binary data to octets.  This incurs a large cost when one bit is encoded as four bytes.  To solve these and related issues, Ten64 is introduced.

Alternative notations and bases have historically been explored to
improve human-machine interfaces, ranging from early discussions on
[binary notations](#CACM-Binary) to modern concepts like
[Hexadecimal](#hexadecimal), and [Bioctal](#bioctal)
. Ten64 builds on this history by utilizing a
64-character binary alphabet composed of standard [ASCII-7](#ascii-7) /
[UTF-8 characters](#utf-8). By
avoiding standard mathematical symbols, Ten64 allows exact numbers to
be embedded directly into programming environments without compiler
confusion.</t>

## Special Characters Introduction

It does NOT use the [Base64](#base64-rfc-4648) alphabet but a binary alphabet similar to hexadecimal 0-9,a-k,$,m-z,A-H,+,J-N,:,P-Z, '@' and'_' along with some additional special characters most notably '#','.' and ';', and '-'.  It is designed to be human and machine readable but is really designed to optimized the reading and writing of numbers for streaming and storage computer systems.  The @ and _ symbol and other alphabet symbols were chosen because they are NOT mathematical symbols, so theoretically this system could also be used embed numbers into programming languages in the future.
It will use a big ending binary system as follows;

## Special Characters Details
```
#   Optional explicit beginning of #Ten64 binary section, use depending on context.
.   The Decimal or Number Space Separator
,   The Separator for Number Lists
;   Optional explicit end of #Ten64 sequence.
-   The negative indicator
    Whitespace Characters: including Line Feeds, Tabs,
       Spaces, Return Sequences, etc. MAY be included
       and MUST cause end of interpretation of the
       Ten64 sequence.
    Other Non Alphabet Characters: MAY be included
       and MUST cause end of interpretation of the
       Ten64 sequence.
```

## Ten64 Alphabet Mappings;
| [Primary ASCII](#ascii) / [UTF8 Character](#utf8) | [Modern Western Integer](#modern-western-integers) | Big-Ending Binary [Sextet](#sextet) |
|---------------------------------------------------|---------------------------------------------|-----------------------------------------|
| 0                                                 | 0                                           | 000000                                  |
| 1                                                 | 1                                           | 100000                                  |
| 2                                                 | 2                                           | 010000                                  |
| 3                                                 | 3                                           | 110000                                  |
| 4                                                 | 4                                           | 001000                                  |
| 5                                                 | 5                                           | 101000                                  |
| 6                                                 | 6                                           | 011000                                  |
| 7                                                 | 7                                           | 111000                                  |
| 8                                                 | 8                                           | 000100                                  |
| 9                                                 | 9                                           | 100100                                  |
| a                                                 | 10                                          | 010100                                  |
| b                                                 | 11                                          | 110100                                  |
| c                                                 | 12                                          | 001100                                  |
| d                                                 | 13                                          | 101100                                  |
| e                                                 | 14                                          | 011100                                  |
| f                                                 | 15                                          | 111100                                  |
| g                                                 | 16                                          | 000010                                  |
| h                                                 | 17                                          | 100010                                  |
| i                                                 | 18                                          | 010010                                  |
| j                                                 | 19                                          | 110010                                  |
| k                                                 | 20                                          | 001010                                  |
| $                                                 | 21                                          | 101010                                  |
| m                                                 | 22                                          | 011010                                  |
| n                                                 | 23                                          | 111010                                  |
| o                                                 | 24                                          | 000110                                  |
| p                                                 | 25                                          | 100110                                  |
| q                                                 | 26                                          | 010110                                  |
| r                                                 | 27                                          | 110110                                  |
| s                                                 | 28                                          | 001110                                  |
| t                                                 | 29                                          | 101110                                  |
| u                                                 | 30                                          | 011110                                  |
| v                                                 | 31                                          | 111110                                  |
| w                                                 | 32                                          | 000001                                  |
| y                                                 | 33                                          | 100001                                  |
| x                                                 | 34                                          | 010001                                  |
| z                                                 | 35                                          | 110001                                  |
| A                                                 | 36                                          | 001001                                  |
| B                                                 | 37                                          | 101001                                  |
| C                                                 | 38                                          | 011001                                  |
| D                                                 | 39                                          | 111001                                  |
| E                                                 | 40                                          | 000101                                  |
| F                                                 | 41                                          | 100101                                  |
| G                                                 | 42                                          | 010101                                  |
| H                                                 | 43                                          | 110101                                  |
| +                                                 | 44                                          | 001101                                  |
| J                                                 | 45                                          | 101101                                  |
| K                                                 | 46                                          | 011101                                  |
| L                                                 | 47                                          | 111101                                  |
| M                                                 | 48                                          | 000011                                  |
| N                                                 | 49                                          | 100011                                  |
| :                                                 | 50                                          | 010011                                  |
| P                                                 | 51                                          | 110011                                  |
| Q                                                 | 52                                          | 001011                                  |
| R                                                 | 53                                          | 101011                                  |
| S                                                 | 54                                          | 011011                                  |
| T                                                 | 55                                          | 111011                                  |
| U                                                 | 56                                          | 000111                                  |
| V                                                 | 57                                          | 100111                                  |
| W                                                 | 58                                          | 010111                                  |
| X                                                 | 59                                          | 110111                                  |
| Y                                                 | 60                                          | 001111                                  |
| Z                                                 | 61                                          | 101111                                  |
| @                                                 | 62                                          | 011111                                  |
| _                                                 | 63                                          | 111111                                  |

# Use-Cases



# Binary

Ten64 is a system to create [BitSlotMaps#1.3.6.1.4.1.33097.1.1.3](#bitslotmaps-13614133097113) (aka, BinaryStrings, BitVectors, BitSets, etc).  Typically, these are turned into integer or decimal numbers in memory.  Ten64 MAY also be used to represent arbitrary [octet arrays](#octet).  Note, conversion to [octet arrays](#octet) is NOT the primary [Use-Case](#use-case) of Ten64, and that round tripping between [octet arrays](#octet) MAY introduce issues.

## Binary big ending sequences MAY be interpreted as integers

  The following sequences explode as follows;

  ```
  #0.1;   expands to a list of integers 0, 1 which MAY also be interpreted as the decimal 0.1 depending on the context.
  #0.1.2; expands to a segmented number / list of integers 0, 1, 2.
  #01.11; expands the a list of integers 64, 65 which MAY also be interpreted as the decimal 64.65 depending on the context.
  #-1.8;  expands to a list of integers -1, 8 and MAY also be interpreted as a decimal -1.8 depending on the context.
  #-1.-9 expands to a list of integers -1, -9
  ```

## Ten64 to Octet (Byte) Array Conversion

When converting Ten64 binary to an array of [octets](#octet), all missing bits MUST be filled with zeros.  This is to ensure that the binary consists of complete [octets](#octet).

## Ten64 from Octet (Byte) Array Conversion

When converting arrays of octets to the Ten64 alphabet, all '0' characters from the Ten64 alphabet at the right MUST be omitted.

## Segmented Numbers
  There are several Use-Cases for segmented numbers including Dates, Datetimes, MiliTimestamps, NanoTimestamps, IANA OIDs, ThreeDPoints and more.

## BigDecimals
  Big Decimals (i.e. Java or Javascript type) can be easily represented with Ten64, which encodes the EXACT number of Decimal Places.  This provides and advantage over [JSON](#json-rfc-8259) which uses [IETF Double Precision Floating Point](#floating-point) numbers, which can cause precision issues in transit.

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
   Ten64 can encode 2d, 3d, and Nd points as segmented numbers.

# Related Technologies

## Modern Western Numeral System

Arabic, Ghubari, and Vedic as well as many other numeral systems were considered for use in this RFC.  I finally settled down on modern the name <b>The Modern Western Numeral System</b>.  It appears that numeral systems have influenced each other over the ages, and we will likely continue carbon dating each glyph 0-9 for some time, as we have recently carbon dated the glyph '0'.  In addition, even if we do find an older carbon date of a particular glyph, It could take a considerable amount of time to determine if that carbon dating references a different culture than the main culture which recorded the glyph.

- [Origin of the Numerals](#origin-of-modern-mathematical-numeral)
- [Carbon Dating Reveals the History of Zero Is Older Than Previously Thought](#carbon-dating-zero)
```
Before we go on to analytically review the
Hindu-Indian Brahmagubta and Islamo-Arabic
Ghubari origin of the modern mathematical
numeral system which is now regarded as the
Western Numeral System.
```
- [Origin of Modern Mathematical Numeral pg 46](#origin-of-modern-mathematical-numeral)

### Modern Western Integers

Modern Western Integers are simply integers composed using the Modern Western Numerical System. <b>Modern Western Integers</b> MAY be positive, negative, or zero.

### Modern Western Decimal Numbers

Modern Western Decimal Numbers Are simply numbers using the modern Western numeral system, which contain a decimal point.

# Compatibility

When drafting Ten64, we went through great lengths to attempt to make it as compatible as possible with the largest number of usage environments.  However, it is impossible to have pristine compatibility with such a large variety of usage environments.

### URI and URI Template Compatibility

The following Ten64 alphabet characters MAY have issues with [URIs](#uri-rfc3986) and [URI Templates](#uri-templates-rfc6570).  The following section is in order of usage in Ten64.

### Leading Special Characters

##### URI Pound Symbol '#'

The pound symbol is a protected character by the [URIs RFC3986 section 3.2](https://www.rfc-editor.org/rfc/rfc3986#section-3.2).  When using Ten64 inside of URIs (aka URLs), the pound symbol MUST be either omitted or escaped with a percent sign '%23'.  Additionally, no major conflicts are foreseen with [URI Templates](https://www.rfc-editor.org/rfc/rfc6570).

##### URI Minus Symbol '-'

The minus symbol is an unreserved character by the [URIs RFC3986 section 2.3](https://www.rfc-editor.org/rfc/rfc3986#section-2.3).  No special treatment of this character is required for [URIs](#uri-rfc3986).  However, [URI Template Variable Names](https://www.rfc-editor.org/rfc/rfc6570#section-2.3) will require encoding as '%2D'.

### Alphabet Characters

##### URI Dollar Sign Symbol '$'

The dollar sign symbol is a sub-delim character by the [URIs RFC3986 section 3.4](https://www.rfc-editor.org/rfc/rfc3986#section-3.4).  It is explicitly permitted in the [query component of a URI](https://www.rfc-editor.org/rfc/rfc3986#section-3.4).  Although NOT required by Ten64, some languages MAY require URL encode the dollar symbol.  In particular, Bash, PowerShell, PHP, Perl, and Ruby where it is used to trigger variable expansion.

Simple String Expansion in [URI Template Variable Values](https://www.rfc-editor.org/rfc/rfc6570#section-3.2.2) MUST encode the dollar sign '$' as '%24'.  However, Reserved Expansion in [URI Template Variable Values](https://www.rfc-editor.org/rfc/rfc6570#section-3.2.3) SHOULD NOT encode the dollar sign.

##### URI Plus Symbol '+'

The plus symbol is a sub-delim character by the [URIs RFC3986 section 3.4](https://www.rfc-editor.org/rfc/rfc3986#section-3.4). It is explicitly permitted in the [query component of a URI](https://www.rfc-editor.org/rfc/rfc3986#section-3.4).  However, many languages and libraries (PHP, Python's urllib, Java Servlets) automatically decode this as a space, so it MAY need to be escaped as '%2B'.

Simple String Expansion in [URI Template Variable Values](https://www.rfc-editor.org/rfc/rfc6570#section-3.2.2) MUST encode the plus symbol as '%3B'.  However, Reserved Expansion in [URI Template Variable Values](https://www.rfc-editor.org/rfc/rfc6570#section-3.2.3) MUST NOT encode the dollar sign '$'.

##### URI Colon Symbol ':'

The colon symbol is a gen-delim character by the [URIs RFC3986 section 2.2](https://www.rfc-editor.org/rfc/rfc3986#section-2.2). It is explicitly permitted in the [query component of a URI](https://www.rfc-editor.org/rfc/rfc3986#section-3.4).
However, many languages and libraries (Express.js, Ruby on Rails, and Django) automatically decode this as a space, so it MAY need to be escaped as '%3A'.

Simple String Expansion in [URI Template Variable Values](https://www.rfc-editor.org/rfc/rfc6570#section-3.2.2) MUST encode the colon symbol ':' as '%3A'.  However, Reserved Expansion in [URI Template Variable Values](https://www.rfc-editor.org/rfc/rfc6570#section-3.2.3) MUST NOT encode the colon ':'.

### Internal and Trailing Special Characters

##### URI Period '.'

The period is unreserved in [URIs RFC3986](https://www.rfc-editor.org/rfc/rfc3986).  However, it is often used for relative paths, We do not anticipate any compatibility issues.

##### URI Semicolon ';'

The semicolon symbol is a sub-delim character by the [URIs RFC3986 section 3.4](https://www.rfc-editor.org/rfc/rfc3986#section-3.4). It is explicitly permitted in the [query component of a URI](https://www.rfc-editor.org/rfc/rfc3986#section-3.4).  There may be legacy issues with Python and Java servlets, which have NOT received security patches.  The semicolon MAY be omitted or encoded as '%3B'.

Simple String Expansion in [URI Template Variable Values](https://www.rfc-editor.org/rfc/rfc6570#section-3.2.2) MUST encode the seimcolon symbol ';' as '%3B'.  However, Reserved Expansion in [URI Template Variable Values](https://www.rfc-editor.org/rfc/rfc6570#section-3.2.3) MUST NOT encode the semicolon ';'.

### EJCN Compatibility


### JSON Compatibility

### Terminal-Shell Compatibility


### XML Compatibility

### Other Textual Compatibility


# Commentary

To improve human readability, we replaced these characters with their respective characters. The following chart shows the history of this.

```
21: lower case 'l' →  '$'
44: upper case 'I' → '%' → '+'
50: upper case 'O' → '?' → ':'
```

# Citations

##### ASCII-7

https://www.ansi.org/ "American Standards Association. (1963). American Standard Code for Information Interchange (ASA X3.4-1963)."

##### Base58

Nakamoto, S. (2009). "Base58 Encoding Scheme." Bitcoin Source Code. [https://github.com/bitcoin/bitcoin/blob/master/src/base58.cpp](https://github.com/bitcoin/bitcoin/blob/master/src/base58.cpp)

msporny, "The Base58 Encoding Scheme," IETF Draft (Individual Submission), February 2024. [https://datatracker.ietf.org/doc/html/draft-msporny-base58-03](https://datatracker.ietf.org/doc/html/draft-msporny-base58-03)

##### Base64 RFC 4648

Josefsson, S., "The Base16, Base32, and Base64 Data Encodings," RFC 4648, October 2006, <https://datatracker.ietf.org/doc/html/rfc4648>.

##### BitSlotMaps 1.3.6.1.4.1.33097.1.1.3

https://adligo.github.io/papers.adligo.com/data_structures/BitSlotMaps.html "Morgan, S. (2025). BitSlotMaps. Adligo Papers."

##### Bioctal

https://en.wikipedia.org/wiki/Bioctal "Community. (n.d.). Bioctal: Hexadecimal 2.0. Wikipedia."

##### Bitcoin
Nakamoto, S. (2008). *Bitcoin: A Peer-to-Peer Electronic Cash System*. [https://bitcoin.org/bitcoin.pdf](https://bitcoin.org/bitcoin.pdf)

##### CACM-Binary

https://doi.org/10.1145/364096.364107 "ACM. (1968). Letters to the editor: On binary notation. Communications of the ACM."

##### DOID-Repo

https://github.com/adligo/doid.adligo.org
    "doid.adligo.org: Domain Oracle Identifiers",
    Adligo, October 2024.

##### FasterXML Jackson

FasterXML, "Jackson: Main Portal page for the Jackson project," GitHub, <https://github.com/fasterxml/jackson>.

##### Floating-Point

IEEE, "IEEE Standard for Floating-Point Arithmetic," IEEE 754-2019, July 2019, <https://ieeexplore.ieee.org/document/8766229>.

##### Gson

Google, "Gson: A Java serialization/deserialization library to convert Java Objects into JSON and back," GitHub, <https://github.com/google/gson>.

##### Hexadecimal

https://en.wikipedia.org/wiki/Hexadecimal "Wikipedia Contributors. (2026, March). Hexadecimal. Wikipedia."

##### IANA OIDs

IANA, "Private Enterprise Numbers (PEN)", March 2026,
<https://www.iana.org/assignments/enterprise-numbers>.

##### JSON RFC 8259
Bray, T., "The JSON Data Interchange Format," RFC 8259, STD 90, December 2017, <https://www.rfc-editor.org/info/rfc8259>.

##### Octet

https://en.wikipedia.org/wiki/Octet_(computing) "Wikipedia Contributors. (2026, March). Octet. Wikipedia."

##### OData Protocol

OASIS. "OData Version 4.01. Part 1: Protocol." Edited by Michael Pizzo, Ralf Handl, and Martin Zurmuehl. 22 June 2021. OASIS Standard. [https://docs.oasis-open.org/odata/odata/v4.01/os/part1-protocol/odata-v4.01-os-part1-protocol.html](https://docs.oasis-open.org/odata/odata/v4.01/os/part1-protocol/odata-v4.01-os-part1-protocol.html).

##### Origin of Modern Mathematical Numeral

Musa, A., "Origin of Modern Mathematical Numeral – 0, 1, 2, 3, 4, 5, 6, 7, 8, 9: the Hindu-Indian-Brahmagubta, The Islamo-Arabic or the West?", Mubi North Education Authority, Adamawa State – Nigeria, p. 46.
https://www.academia.edu/9099310/Origin_of_Modern_Mathematical_Numeral_0_1_2_3_4_5_6_7_8_9_the_Hindu_Indian_Brahmagubta_The_Islamo_Arabic_or_the_West

##### Sextet

https://en.wikipedia.org/wiki/Units_of_information "Wikipedia Contributors. (2026, March). Sextet. Wikipedia."

##### URI RFC3986

[RFC 3986: Uniform Resource Identifier (URI): Generic Syntax](https://www.rfc-editor.org/rfc/rfc3986#section-3.4), T. Berners-Lee et al., January 2005. (Updated by [RFC 9110](https://www.rfc-editor.org/rfc/rfc9110) for HTTP-specific context).

##### URI Templates RFC6570

[RFC 6570: URI Template](https://www.rfc-editor.org/rfc/rfc6570), J. Gregorio et al., March 2012.

##### UTF-8

https://www.rfc-editor.org/rfc/rfc3629 "Yergeau, F. (2003). UTF-8, a transformation format of ISO 10646 (RFC 3629)."

##### UUID RFC9562

Davis, K., Peabody, B., and P. Leach, "Universally Unique IDentifiers (UUIDs)", RFC 9562, DOI 10.17487/RFC9562, May 2024, <https://www.rfc-editor.org/info/rfc9562>.

# Informational references.

##### Origin of the Numerals
Boucenna, A., "Origin of the numerals," June 2006, arXiv:math/0606699, <https://arxiv.org/abs/math/0606699>.

##### Carbon Dating Zero

Katz, B., "Carbon Dating Reveals the History of Zero Is Older Than Previously Thought," Smithsonian Magazine, September 2017, <a href="https://www.smithsonianmag.com/smart-news/dating-ancient-indian-text-gives-new-timeline-history-zero-180964896/" target="_blank" >https://www.smithsonianmag.com/smart-news/dating-ancient-indian-text-gives-new-timeline-history-zero-180964896/</a>



