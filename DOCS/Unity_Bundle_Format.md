[[Back]](../README.md)

### Types
|Type|Size|Description|
|:-:|:-:|:-:|
|zstring|-|Zero terminated UTF-8 string|
|uint8|1 byte|Unsigned|
|uint16|2 bytes|Unsigned|
|uint32|4 bytes|Unsigned|
|uint64|8 bytes|Unsigned|
|int8|1 byte|Signed|
|int16|2 bytes|Signed|
|int32|4 bytes|Signed|
|int64|8 bytes|Signed|

### Header Format
Its big endian
|Reference ID|Offset|Type|Description|Flags|
|:-:|:-:|:-:|:-:|:-:|
|1|0|zstring|The bundle signature. It can be one of: `UnityFS`, `UnityWeb`, `UnityRaw` and `UnityArchive`|-|
|2|-|uint32|Version (usually 6). If version is 6 it seems the signature sets to `UnityFS` if it was `UnityWeb`|Header flag `0x100` is set if version is 6|
|3|+4|zstring|Unity WebBundle version. Usually 6 bytes and looks like: `5.x.x`|-|
|4|-|zstring|Unity player minimum version. Looks like: `2019.4.7f1`|-|
|5|-|uint64|File size (of the whole Bundle file)|-|
|6|+8|uint32|Size of CompressedBlocks Info|-|
|7|+12|uint32|Size of UncompressedBlocks Info|-|
|8|+16|uint32|Flags|-|

# TODO: BlockInfo not 100% correct, RE more
### BlockInfo Format
|Reference ID|Offset|Type|Description|
|:-:|:-:|:-:|:-:|
|1|0|16 bytes|Hash consisting of 16 bytes|
|2|16|uint32|Number of blocks|
|-|-|-|After this point there is blocks, in a repeating pattern|
|3|20|uint32|Uncompressed size of block 1 (if `ID:2 >= 1`)|
|4|24|uint32|Compressed size of block 1 (if `ID:2 >= 1`)|
|5|28|uint16|Flags of block 1 (if `ID:2 >= 1`)|
|6|30|uint32|Uncompressed size of block 2 (if `ID:2 >= 2`)|
|7|34|uint32|Compressed size of block 2 (if `ID:2 >= 2`)|
|8|38|uint16|Flags of block 2 (if `ID:2 >= 2`)|
|-|-|-|etc ...|

### Flags
|Bit|Mask|Description|
|:-:|:-:|:-:|
|1-6|0x3F|Compression type. 0 on all bits means no compresion|
|8|0x80|Determines the BlockInfo offset.<br>If 0 its `ID:5 - ID:6`<br>If 1 its `sizeof ID:4 + sizeof ID:3 + 26`|
|9|0x100|Determines how much offset to add to BlockInfo offset, related to bit 8.<br>See notes (1)<br>The offset is `sizeof ID:4 + sizeof ID:3 + 26` (related to bit 8)<br>If 1 it adds `10`<br>If 0 it adds `sizeof ID:1 + 1`

### Compression Type
|Bits|Decimal|Description|
|:-:|:-:|:-:|
|000|0|None|
|001|1|LZMA|
|010|2|LZ4|
|011|3|LZ4HC|
|100|4|LZHAM|

## Notes
1. Only bit 8 or bit 9 can be set, if both are set bit 8 is chosen as the active one, if neither is set bit 9 is chosen as the active one.

* sizeof in the flags **does NOT include** the zero terminator
* Version 6 header seems to be the newest format
* `ID:5 - ID:6` Means the field from the table at ID 5 minus the field from the table at ID 6