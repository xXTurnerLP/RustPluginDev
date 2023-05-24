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

### After the header
###### UnityFS format (info from [3rd party lib](https://github.com/HearthSim/UnityPack))
```
if LZ4HC compression (from flags):
    next CompressedBlocks bytes of LZ4HC compressed data:
        first 16 bytes = guid
        next int32 = number of blocks:
            first int32 of the block = uncompressed size
            next int32 of the block = compressed size
            next int16 of the block = block flags
        
        next int32 = number of nodes (also called paths here: http://wiki.xentax.com/index.php/Unity_Unity3d_UnityFS):
            first int64 of the node = offset
            next int64 of the node = size
            next int32 of the node = status
            next zstring of the node = name
```

###### UnityRaw format  (info from [3rd party lib](https://github.com/HearthSim/UnityPack))
|Offset|Type|Description|
|:-:|:-:|:-:|
|0|uint32|File size|
|4|int32|Header size|
|8|int32|File count|
|12|int32|Bundle count|
|16|uint32|Bundle size (if header version is >= 2)|
|20|uint32|Uncompressed bundle size (if header version is >= 3)|
|24|uint32|Compressed file size (if (this) header size (offset=4) is >= 60)|
|28|uint32|Asset header size (if (this) header size (offset=4) is >= 60)|
|32|int32|*unknown*|
|36|uint8|*unknown*|
|37|zstring|Name|

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
|0-5|0x3F|Compression type. 0 on all bits means no compresion|
|7|0x80|Determines the BlockInfo offset.<br>If 0 its `ID:5 - ID:6`<br>If 1 its `sizeof ID:4 + sizeof ID:3 + 26`|
|8|0x100|Determines how much offset to add to BlockInfo offset, related to bit 8.<br>See notes (1)<br>The offset is `sizeof ID:4 + sizeof ID:3 + 26` (related to bit 8)<br>If 1 it adds `10`<br>If 0 it adds `sizeof ID:1 + 1`

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