[[Home]](../../README.md) <br>
[[Back]](./Index.md)

# About
RakNet is the C++ library Facepunch uses for network communication to/from the client/server. However the RakNet version used in the game (client) as well in the server is heavily modified since RakNet is open-source. <br>
RakNet is the "legacy" method of networking, the newer method is SteamNetworking but as of writing this, that method is rarely used, its only used if you enable it manually with a launch option.

# Overview
## Exported DLL functions
* `NETRCV_Address`
* `NETRCV_GUID`
* `NETRCV_LengthBits`
* `NETRCV_Port`
* `NETRCV_RawData`
* `NETRCV_ReadBytes`
* `NETRCV_SetReadPointer`
* `NETRCV_UnreadBits`
* `NETSND_Broadcast`
* `NETSND_ReadCompressedFloat`
* `NETSND_ReadCompressedInt32`
* `NETSND_ReadCompressedInt64`
* `NETSND_Send`
* `NETSND_Size`
* `NETSND_Start`
* `NETSND_WriteBytes`
* `NETSND_WriteCompressedFloat`
* `NETSND_WriteCompressedInt32`
* `NETSND_WriteCompressedInt64`
* `NET_Close`
* `NET_CloseConnection`
* `NET_Create`
* `NET_GetAddress`
* `NET_GetAveragePing`
* `NET_GetGUID`
* `NET_GetLastPing`
* `NET_GetLowestPing`
* `NET_GetReceiveBufferSize`
* `NET_GetStatistics`
* `NET_GetStatisticsString`
* `NET_LastStartupError`
* `NET_LimitBandwidth`
* `NET_Receive`
* `NET_SendMessage`
* `NET_StartClient`
* `NET_StartServer`

# Function definitions
## NETRCV_Address
**Parameters:** `void* net` <br>
**Returns:** `uint32_t` <br>
<br>
**Description:** ...

## NETRCV_GUID
**Parameters:** `void* net` <br>
**Returns:** `uint64_t` <br>
<br>
**Description:** ...

## NETRCV_LengthBits
**Parameters:** `void* net` <br>
**Returns:** `int` <br>
<br>
**Description:** ...

## NETRCV_Port
**Parameters:** `void* net` <br>
**Returns:** `uint32_t` <br>
<br>
**Description:** ...

## NETRCV_RawData
**Parameters:** `void* net` <br>
**Returns:** `void*` <br>
<br>
**Description:** ...

## NETRCV_ReadBytes
**Parameters:** `void* net, void* data, int length` <br>
**Returns:** `bool` <br>
<br>
**Description:** ...

## NETRCV_SetReadPointer
**Parameters:** `void* net, int bitsOffset` <br>
**Returns:** `void` <br>
<br>
**Description:** ...

## NETRCV_UnreadBits
**Parameters:** `void* net` <br>
**Returns:** `int` <br>
<br>
**Description:** ...

## NETSND_Broadcast
**Parameters:** `void* net, int priority, int reliability, int channel` <br>
**Returns:** `uint32_t` <br>
<br>
**Description:** ...

## NETSND_ReadCompressedFloat
**Parameters:** `void* net` <br>
**Returns:** `float` <br>
<br>
**Description:** ...

## NETSND_ReadCompressedInt32
**Parameters:** `?` <br>
**Returns:** `?` <br>
<br>
**Description:** ...

## NETSND_ReadCompressedInt64
**Parameters:** `?` <br>
**Returns:** `?` <br>
<br>
**Description:** ...

## NETSND_Send
**Parameters:** `void* net, uint64_t connectionID, int priority, int reliability, int channel` <br>
**Returns:** `uint32_t` <br>
<br>
**Description:** ...

## NETSND_Size
**Parameters:** `void* net` <br>
**Returns:** `uint32_t` <br>
<br>
**Description:** ...

## NETSND_Start
**Parameters:** `void* net` <br>
**Returns:** `void` <br>
<br>
**Description:** ...

## NETSND_WriteBytes
**Parameters:** `void* net, void* data, int length` <br>
**Returns:** `void` <br>
<br>
**Description:** ...

## NETSND_WriteCompressed
**Parameters:** `?` <br>
**Returns:** `?` <br>
<br>
**Description:** ...

## NETSND_WriteCompressed
**Parameters:** `?` <br>
**Returns:** `?` <br>
<br>
**Description:** ...

## NETSND_WriteCompressed
**Parameters:** `?` <br>
**Returns:** `?` <br>
<br>
**Description:** ...

## NET_Close
**Parameters:** `void* net` <br>
**Returns:** `void` <br>
<br>
**Description:** ...

## NET_CloseConnection
**Parameters:** `void* net, uint64_t connectionID` <br>
**Returns:** `void` <br>
<br>
**Description:** ...

## NET_Create
**Parameters:** `none` <br>
**Returns:** `void*` - Ptr to raknetstruct <br>
<br>
**Description:** Creates heap allocated block of 0x250 bytes. Some values are initialized to 0, 2048 and 1. <br>
Image of how the structure looks in ReClass (name of fields are what they are initialized to) <br>
![img](../../Resources/Images/RakNetReversal/raknetstruct_init.png)

## NET_GetAddress
**Parameters:** `void* net, uint64_t connectionID` <br>
**Returns:** `void*` <br>
<br>
**Description:** ...

## NET_GetAveragePing
**Parameters:** `void* net, uint64_t connectionID` <br>
**Returns:** `int` <br>
<br>
**Description:** ...

## NET_GetGUID
**Parameters:** `?` <br>
**Returns:** `?` <br>
<br>
**Description:** ...

## NET_GetLastPing
**Parameters:** `void* net, uint64_t connectionID` <br>
**Returns:** `int` <br>
<br>
**Description:** ...

## NET_GetLowestPing
**Parameters:** `void* net, uint64_t connectionID` <br>
**Returns:** `int` <br>
<br>
**Description:** ...

## NET_GetReceiveBufferSize
**Parameters:** `void* net` <br>
**Returns:** `uint32_t` <br>
<br>
**Description:** ...

## NET_GetStatistics
**Parameters:** `void* net, uint64_t connectionID, RaknetStats* data, int dataLength` <br>
**Returns:** `bool` <br>
<br>
**Description:** ...

## NET_GetStatisticsString
**Parameters:** `void* net, uint64_t connectionID` <br>
**Returns:** `void*` <br>
<br>
**Description:** ...

## NET_LastStartupError
**Parameters:** `void* net` <br>
**Returns:** `void*` <br>
<br>
**Description:** ...

## NET_LimitBandwidth
**Parameters:** `?` <br>
**Returns:** `?` <br>
<br>
**Description:** ...

## NET_Receive
**Parameters:** `void* net` <br>
**Returns:** `bool` <br>
<br>
**Description:** ...

## NET_SendMessage
**Parameters:** `void* net, void* data, int length, uint32_t adr, uint16_t port` <br>
**Returns:** `void` <br>
<br>
**Description:** ...

## NET_StartClient
**Parameters:** `void* net, char* hostName, int port, int retries, int retryDelay, int timeout` <br>
**Returns:** `int` <br>
<br>
**Description:** ...

## NET_StartServer
**Parameters:** `void* net, char* ip, int port, int maxConnections` <br>
**Returns:** `int` <br>
<br>
**Description:** NOTE: If IP is empty string it is considered to be 0.0.0.0
