# Header & Trailer

### Standard Header

<table><thead><tr><th width="102">Tag</th><th width="165">Name</th><th width="95">Required</th><th>Description</th></tr></thead><tbody><tr><td>8</td><td>BeginString</td><td>Y</td><td><strong>FIXT.1.1</strong></td></tr><tr><td>9</td><td>BodyLength</td><td>Y</td><td>Message body length</td></tr><tr><td>35</td><td>MsgType</td><td>Y</td><td>Message type</td></tr><tr><td>49</td><td>SenderCompID</td><td>Y</td><td>CompID of the party sending the message</td></tr><tr><td>56</td><td>TargetCompID</td><td>Y</td><td>CompID of the party the message is sent to</td></tr><tr><td>34</td><td>MsgSeqNum</td><td>Y</td><td>Sequence number of this message</td></tr><tr><td>43</td><td>PossDupFlag</td><td>N</td><td><p>Indicates possible retransmission of message with this sequence number.</p><p>N = Original transmission<br>Y = Possible duplicate</p></td></tr><tr><td>52</td><td>SendingTime</td><td>Y</td><td><p>Time that message was sent in microseconds in GMT</p><p>Format: YYYYmmDD-hh:mm:ss.uuuuuu</p></td></tr><tr><td>122</td><td>OrigSendingTime</td><td>N</td><td><p>Required if PossDupFlag (43) = Y<br>Time the message was originally transmitted in microseconds in GMT</p><p>Format: YYYYmmDD-hh:mm:ss.uuuuuu</p></td></tr></tbody></table>

### Standard Trailer

<table><thead><tr><th width="124">Tag</th><th width="140">Name</th><th width="139">Required</th><th>Description</th></tr></thead><tbody><tr><td>10</td><td>CheckSum</td><td>Y</td><td>FIX checksum calculated</td></tr></tbody></table>
