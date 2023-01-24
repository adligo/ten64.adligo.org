# ten64.adligo.org
This will house the Text Encoded Numbers as Base 64 bit project, with an accompanying RFC.   It will NOT use the regular Base64 alphabet but a virtual binary alphabet similar to hexidecimal 0-9,a-z,A-Z along with some additional special characters most notably '#','.' and ';'.  It is designed to be human and machine readable but is really designed to optimized the reading and writing of numbers streaming and storage computer systems.   It will use a big ending virtual binary system;

Special Characters;
#   Optional explicit end of #Ten64 binary stream / chunk
.   The Decimal / List Seperator
;   Optional explicit begining of #Ten64 binary stream / chunk

| Primary ASCII / UTF8 Character   |  Arabic Numeral Value |  Virtual Binary Sextet  | 
|----------------------------------|-----------------------|-------------------------|
| 0                                |  0                    | 000000                  |
| 1                                |  1                    | 100000                  |
