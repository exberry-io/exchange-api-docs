# Session Messages

### LOGON (_MsgType = A_)

| Tag  | Name             | Required | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                |
| ---- | ---------------- | -------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 98   | EncryptMethod    | Y        | 0 = NONE                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
| 108  | HeartBtInt       | Y        | Hearbeat interval in seconds                                                                                                                                                                                                                                                                                                                                                                                                                               |
| 553  | Username         | Y        | apiKey                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| 554  | Password         | Y        | <p><code>HMAC SHA256 signature</code> computed using provided <code>secret</code> key and message. (See how to compute below).</p><p><code>Message = "apiKey":$apiKey,"timestamp":$SendingTime(52)</code><br>Example:<br><code>Message = "apiKey":"1234567abcdz","timestamp":"20210625-15:47:07.473000"</code></p><p><code>secret = MySecretKey</code></p><p><code>signature = ef15c2600af634a83c3e5ada5f80478153fea0f684e640db2bb5edc91aa43a44</code></p> |
| 141  | ResetSeqNumFlag  | N        | <p>Does client and server should reset sequence numbers</p><p>Y= Reset MsgSeqNum to 1</p><p>N = Do not reset MsgSeqNum</p>                                                                                                                                                                                                                                                                                                                                 |
| 1137 | DefaultApplVerID | Y        | <p>Fix version</p><p>9 = FIX50SP2</p>                                                                                                                                                                                                                                                                                                                                                                                                                      |

How to compute the signature

```
$ echo -n '"apiKey":"1234567abcdz","timestamp":"20210625-15:47:07.473000"' | openssl dgst -sha256 -hmac 'MySecretKey'
(stdin)= ef15c2600af634a83c3e5ada5f80478153fea0f684e640db2bb5edc91aa43a44
```

### LOGOUT _(MsgType = 5)_

| Tag | Name | Required | Description |
| --- | ---- | -------- | ----------- |
| 58  | Text | N        | Free text   |

### Heartbeat (_MsgType = 0)_

| Tag | Name      | Required | Description                                                                                             |
| --- | --------- | -------- | ------------------------------------------------------------------------------------------------------- |
| 112 | TestReqID | N        | <p>Required when the heartbeat is the result of a Test Request message.</p><p>ID of the TestRequest</p> |

### TestRequest (_MsgType = 1)_

| Tag | Name      | Required | Description           |
| --- | --------- | -------- | --------------------- |
| 112 | TestReqID | Y        | ID of the TestRequest |

### ResendRequest (_MsgType = 2)_

| Tag | Name       | Required | Description                                                                                                                                                                                                                                                                         |
| --- | ---------- | -------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 7   | BeginSeqNo | Y        | Sequence number of first message in range to be resent                                                                                                                                                                                                                              |
| 16  | EndSeqNo   | Y        | <p>Sequence number of last message in range to be resent.</p><p>If the request is for partial resends it will contain the last missed message sequence number.</p><p>If request is for all messages subsequent to a particular message, EndSeqNo = "0" (representing infinity).</p> |

### SequenceReset (_MsgType = 4)_

| Tag | Name        | Required | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| --- | ----------- | -------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 123 | GapFillFlag | N        | <p>Indicates that the Sequence Reset message is replacing administrative or application messages which will not be resent.</p><p>N = Sequence Reset - Ignore MsgSeqNum (34)</p><p>Y = Gap Fill Message - MsgSeqNum (34) is valid</p><p>Exberry sends out only Y as part of the ResendRequest flow.</p><p>In case client application will send message with N, Exberry will ignore the MsgSeqNum(34) and apply the NewSeqNo(36) as the new inbound sequence</p> |
| 36  | NewSeqNo    | Y        | Sequence number of the next message to be transmitted.                                                                                                                                                                                                                                                                                                                                                                                                         |

### Reject (_MsgType = 3)_

| Tag | Name                | Required | Description                                                                                            |
| --- | ------------------- | -------- | ------------------------------------------------------------------------------------------------------ |
| 98  | RefSeqNum           | _Y_      | MsgSeqNum of rejected message                                                                          |
| 58  | Text                | N        | Exberry error message                                                                                  |
| 373 | SessionRejectReason | N        | Error code, per [FIX specifications](https://fiximate.fixtrading.org/en/FIX.Latest\_EP266/tag373.html) |
| 371 | RefTagID            | N        | Reference TadId caused the rejection                                                                   |
| 372 | RefMsgType          | N        | Reference MsgType of rejected message                                                                  |
