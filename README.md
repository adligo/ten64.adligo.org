# Text Encoded Base 64 Numbers (Ten64)

### Abstract

Ten64 is a [positional numeral system](#positional-number-systems-wikipedia) for representing numeric values with an alphabet of 64 characters [(aka. radix/base 64)](#radix-wikipedia).  However, unlike most [positional number systems](#positional-number-systems-wikipedia), the characters are written in ascending (aka. [big-endian](#endian-wikipedia)) and NOT descending order.
The main differences are the use of [sextets (six bits)](#sextet) instead of [octets (aka. bytes)](#octet).  Similar to [hexadecimal](#hexadecimal), Ten64 can be used to create binary strings of arbitrary length.  Ten64 can encode one or more [Modern Western Numbers (aka Arabic, Vedic)](#modern-western-numeral-system), interpreting the characters respective [composite big-endian binary](#endian-wikipedia) as a one or more [Modern Western Numbers](#modern-western-numeral-system).

The motivation for Ten64 is to encode numbers in a compact and human-readable format similar to [Base58](#base58).  However, Ten64 is designed to be [optimized](#performance) for use with numbers commonly used in identifiers, such as [DIDs](#decentralized-identifiers-dids), [DOIDs](#doid-repo), [IANA OIDs](#iana-oids), [UUIDs](#uuid), [dates](#dates), [time](#time)[(stamp)s](#timestamps), [points](#points), and more.  Unlike [Base58](#base58), and more like [hexadecimal](#hexadecimal) Ten64 aligns the [Modern Western Numerals](#modern-western-numeral-system) with the respective big-ending binary (i.e.; 0 → 0, 1 → 1, 2 → 11, etc).  Finally, Ten64 provides a disk-based number encoding system for huge numbers and number streams.

## Authors

- S Morgan

### Acknowledgments

- R Ismo

R Ismo was instrumental in providing meticulous scrutiny to this project.  Although he disagreed and MAY still disagree with much of it, the discourse was wonderful and fully worthwhile.

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

Alternative notations and bases have historically been explored to
improve human-machine interfaces, ranging from early discussions on
[binary notations](#CACM-Binary) to modern concepts like
[Hexadecimal](#hexadecimal), and [Bioctal](#bioctal). Ten64 builds on this history by using a 64-character alphabet composed of standard [ASCII-7](#ascii-7) /
[UTF-8 characters](#utf-8).

In addition to providing a solid manor to represent numbers using characters and their respective binary representations, Ten64 Extends these representations with interpretation conventions.  These interpretation conventions will likely be specified by some sort of schema system like  [EJCN (Extensible JSON Classification Notation) Schemas](#ejcn-extensible-json-classification-notation-schemas), [JSON Schemas](#json-schemas) or [XML Schemas](#xml-schemas).

## Human-Readable Verses Human-Decipherable

In Ten64, we are targeting human-readable characters in the alphabet but do NOT intend to be human-decipherable.  Our definition of these terms MAY be specific to this paper.

##### What is Human-Readable?

Our intended meaning of the term human-readable in Ten64 is that a human will be able to distinguish the characters from each other.  We believe the best way of explaining this is with an example where one human would literally read the characters to another human over an audio-only telephone call.  For example, the word Adligo is spelled out using a spelled-out phonetic alphabet.

```
Capital A, as in Apple.
Small d, as in dog.
Small l, as in lake.
Small i, as in indigo.
Small g, as in golf.
Small o as in otter.
```

This is clearly human-readable.

##### What is Human-Decipherable?

Our intended meaning of the term human-decipherable in Ten64 is that most humans would be able to decipher the character stream.  Ten64 does not target human-decipherability.  We consider human-decipherability to be out of scope and may target another ID or RFC for this work.  Ten64 targets computer-decipherability and will rely on computer algorithms to translate Ten64 into in-memory integer and decimal numbers. In particular;

- [The Ten64 Integer Serialization Algorithm](#ten64-integer-serialization-algorithm)
- [The Ten64 Decimal Serialization Algorithm](#ten64-decimal-serialization-algorithm)

Although this is decipherable for some humans who are developers or software architects, this point might seem insignificant.  For the vast majority of the population, it is significant.  The point is that software tools will generally be used to decipher Ten64.

## Special Characters Introduction

Ten64 does NOT use the [Base64 RFC 4648](#base64-rfc-4648) alphabet but a alphabet similar to hexadecimal 0-9,a-k,$,m-z,A-H,+,J-N,!,P-Z, '@' and '_' along with some additional special characters most notably '#','.',',' and ';', '-', and the UNIX Line Feed 10 (0x0A in [hexadecimal](#hexadecimal)).  It is designed to be human and machine-readable but is really designed to [optimized](#performance) the reading and writing of numbers for streaming and storage computer systems.  The @ and _ symbol and other alphabet symbols were chosen because they human-readable. Theoretically, this system could also be used embed numbers into programming languages in the future.  However, usage MAY require the explicit starting character pound '#', and explicit termination semicolon character ';'.  It will use a big ending binary system as follows;

## Special Characters and Sequences Details
```
#   Optional explicit beginning of #Ten64 binary section, use depending on context.
#.. The optional explicit begining of a multiple line Ten64 sequence.
.   The Decimal or Number Space Separator
,   The Separator for Number Lists
;   Optional explicit end of #Ten64 sequence.
-   The negative indicator, and human-readable seperator
    Whitespace Characters: including Line Feeds, Tabs,
       Spaces, Return Sequences, etc. MAY be included
       and MUST cause end of interpretation of the
       Ten64 sequence.
UNIX Line Feeds:
    UNIX line feeds 10 / hex 0x0A
    MAY be used to continue a number sequence when the number
    sequence starts with '#..'.

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
| !                                                 | 50                                          | 010011                                  |
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

##### Shortened Identifiers

The primary Use-Case is simply an upgrade of [hexadecimal](#hexadecimal), where Ten64 provides a more succinct/compressed string of characters.  In addition, we target the encoding of numbers (n in the <b>Text Encoded Base 64 Numbers</b> title).  Generally, we are also targeting identifiers of all kinds, attempting to make them as short as possible to improve human readability.  Finally, we target human-readable compressed identifiers shortening [UUID's](#uuid-rfc9562) as follows;

```
# 36 character UUID
00000000-0000-0000-0000-000000000000
# to 27 characers
000000.000.000.000.00000000
# or 22 characters 128/6
0000000000000000000000
# or single or short sequences characters for small numbers
0
```

##### Huge Numbers and the 10-6-4 Convention

Ten64 is designed to be used to encode huge numbers, and is also [optimized](#performance) for this use-case.  The 10-6-4 convention suggests huge strings of numbers SHOULD be segmented into segments of ten characters, six characters, and then four characters.  In addition, these huge strings may be separated by the Unix line feed.  This also creates a convention of at MOST 92 characters per line SHOULD be used, it is an effort to improve human readability.

```
#..
0123456789-abcdef-ghij-k$mnopqrst-uvwyxz-ABCD-EFGH+JKLMN-!PQRST-UVWY-XZ@_012345-6789ab-cdef
ghijk$mnop-qrstuv-wyxz-ABCDEFGH+J-KLMN!P-QRST-UVWYXZ@012-345678-9abc-defghijk$m-nopqrs-tuvw
yxzABCDEFG-H+JKLM;
#..
0123456789-abcdef-ghij-k$mnopqrst-uvwyxz-ABCD-EFGH+JKLMN-!PQRST-UVWY-XZ@_012345-6789ab-cdef
ghijk$mnop-qrstuv-wyxz-ABCDEFGH+J-KLMN!P-QRST-UVWY.XZ@012-345678-9abc-defghijk$m-nopqrs-tuv
wyxzABCDEF-GH+JKL-M;
```

The above code illustrates a huge integer number and a huge decimal number. These kinds of Ten64 character sequences MAY be included as strings or as template literals in many programming languages.  In addition, these huge numbers may be streamed from disk or over the network.

# Binary

Ten64 is also a system to create [BitSlotMaps#1.3.6.1.4.1.33097.1.1.3](#bitslotmaps-13614133097113) (aka, BinaryStrings, BitVectors, BitSets, etc).  Numbers are read into memory using the [Ten64 integer serialization algorithm](#ten64-integer-serialization-algorithm).  Then the binary integer numbers can be reinterpreted per the user's wishes.

Ten64 MAY also be used to represent arbitrary [octet arrays](#octet).  Note, conversion to [octet arrays](#octet) is NOT the primary [Use-Case](#use-case) of Ten64, and that round tripping between [octet arrays](#octet) MAY introduce issues.

## Ten64 to Octet Array Conversion

When converting Ten64 binary to an array of [octets](#octet), all missing bits MUST be filled with zeros.  This is to ensure that the binary consists of complete [octets](#octet).

## Ten64 from Octet (Byte) Array Conversion

When converting arrays of octets to the Ten64 alphabet, all '0' characters from the Ten64 alphabet at the right MUST be omitted.

# Related Technologies

There are a ton of number libraries in various languages, [Java's BigInt](#bigint-java) likely influenced [ECMA Scripts BigDecimal](#ecma-262).  In addition, this [ECMA Script](#ecma-262) [BigDecimal](#bigdecimal-npm) implementation is based on [Java's BigDecimal](#bigdecimal-java).  We do NOT expect Ten64 to gain wide adoption over the [Modern Western Numeral System](#modern-western-numeral-system), since the [Modern Western Numeral System](#modern-western-numeral-system) is taught in early elementary schools and used all the way through advanced mathematics classes.

# Performance

In addition, to significantly reducing the number of characters required for encoding, which saves on disk space in files and on the number of bytes transferred over sockets.  Ten64 SHOULD be implemented using a more optimal algorithm to serialize and deserialize the data than [Modern Base-10 Numeral System](#modern-western-numeral-system) serialization uses.

To create integers from the Ten64 alphabet, algorithms SHOULD use case (aka switch) statements to convert the Ten64 alphabet into little-endian binary (used in the majority of in memory number systems).  Then the algorithms should shift the bits and use the binary and (i.e. &) operator to aggregate the number into integers.  This [Ten64 Integer Serialization#1.3.6.1.4.1.33097.0.2.4](#ten64-integer-serialization-algorithm) Algorithm will complete with a big O(s) time cost.

Comparison with other algorithms which use various serialization and deserialization forms to and from the modern western numerical system is generally much slower.

- [Number Conversion Calculator](#number-conversion-calculator)
- [Number Conversion at Instructables](#number-conversion-instructables)
- [Number Conversion at Khan Academy](#number-conversion-khan-academy)
- [Number Conversion at Lumen](#number-conversion-lumen)
- [Number Conversion at WikiHow](#number-conversion-wikihow)

However, When converting decimal numbers using [Ten64 Decimal Serialization#1.3.6.1.4.1.33097.0.2.5](#ten64-decimal-serialization-algorithm), division is required.  This incurs the following time cost depending on the division algorithm used.

- Serialization from [Modern Western Decimal Numbers](#modern-western-decimal-numbers) [O(c log c)](#ten64-decimal-serialization-algorithm)
- De-Serialization to [Modern Western Decimal Numbers](#modern-western-decimal-numbers) [O(cd²) - O(M(cd))(#ten64-decimal-serialization-algorithm)

# Compatibility

When drafting Ten64, we went through great lengths to attempt to make it as compatible as possible with the largest number of usage environments.  However, it is impossible to have pristine compatibility with such a large variety of usage environments.  In short, we recommend using Reserved Expansion from [URI Template Variable Values](https://www.rfc-editor.org/rfc/rfc6570#section-3.2.3) with Ten64, ommitting the '#' and ';'.

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

Simple String Expansion in [URI Template Variable Values](https://www.rfc-editor.org/rfc/rfc6570#section-3.2.2) MUST encode the dollar sign '$' as '%24'.  However, Reserved Expansion in [URI Template Variable Values](https://www.rfc-editor.org/rfc/rfc6570#section-3.2.3) MUST NOT encode the dollar sign '$'.

##### URI Plus Symbol '+'

The plus symbol is a sub-delim character by the [URIs RFC3986 section 3.4](https://www.rfc-editor.org/rfc/rfc3986#section-3.4). It is explicitly permitted in the [query component of a URI](https://www.rfc-editor.org/rfc/rfc3986#section-3.4).  However, many languages and libraries (PHP, Python's urllib, Java Servlets) automatically decode this as a space, so it MAY need to be escaped as '%2B'.

Simple String Expansion in [URI Template Variable Values](https://www.rfc-editor.org/rfc/rfc6570#section-3.2.2) MUST encode the plus symbol as '%2B'.  However, Reserved Expansion in [URI Template Variable Values](https://www.rfc-editor.org/rfc/rfc6570#section-3.2.3) MUST NOT encode the dollar sign '+'.

##### URI Exclamation Mark '!'

The exclamation mark is a sub-delim character by the [URIs RFC3986 section 2.2](https://www.rfc-editor.org/rfc/rfc3986#section-2.2). It is explicitly permitted in the [query component of a URI](https://www.rfc-editor.org/rfc/rfc3986#section-3.4).
However, many languages and libraries (Express.js, Ruby on Rails, and Django) automatically decode this as a space, so it MAY need to be escaped as '%21'.

Simple String Expansion in [URI Template Variable Values](https://www.rfc-editor.org/rfc/rfc6570#section-3.2.2) MUST encode the exclamation mark '!' as '%21'.  However, Reserved Expansion in [URI Template Variable Values](https://www.rfc-editor.org/rfc/rfc6570#section-3.2.3) MUST NOT encode the exclamation mark '!'.

### Internal and Trailing Special Characters

##### URI Period '.'

The period is unreserved in [URIs RFC3986](https://www.rfc-editor.org/rfc/rfc3986).  However, it is often used for relative paths, We do NOT anticipate any compatibility issues.

##### URI Semicolon ';'

The semicolon symbol is a sub-delim character by the [URIs RFC3986 section 3.4](https://www.rfc-editor.org/rfc/rfc3986#section-3.4). It is explicitly permitted in the [query component of a URI](https://www.rfc-editor.org/rfc/rfc3986#section-3.4).  There may be legacy issues with Python and Java servlets, which have NOT received security patches.  The semicolon MAY be omitted or encoded as '%3B'.

Simple String Expansion in [URI Template Variable Values](https://www.rfc-editor.org/rfc/rfc6570#section-3.2.2) MUST encode the seimcolon symbol ';' as '%3B'.  However, Reserved Expansion in [URI Template Variable Values](https://www.rfc-editor.org/rfc/rfc6570#section-3.2.3) MUST NOT encode the semicolon ';'.

### DID Compatibility

We removed the use of the colon ':' and replaced it with an exclamation point '!', specifically to increase compatibility with the [DID specification](#decentralized-identifiers-dids). However depending on usage, Ten64 MAY still require URI encoding for use with [DIDs](#decentralized-identifiers-dids).

```
did:ten64:abc123
```

### DOID Compatibility

[DOID](#doid-repo) uses Ten64 in a downstream manner, so it is fully compatible with Ten64.

### EJCN Compatibility

[EJCN (Extensible JSON Classification Notation)](#extensible-json-classification-notation-ejcn) uses Ten64 in a downstream manner, so it is fully compatible with Ten64.

### JSON Compatibility

[JSON Strings](#json-rfc-8259) are fully compatible with Ten64, as they use the backslash character '\\' for escaping characters.

### Terminal-Shell Compatibility

Various shells (i.e. Bash) will likely have some compatibility issues due to the dollar sign character '\$' use when referenceing ENVIRONMENT_VARIABLES and function parameters.  This can be overcome by escaping the dollar sign character '$'
 with a backslash '\$', or by wrapping the text in single quotes.

### XML Compatibility

[XML Strings](#xml-compatibility) are fully compatible with Ten64, as they use the are distinct from the big five XML characters '<', '>', '&', ", and '.

### Other Textual Compatibility

Ten64 SHOULD be generally compatible with most text files and programming syntax.  We have intentionally avoided commonly used characters like brackets, braces and parentheses '[',']','{','}','(',')'.  In addition, we have avoided common escape characters '%','&', etc.

# Interpretation Conventions

The Ten64 character sequences may have additional corresponding meta-data information from various schema sources like;

- [EJCN (Extensible JSON Classification Notation )](#ejcn-extensible-json-classification-notation)
- [EJCN (Extensible JSON Classification Notation ) Schemas](#ejcn-extensible-json-classification-notation-schemas)
- [JSON Schemas](#json-schemas)
- [XML Schemas](#xml-schemas)

Although the definitions of all these schema types are out of the scope of this document, we supply the following interpretation conventions.

## The Default Interpretation as Integers, Decimals, and Lists of Lists of Integers.

The following sequences explode as follows;

  ```
  #0.1;   expands to a list of integers 0, 1 which MAY also be interpreted as the decimal 0.1 depending on the context.
  #0.1.2; expands to a segmented number / list of integers 0, 1, 2.
  #01.11; expands the a list of integers 64, 65 which MAY also be interpreted as the decimal 64.65 depending on the context.
  #-1.8;  expands to a list of integers -1, 8 and MAY also be interpreted as a decimal -1.8 depending on the context.
  #-1.-9 expands to a list of integers -1, -9
  ```

## Segmented Number Interpretations Summary

There are several interpretation Use-Cases for segmented numbers, including Dates, Datetimes, MiliTimestamps, NanoTimestamps, [DOIDs](#doid-repo), [IANA OIDs](#iana-oids), Points and more.

## BigDecimal Interpretations

Big Decimals (i.e. Java or Javascript type) can be easily represented with Ten64, which encodes the EXACT number of Decimal Places.  This provides and advantage over [JSON](#json-rfc-8259) which uses [IETF Double Precision Floating Point](#floating-point) numbers, which can cause precision issues in transit.

## Date Interpretations

Dates can be greatly condensed using the Segmented Number base class.  Dates should be standardized as the year ten64 followed by a dot, and one ten64 character for the month and one ten64 character for the day.  For example;

 ```
   #Vv.21;  expands to 2023-02-01
 ```

## Datetime Interpretations

Datetimes add the additional timezone, (military time) hour and minute to the date.  The timezone, (military time) hour and minute each only take one ten64 character and can be encoded as follows;

  ```
    #Vv.216jR;  expands to 2023-02-01 CST 7:53 PM
  ```

Note j is 19, in military time -12 = 7 PM.

## List Interpretations

Ten64 supports lists of number separated by commas, each number MAY include whitespace characters on either side of the number;

  ```
    #1,2,3,4;  expands to a number list of 1,2,3,4
    #1.2.3,4.5.6,7.8.9;  expands to a list of 3d points
  ```

## MiliTimestamp Interpretations

MiliTimestamps add a single ten64 character for seconds and separate milliseconds with an additional dot.

  ```
    #Vv.216jR1.2; expands to 2023-02-01 CST 7:53:01.2
    or with milliseconds expanded PM 2023-02-01 CST 7:53:01.002 PM
  ```

## NanoTimestamp Interpretations

NanoTimestamps add an additional dot, as the MiliSeconds can be multiple characters (with values 0-1000), and then multiple ten64 characters representing the additional (0-1,000,000,000) nanoseconds that are NOT tracked as milliseconds.

```
    #Vv.216jR1.2.7;  expands to 2023-02-01 CST 7:53:01.2.7 PM
    or with nanoseconds expanded 2023-02-01 CST 7:53:01.002000007 PM
```

## Point Interpretations

Ten64 can encode 2d, 3d, and Nd points as segmented numbers.

```
# A 3d decimal point
#-1.7,-5.2,-7.9;
```

## Time Interpretations

Time will use the time segments from the [Datetime](#datetimes), [MiliTimestpan](#militimestamp), and [NanoTimestamp](#nanotimestamp).

  ```
    # The Date Time
    #Vv.216jR;  expands to 2023-02-01 CST 7:53 PM
    # vs just the time part
    #6jR;  expands to CST 7:53 PM
  ```
Note j is 19, in military time -12 = 7 PM.

## Modern Western Numeral System

Arabic, Ghubari, and Vedic as well as many other numeral systems were considered for use in this RFC.  We finally settled down on modern the name <b>The Modern Western Numeral System</b>.  It appears that numeral systems have influenced each other over the ages, and we will likely continue carbon dating each glyph 0-9 for some time, as we have recently carbon dated the glyph '0'.  In addition, even if we do find an older carbon date of a particular glyph, It could take a considerable amount of time to determine if that carbon dating references a different culture than the main culture which recorded the glyph.

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

Modern Western Decimal Numbers Are simply numbers using the Modern Western Numeral System, which contain a decimal point.

# Commentary

To improve human readability, we replaced these characters with their respective characters. The following chart shows the history of this.

```
21: lower case 'l' →  '$'
44: upper case 'I' → '%' → '+'
50: upper case 'O' → '?' → ':' → '!'
```

2026-03-28 Replaced the % sign with the + sign to make [URI (URL)](#uri-rfc3986) escaping easier.  Replaced the question mark with the colon to make [URI (URL)](#uri-rfc3986) escaping easier, and then later on replaced it with the exclamation point to make this Ten64 more compatible with [DIDs](#decentralized-identifiers-dids).

Although Ten64 can encode and decode numbers of any size and precision, they are often not human-decipherable.  During the creation of this text, there was much discussion about JSON RFCs [4627](#json-rfc-4627), [7158](#json-rfc-7158), [7159](#json-rfc-7159), [8259](#json-rfc-8259), [JavaScript](#javascript-wikipedia) and [ECMA Script](#ecma-262) Numbers.  Scott believes that a separate document SHOULD be created to address the serialization/de-serialization (aka encoding/decoding) which uses text representing the [Modern Western Numeral System](#modern-western-numeral-system) specifically.  Scott suggests something like the following;

```
12 → int, long or ECMA BigInt, Language specifies.
12.78 → Java BigDecimal style numbers
f12.78 → 32 bit IEEE 754 single floating point decimal numbers
d12.78 → 64 bit IEEE 754 double floating point decimal numbers
```

Although the current state of the [JSON RFC 8259](#json-rfc-8259) specification is fairly clear, It has a muddied past, which has created confusion and varying interpretations (i.e. [GSON](#gson), [Jackson](#fasterxml-jackson) and others).  This starts with the usage of the term [JavaScript](#javascript-wikipedia) in the title, the J in [JSON](#json-rfc-8259).  It took some time for an actual [JavaScript-like specification](#javascript-wikipedia) to emerge as [ECMA Script](#ecma-262) which specifies [IEEE 754-2019 Floating Point Nubmers](#floating-point-ieee-754-2019).  These challenges and issues are not traceable to a single standard, but instead are often the result of the interaction between three standards or more bodies the [IEEE](#ieee), [IETF](#ietf) and [ECMA International](#ecma-262).

As a side note, the [ECMA Script 262 website](https://tc39.es/ecma262) chews up enough resources (processor/RAM I didn't benchmark it?) that it slows down and crashes browsers on my computer with 64 GB of RAM.  However, for the brave people who want to click on these direct links;

- [ECMA Script 262 Section 6.1.6 Numeric Types](https://tc39.es/ecma262/#sec-numeric-types)
- [ECMA Script 262 Section 6.1.6.1 Language Types Number Type](https://tc39.es/ecma262/#sec-ecmascript-language-types-number-type)
- [ECMA Script 262 Section 21 Numbers and Dates](https://tc39.es/ecma262/#sec-numbers-and-dates)

In some ways, this issue appears to be fixed in part by more modern RFC's including the following;
- [CBOR RFC 8949](#cbor-rfc-8949) Which simply uses a binary format to transfer the [floating-point numbers](#floating-point-ieee-754-2019).

- [HTTP Structured Fields RFC 9651](#http-structured-fields-rfc-9651) Which does NOT target text but [HTTP or really UIRs RFC 3986](#uri-rfc3986) and puts a tight limitation on decimal numbers, only allowing three digits, which isn't compatible with [Bitcoin](#bitcoin) and other large decimal number formats.

The culmination of all of these points result in ubiquitious usage of string wrappers for numbers in tools like JSON, which forces the various parsers to get it right all the time;

```
{ "myJSONDecimalNumber": "12.3" }
```

This essentially defeats the purpose of having a number type in [JSON](#json-rfc-8259).

In addition, printing floating point numbers has been challenging historically.  Which has led to the [Ryū](#ryū) algorithm, which greatly reduced the time cost (asympotic complexity) of printing floating point numbers in various JVM environments.

# Citations

##### ASCII-7

https://www.ansi.org/ "American Standards Association. (1963). American Standard Code for Information Interchange (ASA X3.4-1963)."

##### Asymptotic Cost of Multiplication

- [https://en.wikipedia.org/wiki/Multiplication_algorithm#Computational_complexity_of_multiplication](https://en.wikipedia.org/wiki/Multiplication_algorithm#Computational_complexity_of_multiplication)
-
##### Base58

Nakamoto, S. (2009). "Base58 Encoding Scheme." Bitcoin Source Code. [https://github.com/bitcoin/bitcoin/blob/master/src/base58.cpp](https://github.com/bitcoin/bitcoin/blob/master/src/base58.cpp)

msporny, "The Base58 Encoding Scheme," IETF Draft (Individual Submission), February 2024. [https://datatracker.ietf.org/doc/html/draft-msporny-base58-03](https://datatracker.ietf.org/doc/html/draft-msporny-base58-03)

##### Base64 RFC 4648

Josefsson, S., "The Base16, Base32, and Base64 Data Encodings," RFC 4648, October 2006, <https://datatracker.ietf.org/doc/html/rfc4648>.

##### BigInt Java

https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/math/BigInteger.html
    "Java Platform, Standard Edition v21: Class BigInteger",
    Oracle Corporation, 2023.

##### BigDecimal Java

https://docs.oracle.com/en/java/javase/23/docs/api/java.base/java/math/BigDecimal.html
    "Java Platform, Standard Edition v21: Class BigDecimal",
    Oracle Corporation, 2023.

##### BigDecimal NPM

 https://www.npmjs.com/package/bigdecimal
    "bigdecimal: Arbitrary-precision decimal arithmetic",
    STZ-IDA, September 2024.

##### Bioctal

https://en.wikipedia.org/wiki/Bioctal "Community. (n.d.). Bioctal: Hexadecimal 2.0. Wikipedia."

##### BitSlotMaps 1.3.6.1.4.1.33097.1.1.3

https://adligo.github.io/papers.adligo.com/data_structures/BitSlotMaps.html "Morgan, S. (2025). BitSlotMaps. Adligo Papers."

##### Bitcoin
Nakamoto, S. (2008). *Bitcoin: A Peer-to-Peer Electronic Cash System*. [https://bitcoin.org/bitcoin.pdf](https://bitcoin.org/bitcoin.pdf)

##### CACM-Binary

https://doi.org/10.1145/364096.364107 "ACM. (1968). Letters to the editor: On binary notation. Communications of the ACM."

##### CBOR RFC 8949

Bormann, C. and P. Hoffman, "Concise Binary Object Representation (CBOR)", RFC 8949, STD 94, DOI 10.17487/RFC8949, December 2020, https://www.rfc-editor.org/rfc/rfc8949.

### Decentralized Identifiers (DIDs)

"Decentralized Identifiers (DIDs) v1.0: Core Architecture, Data Model, and Representations." Edited by Manu Sporny, Drummond Reed, Markus Sabadello, Drummond Reed, and Dave Longley. 19 July 2022. W3C Recommendation. [https://www.w3.org/TR/did-core/](https://www.w3.org/TR/did-core/).

##### DOID-Repo

https://github.com/adligo/doid.adligo.org
    "doid.adligo.org: Domain Oracle Identifiers",
    Adligo, October 2024.

##### ECMA 262

https://tc39.es/ecma262/
"ECMAScript 2025 Language Specification",
Ecma International, June 2025.

##### EJCN (Extensible JSON Classification Notation)

https://github.com/adligo/ejcn.adligo.org
"ejcn.adligo.org: Extensible JSON Classification Notation",
Adligo, October 2024.

##### EJCN (Extensible JSON Classification Notation) Schemas

"Extensible JSON Classification Notation Schemas", GitHub Repository, 2024, <https://github.com/adligo/ejcn_schemas.adligo.org>.

##### Endian Wikipedia
   Wikipedia contributors. (2024). Endianness. In Wikipedia, The Free Encyclopedia. Retrieved March 30, 2026, from https://en.wikipedia.org/wiki/Endianness


##### FasterXML Jackson

FasterXML, "Jackson: Main Portal page for the Jackson project," GitHub, <https://github.com/fasterxml/jackson>.

##### Floating-Point IEEE 754-2019

IEEE, "IEEE Standard for Floating-Point Arithmetic," IEEE 754-2019, July 2019, <https://ieeexplore.ieee.org/document/8766229>.

##### Floating-Point Gordon

Gordon College Department of Mathematics and Computer Science, "The IEEE 754 Floating-Point Standard," MAT342 Numerical Analysis Handouts,
[Gordon College: IEEE 754 Floating-Point Standard](https://cs.gordon.edu/courses/mat342/handouts/ieee.html)
(accessed March 2026).

##### Floating-Point J Burkardt

Burkardt, John, "IEEE Floating Point Numbers," Florida State University, Department of Scientific Computing,
[John Burkardt: IEEE Floating Point Numbers](https://people.sc.fsu.edu/~jburkardt/html/ieee.html)
(accessed October 2023).

##### Floating-Point Printing

Jonathan Richard Shewchuk. 1997. Adaptive Floating-Point Summation and Arbitrary Precision Floating-Point Arithmetic. Discrete & Computational Geometry 18, 3 (October 1997), 305–363.
[Shewchuk: Adaptive Floating-Point Summation](https://dl.acm.org/doi/10.1145/1806596.1806623)

##### Floating-Point Wikipedia

[Wikipedia: IEEE 754 (Standard for Floating-Point Arithmetic)](https://en.wikipedia.org/wiki/IEEE_754)

##### Grisu3

Loitsch, F. (2010). "Printing floating-point numbers quickly and accurately with integers." ACM SIGPLAN Notices, 45(6), 233–243. https://dl.acm.org/doi/10.1145/1806596.1806623

##### Gson

Google, "Gson: A Java serialization/deserialization library to convert Java Objects into JSON and back," GitHub, <https://github.com/google/gson>.

##### JavaScript Wikipedia

Wikipedia contributors. "JavaScript." Wikipedia, The Free Encyclopedia. [https://en.wikipedia.org/wiki/JavaScript](https://en.wikipedia.org/wiki/JavaScript). Accessed 29 March 2026.

##### JSON Schemas
Wright, A., Andrews, H., and Hutton, J., "JSON Schema: A Media Type for Describing JSON Data Structures", IETF Draft, November 2017, <https://json-schema.org/draft-07/json-schema-core.html>.

##### Hexadecimal

https://en.wikipedia.org/wiki/Hexadecimal "Wikipedia Contributors. (2026, March). Hexadecimal. Wikipedia."

##### HTTP Structured Fields RFC 9651

Nottingham, M. and P. Kamp, "Structured Field Values for HTTP", RFC 9651, DOI 10.17487/RFC9651, September 2024, https://www.rfc-editor.org/rfc/rfc9651.

##### IANA OIDs

IANA, "Private Enterprise Numbers (PEN)", March 2026,
<https://www.iana.org/assignments/enterprise-numbers>.

##### IEEE

IEEE. "Institute of Electrical and Electronics Engineers." Accessed March 30, 2026. https://www.ieee.org.

##### IETF

IETF. "Internet Engineering Task Force." Accessed March 30, 2026. https://www.ietf.org.

##### Information Theory Elements Cover Thomas 2006

Cover, T. M., & Thomas, J. A. (2006). Elements of Information Theory (2nd ed.). Wiley-Interscience. https://doi.org/10.1002/047174882X

##### JSON RFC 4627

Citation: Crockford, D., "The application/json Media Type for JavaScript Object Notation (JSON)", RFC 4627, DOI 10.17487/RFC4627, July 2006, <https://www.rfc-editor.org/info/rfc4627>.

##### JSON RFC 7158

Full Citation: Bray, T., Ed., "The JavaScript Object Notation (JSON) Data Interchange Format", RFC 7158, DOI 10.17487/RFC7158, March 2013, <https://www.rfc-editor.org/info/rfc7158>.

##### JSON RFC 7159

Full Citation: Bray, T., Ed., "The JavaScript Object Notation (JSON) Data Interchange Format", RFC 7159, DOI 10.17487/RFC7159, March 2014, <https://www.rfc-editor.org/info/rfc7159>.

##### JSON RFC 8259

Bray, T., "The JSON Data Interchange Format," RFC 8259, STD 90, December 2017, <https://www.rfc-editor.org/info/rfc8259>.

##### Math Asymptotic Processor Performance Wikipedia

Wikipedia contributors, "Computational complexity of mathematical operations," Wikipedia, The Free Encyclopedia, https://en.wikipedia.org/wiki/Computational_complexity_of_mathematical_operations (accessed October 2023).

##### Mathematical Theory of Communication Shannon 1948

Shannon, C. E. (1948). A Mathematical Theory of Communication. Bell System Technical Journal, 27(3), 379–423. https://doi.org/10.1002/j.1538-7305.1948.tb01338.x

##### Number Conversion Calculator

RapidTables, "Decimal to Binary Converter," RapidTables.com,
[RapidTables: Decimal to Binary Converter (Example: 756)](https://www.rapidtables.com/convert/number/decimal-to-binary.html?x=756)
(accessed March 2026).

##### Number Conversion Instructables

Instructables User 'mistic', "How to Convert From Decimal to Binary," Instructables,
[Instructables: How to Convert From Decimal to Binary](https://www.instructables.com/How-to-Convert-From-Decimal-to-Binary/)
(accessed October 2023).

##### Number Conversion Lumen

Lumen Learning, "Converting Between Bases," Mathematics for the Liberal Arts, https://courses.lumenlearning.com/waymakermath4libarts/chapter/converting-between-bases/ (accessed March 2026).

##### Number Conversion Khan Academy

Khan, Sal, "Large Number Decimal to Binary," Khan Academy, https://www.khanacademy.org/math/algebra-home/alg-intro-to-algebra/algebra-alternate-number-bases/v/large-number-decimal-to-binary (accessed March 2026).

##### Number Conversion WikiHow

WikiHow Staff, "How to Convert from Decimal to Binary," WikiHow, September 14, 2023,
[WikiHow: How to Convert from Decimal to Binary](https://www.wikihow.com/Convert-from-Decimal-to-Binary)
(accessed March 29, 2026).

##### Octet

https://en.wikipedia.org/wiki/Octet_(computing) "Wikipedia Contributors. (2026, March). Octet. Wikipedia."

##### OData Protocol

OASIS. "OData Version 4.01. Part 1: Protocol." Edited by Michael Pizzo, Ralf Handl, and Martin Zurmuehl. 22 June 2021. OASIS Standard. [https://docs.oasis-open.org/odata/odata/v4.01/os/part1-protocol/odata-v4.01-os-part1-protocol.html](https://docs.oasis-open.org/odata/odata/v4.01/os/part1-protocol/odata-v4.01-os-part1-protocol.html).

##### Origin of Modern Mathematical Numeral

Musa, A., "Origin of Modern Mathematical Numeral – 0, 1, 2, 3, 4, 5, 6, 7, 8, 9: the Hindu-Indian-Brahmagubta, The Islamo-Arabic or the West?", Mubi North Education Authority, Adamawa State – Nigeria, p. 46.
https://www.academia.edu/9099310/Origin_of_Modern_Mathematical_Numeral_0_1_2_3_4_5_6_7_8_9_the_Hindu_Indian_Brahmagubta_The_Islamo_Arabic_or_the_West

##### Positional Number Systems Wikipedia

Wikipedia contributors. (2024). Numeral system: Positional systems in detail. In Wikipedia, The Free Encyclopedia. Retrieved March 30, 2026, from https://en.wikipedia.org/wiki/Numeral_system#Positional_systems_in_detail

##### Radix 10 vs 60 Research Gate

ResearchGate. (2014, May). Why we use base-10 almost everywhere than base-60 which was first invented method? [Online Discussion Group]. https://www.researchgate.net/post/Why-we-use-base-10-almost-everywhere-than-base-60-which-was-first-invented-method

##### Radix Wikipedia

Wikipedia contributors. (2024). Radix. In Wikipedia, The Free Encyclopedia. Retrieved March 30, 2026, from https://en.wikipedia.org/wiki/Radix

##### Ryū

Adams, U. (2018). "Ryū: fast float-to-string conversion." Proceedings of the 39th ACM SIGPLAN Conference on Programming Language Design and Implementation (PLDI), 270–282. https://doi.org/10.1145/3192366.3192369

##### Sextet

https://en.wikipedia.org/wiki/Units_of_information "Wikipedia Contributors. (2026, March). Sextet. Wikipedia."

##### Ten64 Decimal Serialization Algorithm

Adligo Project, "Ten64DecimalSerialization," Adligo Concrete Algorithms, https://adligo.github.io/papers.adligo.com/algorithms/concrete/Ten64DecimalSerialization.html (accessed March 2026).

##### Ten64 Integer Serialization Algorithm

Adligo Project, "Ten64IntegerSerialization," Adligo Concrete Algorithms, https://adligo.github.io/papers.adligo.com/algorithms/concrete/Ten64IntegerSerialization.html (accessed March 2026).

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

##### IETF Language and Style Guide

Internet Engineering Task Force (IETF), "Language and Style," IETF Author Resources, https://authors.ietf.org/language-and-style (accessed March 2026).

##### XML Schemas

Peterson, D., Gao, S., Malhotra, A., Sperberg-McQueen, C. M., and Thompson, H. S., Eds., "W3C XML Schema Definition Language (XSD) 1.1 Part 2: Datatypes", W3C Recommendation, 5 April 2012, <https://www.w3.org/TR/xmlschema11-2/>.
