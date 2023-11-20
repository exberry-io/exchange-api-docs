# Session Messages

### LOGON (_MsgType = A_)

{% code overflow="wrap" %}
```
8=FIXT.1.1|9=192|35=A|49=Target|56=EXBERRY|34=1|52=20230202-13:04:03.924000|98=0|108=30|141=Y|553=03189d54-ef96-4b21-909f-aaf746ba97c0|554=6342f2c079f30e8212345b5a7662f4ef6a45695186edc7e06d6cf9b123456789d6|1137=9|10=235|
8=FIXT.1.1|9=82|35=A|49=EXBERRY|56=Target|34=1|52=20230202-13:04:04.359807|98=0|108=30|141=Y|1137=9|10=033|
```
{% endcode %}

<table><thead><tr><th width="108">Tag</th><th width="164">Name</th><th width="114">Required</th><th>Description</th></tr></thead><tbody><tr><td>98</td><td>EncryptMethod</td><td>Y</td><td><p>Method of encryption</p><p>0 = None / Other </p><p>99 = Custom </p></td></tr><tr><td>108</td><td>HeartBtInt</td><td>Y</td><td>Hearbeat interval in seconds</td></tr><tr><td>553</td><td>Username</td><td>Y</td><td>apiKey</td></tr><tr><td>554</td><td>Password</td><td>Y</td><td>When EncryptMethod (98) = 0 : secret key <br>When EncryptMethod (98) = 99 : digital signature (see below)</td></tr><tr><td>141</td><td>ResetSeqNumFlag</td><td>N</td><td><p>Does client and server should reset sequence numbers</p><p>Y= Reset MsgSeqNum to 1</p><p>N = Do not reset MsgSeqNum</p></td></tr><tr><td>1137</td><td>DefaultApplVerID</td><td>Y</td><td><p>Fix version</p><p>9 = FIX50SP2</p></td></tr><tr><td><mark style="color:blue;">(NEW v1.21.0)</mark> <br>20037</td><td>Custom field</td><td>N</td><td><p>Y = sending in the ER the information about the legs and the parent trades</p><p>N = sending in the ER the information only about the parent trades <br>If not sent, system will use N as default</p></td></tr></tbody></table>

How to compute the signature\
HMAC SHA256 signature computed using provided secret key and message.\
\
Example:

```
Message = "apiKey":$apiKey,"timestamp":$SendingTime(52)
Message = "apiKey":"1234567abcdz","timestamp":"20210625-15:47:07.473000"
secret = MySecretKey
signature = ef15c2600af634a83c3e5ada5f80478153fea0f684e640db2bb5edc91aa43a44
```

```
$ echo -n '"apiKey":"1234567abcdz","timestamp":"20210625-15:47:07.473000"' | openssl dgst -sha256 -hmac 'MySecretKey'
(stdin)= ef15c2600af634a83c3e5ada5f80478153fea0f684e640db2bb5edc91aa43a44
```

### LOGOUT _(MsgType = 5)_

{% code overflow="wrap" %}
```
8=FIXT.1.1|9=98|35=5|49=Target|56=EXBERRY|34=86|52=20230202-13:46:00.588000|58=Exit program. Message: SIGINT|789=92|10=176|
8=FIXT.1.1|9=85|35=5|49=EXBERRY|56=Target|34=92|52=20230202-13:46:00.553453|58=Client initiated logout|10=099|
```
{% endcode %}

<table><thead><tr><th width="200">Tag</th><th>Name</th><th>Required</th><th>Description</th></tr></thead><tbody><tr><td>58</td><td>Text</td><td>N</td><td>Free text</td></tr></tbody></table>

### Heartbeat (_MsgType = 0)_

{% code overflow="wrap" %}
```
fixin 8=FIXT.1.1|9=57|35=0|49=EXBERRY|56=Target|34=7|52=20230202-13:05:20.655227|10=137|
```
{% endcode %}

