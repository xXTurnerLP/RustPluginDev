# Demo file format (for raknet)

## When a demo file is saved to disk the following happens
1. The first thing written to the file is the size & the string `RUST DEMO FORMAT`, the string length is 1 byte that says the amount of bytes the string is, its always before the string. This should be `0x10` or `16` because the string is 16 letters.

## Format (demo file)
* First byte is length of header string length
* Next N bytes are the header string itself (ansi)
* Next 4 bytes are the length of the actual header
* Next N bytes are the actual header itself
* At the end there is a 0 byte (`0x00`). This indicates the end of header section
* After the end of the header, begins the **RAW PACKETS**

<hr>

## Format (packet)
#### **Note:** There is 2 types of packets: OnPlayerTick Packets & Event packets
### **OnPlayerTick packet**
* Length of protobuf + 1 (int)
* Total milliseconds since the start of the recording (long)
* Packet ID + 140 (byte)
* Protobuf
* 0 byte (0x00)
* 0 byte (0x00)
### **Event packet**
* Length of stream (int)
* Total milliseconds since the start of the recording (long)
* Stream data
* 0 byte (0x00)
* 0 byte (0x00)

<hr>

## Format (header)
##### **Note:** When multiple bytes are present that is custom format, not sequental byte after byte
* First byte is `0x10 (16)`
* Next 16 bytes are the string `RUST DEMO FORMAT`
* Next 4 bytes are the length of the actual header
* Next N bytes are the actual header itself
* * Starting with the first byte: `0x8 (8)`
* * Next byte is demo format version: `0x3 (3)`
* * Next byte is `0x12 (18)`
* * Next byte is size of level name (`0xE (14) for "Procedural Map"`)
* * Next N (`0xE 14`) bytes are the name of the level: `Procedural Map`
* * Next byte is `0x18 (24)`
* * Next N (at most 4) bytes are the seed
* * Next byte is `0x20 (32)`
* * Next N (at most 4) bytes are the level size
* * Next byte is `0x2A (42)`
* * Next byte is the string length of world checksum
* * Next N bytes is the string of world checksum
* * Next byte is `0x30 (48)`
* * Next N (at most 8) bytes are the user steam64id
* * Next byte is `0x3A (58)`
* * Next N (at most 4) bytes are the size of structure with eye position of the player
* * * Next byte is `0xD (13)`
* * * Next 4 bytes are X coordinate of the eyes
* * * Next byte is `0x15 (21)`
* * * Next 4 bytes are Y coordinate of the eyes
* * * Next byte is `0x1D (29)`
* * * Next 4 bytes are Z coordinate of the eyes
* * Next byte is `0x42 (66)`
* * Next N (at most 4) bytes are the size of structure with eye rotation of the player (head forward)
* * * Next byte is `0xD (13)`
* * * Next 4 bytes are X coordinate of the eyes
* * * Next byte is `0x15 (21)`
* * * Next 4 bytes are Y coordinate of the eyes
* * * Next byte is `0x1D (29)`
* * * Next 4 bytes are Z coordinate of the eyes
* * Next byte is `0x4A (74)`
* * Size of string
* * string levelurl
* * Next byte is `0x50 (80)`
* * N bytes (at most 8) DateTime time of the start time of the recording
* * Next byte is `0x58 (88)`
* * N bytes (at most 8) milliseconds, how long is the demo recording