# Time In Force

Available "Time In Force" :

* **GTC** - Good Till Cancel (resting on book till cancellation)
* **GTD** - Good Till Date (at given time (expiryDate) order will be automatically cancelled).
* **FOK** - Fill Or Kill (all or nothing)
* **IOC** - Immediate Or Cancelled (allows partial fills)
* **GAA** - Good At Auction can be captured only during auctions and will expire after auction in case it was not filled .
* **DAY** - Order will be automatically cancelled when trading day is closed.&#x20;

Availability During Continuous Trading:

<table><thead><tr><th width="137">Type</th><th width="85" align="center">GTC</th><th width="86" align="center">GTD</th><th width="79" align="center">FOK</th><th width="78" align="center">IOC</th><th width="76" align="center">GAA</th><th>DAY</th></tr></thead><tbody><tr><td>Limit</td><td align="center">✔️</td><td align="center">✔️</td><td align="center">✔️</td><td align="center">✔️</td><td align="center">❌</td><td>✔️</td></tr><tr><td>Market</td><td align="center">❌</td><td align="center">❌</td><td align="center">✔️</td><td align="center">✔️</td><td align="center">❌</td><td>❌</td></tr><tr><td>Stop</td><td align="center">❌</td><td align="center">❌</td><td align="center">✔️</td><td align="center">✔️</td><td align="center">❌</td><td>❌</td></tr><tr><td>StopLimit</td><td align="center">✔️</td><td align="center">✔️</td><td align="center">✔️</td><td align="center">✔️</td><td align="center">❌</td><td>✔️</td></tr></tbody></table>

<mark style="color:blue;">NEW v1.33.0</mark> Availability During Auction:

Note: IOC is available for market and limit orders,  Limit IOC is identical to Limit GAA (both are cancelled if not executed when auction completed).

<table><thead><tr><th width="130">Type</th><th width="88">GTC</th><th width="79">GTD</th><th width="87">FOK</th><th width="87">IOC</th><th width="69">GAA</th><th>DAY</th></tr></thead><tbody><tr><td>Limit</td><td>✔️</td><td>✔️</td><td>❌</td><td>✔️</td><td>✔️</td><td>✔️</td></tr><tr><td>Market</td><td>❌</td><td>❌</td><td>❌</td><td>✔️</td><td>❌</td><td>❌</td></tr><tr><td>Stop</td><td>❌</td><td>❌</td><td>❌</td><td>❌</td><td>❌</td><td>❌</td></tr><tr><td>StopLimit</td><td>❌</td><td>❌</td><td>❌</td><td>❌</td><td>❌</td><td>❌</td></tr></tbody></table>
