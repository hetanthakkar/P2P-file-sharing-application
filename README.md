### P2P File Sharing

A Java-based Peer-to-Peer (P2P) file sharing application similar to BitTorrent.

#### Handshake Message

The handshake message consists of three parts: handshake header, zero bits, and peer ID. The total length of the handshake message is 32 bytes.

- **Handshake Header:** An 18-byte string 'P2PFILESHARINGPROJ'.
- **Zero Bits:** 10 bytes of zero bits.
- **Peer ID:** A 4-byte integer representing the peer ID.

#### Actual Messages

After the initial handshake, each peer can send a stream of actual messages. An actual message consists of the following components:

- **Message Length:** A 4-byte field specifying the length of the message in bytes, excluding the length field itself.
- **Message Type:** A 1-byte field specifying the type of the message.
- **Message Payload:** Variable-sized payload data, depending on the message type.

#### Message Types

The following message types are supported:

- **choke, unchoke, interested, not interested:** These messages have no payload.
- **have:** This message has a payload containing a 4-byte piece index field.
- **bitfield:** This message is sent as the first message after the handshake is completed when a connection is established. It has a bitfield as its payload, where each bit represents whether the peer has the corresponding piece or not. The first byte of the bitfield corresponds to piece indices 0-7 from high bit to low bit, respectively. The next byte corresponds to piece indices 8-15, and so on. Spare bits at the end are set to zero. Peers that don't have any pieces yet may skip sending a 'bitfield' message.
- **request:** This message has a payload consisting of a 4-byte piece index field. Note that this payload is different from that of BitTorrent, as we don't divide a piece into smaller subpieces.
- **piece:** This message has a payload consisting of a 4-byte piece index field and the content of the piece.
