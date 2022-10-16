[[Back]](../README.md)

1. Client initiates connection with `RakNet::NET_StartClient()`
1. Server sends PacketID `0x11 (17) (enum: Message.Type.RequestUserInformation)` to the client (Client should always listen for new data with `RakNet::NET_Receive()`)
1. todo