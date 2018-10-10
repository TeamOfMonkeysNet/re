RTMP module
-----------

This module implements Real Time Messaging Protocol (RTMP) [1].


The following logical blocks are supported/planned:

- RTMP Handshake
- RTMP Header encoding and decoding
- RTMP Chunking and De-chunking
- RTMP Transport via TCP
- RTMP Client
- RTMP Server
- AMF (Action Message Format) Encoding/Decoding

- RTMPS (RTMP over TLS)
- RTMPE (RTMP over Adobe Encryption)
- RTMPT (RTMP over HTTP)
- RTMFP (RTMP over UDP)


TODO:
----

- [x] improve AMF encoding API
- [x] implement AMF transaction matching
- [x] add support for Data Message
- [x] add support for AMF Strict Array (type 10)
- [ ] add support for TLS encryption
- [ ] add support for extended timestamp




Protocol stack:
--------------

    .-------.  .-------.
    |  AMF  |  |  FLV  |
    '-------'  '-------'
        |          |
        +----------'
        |
    .-------.
    |  RTMP |
    '-------'
        |
        |
    .-------.
    |  TCP  |
    '-------'


Message Sequence:
----------------


```
Client                                      Server

|----------------- TCP Connect -------------->|
|                                             |
|                                             |
|                                             |
|<-------------- 3-way Handshake ------------>|
|                                             |
|                                             |
|                                             |
|----------- Command Message(connect) ------->| chunkid=3, streamid=0, tid=1
|                                             |
|<------- Window Acknowledgement Size --------| chunkid=2, streamid=0
|                                             |
|<----------- Set Peer Bandwidth -------------| chunkid=2, streamid=0
|                                             |
|-------- Window Acknowledgement Size ------->|
|                                             |
|<------ User Control Message(StreamBegin) ---| chunkid=2, streamid=0
|                                             |
|<------------ Command Message ---------------| chunkid=3, streamid=0, tid=1
|        (_result- connect response)          |
```


Interop:
-------

- Wowza Streaming Engine 4.7.1
- Youtube service
- FFmpeg's RTMP module




References:
----------

[1] http://wwwimages.adobe.com/www.adobe.com/content/dam/acom/en/devnet/rtmp/pdf/rtmp_specification_1.0.pdf

[2] https://wwwimages2.adobe.com/content/dam/acom/en/devnet/flv/video_file_format_spec_v10_1.pdf

[3] https://en.wikipedia.org/wiki/Action_Message_Format

[4] https://wwwimages2.adobe.com/content/dam/acom/en/devnet/pdf/amf0-file-format-specification.pdf
