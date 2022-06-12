# Header & Trailer

### Standard Header

| Tag | Name            | Required | Description                                                                                                                                          |
| --- | --------------- | -------- | ---------------------------------------------------------------------------------------------------------------------------------------------------- |
| 8   | BeginString     | Y        | **FIXT.1.1**                                                                                                                                         |
| 9   | BodyLength      | Y        | Message body length                                                                                                                                  |
| 35  | MsgType         | Y        | Message type                                                                                                                                         |
| 49  | SenderCompID    | Y        | CompID of the party sending the message                                                                                                              |
| 56  | TargetCompID    | Y        | CompID of the party the message is sent to                                                                                                           |
| 34  | MsgSeqNum       | Y        | Sequence number of this message                                                                                                                      |
| 43  | PossDupFlag     | N        | <p>Indicates possible retransmission of message with this sequence number.</p><p>N = Original transmission<br>Y = Possible duplicate</p>             |
| 52  | SendingTime     | Y        | <p>Time that message was sent in microseconds in GMT</p><p>Format: YYYYmmDD-hh:mm:ss.uuuuuu</p>                                                      |
| 122 | OrigSendingTime | N        | <p>Required if PossDupFlag (43) = Y<br>Time the message was originally transmitted in microseconds in GMT</p><p>Format: YYYYmmDD-hh:mm:ss.uuuuuu</p> |

### Standard Trailer

| Tag | Name     | Required | Description             |
| --- | -------- | -------- | ----------------------- |
| 10  | CheckSum | Y        | FIX checksum calculated |