<table><thead><tr><th width="94">Tag</th><th width="126">Name</th><th width="118">Required</th><th>Description</th></tr></thead><tbody><tr><td>112</td><td>TestReqID</td><td>N</td><td><p>Required when the heartbeat is the result of a Test Request message.</p><p>ID of the TestRequest</p></td></tr></tbody></table>

### TestRequest (_MsgType = 1)_

{% code overflow="wrap" %}
```
8=FIXT.1.1|9=66|35=1|49=Target|56=EXBERRY|34=223|52=20230130-13:16:29.697000|112=19|10=044|
```
{% endcode %}

<table><thead><tr><th width="119">Tag</th><th width="136">Name</th><th width="137">Required</th><th>Description</th></tr></thead><tbody><tr><td>112</td><td>TestReqID</td><td>Y</td><td>ID of the TestRequest</td></tr></tbody></table>

### ResendRequest (_MsgType = 2)_

{% code overflow="wrap" %}
```
 8=FIXT.1.1|9=72|35=2|49=EXBERRY|56=Target|34=20906|52=20230130-10:57:31.965768|7=444|16=0|10=076|
```
{% endcode %}

<table><thead><tr><th width="78">Tag</th><th width="134">Name</th><th width="103">Required</th><th>Description</th></tr></thead><tbody><tr><td>7</td><td>BeginSeqNo</td><td>Y</td><td>Sequence number of first message in range to be resent</td></tr><tr><td>16</td><td>EndSeqNo</td><td>Y</td><td><p>Sequence number of last message in range to be resent.</p><p>If the request is for partial resends it will contain the last missed message sequence number.</p><p>If request is for all messages subsequent to a particular message, EndSeqNo = "0" (representing infinity).</p></td></tr></tbody></table>

### SequenceReset (_MsgType = 4)_

{% code overflow="wrap" %}
```
 8=FIXT.1.1|9=72|35=4|49=Target|56=EXBERRY|34=448|52=20230130-10:57:31.963000|36=445|123=Y|10=097|
```
{% endcode %}

<table><thead><tr><th width="79">Tag</th><th width="119">Name</th><th width="104">Required</th><th width="446">Description</th></tr></thead><tbody><tr><td>123</td><td>GapFillFlag</td><td>N</td><td><p>Indicates that the Sequence Reset message is replacing administrative or application messages which will not be resent.</p><p>N = Sequence Reset - Ignore MsgSeqNum (34)</p><p>Y = Gap Fill Message - MsgSeqNum (34) is valid</p><p>Exberry sends out only Y as part of the ResendRequest flow.</p><p>In case client application will send message with N, Exberry will ignore the MsgSeqNum(34) and apply the NewSeqNo(36) as the new inbound sequence</p></td></tr><tr><td>36</td><td>NewSeqNo</td><td>Y</td><td>Sequence number of the next message to be transmitted.</td></tr></tbody></table>

### Reject (_MsgType = 3)_

{% code overflow="wrap" %}
```
8=FIXT.1.1|9=82|35=3|49=EXBERRY|56=Target|34=2|52=20230202-14:04:51.618511|45=2|371=263|372=x|373=4|10=050|
```
{% endcode %}

<table><thead><tr><th width="101">Tag</th><th width="200">Name</th><th width="104">Required</th><th>Description</th></tr></thead><tbody><tr><td>98</td><td>RefSeqNum</td><td><em>Y</em></td><td>MsgSeqNum of rejected message</td></tr><tr><td>58</td><td>Text</td><td>N</td><td>Exberry error message</td></tr><tr><td>373</td><td>SessionRejectReason</td><td>N</td><td>Error code, per <a href="https://fiximate.fixtrading.org/en/FIX.Latest/cds373.html">FIX specifications</a></td></tr><tr><td>371</td><td>RefTagID</td><td>N</td><td>Reference TadId caused the rejection</td></tr><tr><td>372</td><td>RefMsgType</td><td>N</td><td>Reference MsgType of rejected message</td></tr></tbody></table>
