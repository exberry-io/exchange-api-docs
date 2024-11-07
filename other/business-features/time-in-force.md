# Time In Force

Available "Time In Force" :

* **GTC** - Good Till Cancel
  * &#x20;Order is resting on the book until cancellation or execution&#x20;
* **GTD** - Good Till Date&#x20;
  * &#x20;Order is resting on the book until cancellation or execution or expiry date or  expiry date & time (where order will be automatically cancelled)
* **FOK** - Fill Or Kill&#x20;
  * &#x20;Partial fill is not allowed. In case not all the amount can be immediately filled, the entire order amount will be cancelled.
* **IOC** - Immediate Or Cancelled&#x20;
  * Partial fill is allowed. The amount that can be immediately filled is filled, the residual amount is cancelled.
* **GAA** - Good At Auction&#x20;
  * can be captured only during auctions and will be cancelled after auction in case it was not filled .
* **DAY**&#x20;
  * Order will be automatically cancelled when trading day is closed.&#x20;

Availability During Continuous Trading:

<table><thead><tr><th width="137">Type</th><th width="85" align="center">GTC</th><th width="86" align="center">GTD</th><th width="79" align="center">FOK</th><th width="78" align="center">IOC</th><th width="76" align="center">GAA</th><th>DAY</th></tr></thead><tbody><tr><td>Limit</td><td align="center">✔️</td><td align="center">✔️</td><td align="center">✔️</td><td align="center">✔️</td><td align="center">❌</td><td>✔️</td></tr><tr><td>Market</td><td align="center">❌</td><td align="center">❌</td><td align="center">✔️</td><td align="center">✔️</td><td align="center">❌</td><td>❌</td></tr><tr><td>Stop</td><td align="center">❌</td><td align="center">❌</td><td align="center">✔️</td><td align="center">✔️</td><td align="center">❌</td><td>❌</td></tr><tr><td>StopLimit</td><td align="center">✔️</td><td align="center">✔️</td><td align="center">✔️</td><td align="center">✔️</td><td align="center">❌</td><td>✔️</td></tr></tbody></table>

<mark style="color:blue;">NEW v1.33.0</mark> Availability During Auction:

Note: IOC is available for market and limit orders,  Limit IOC is identical to Limit GAA (both are cancelled if not executed when auction completed).

<table><thead><tr><th width="130">Type</th><th width="88">GTC</th><th width="79">GTD</th><th width="87">FOK</th><th width="87">IOC</th><th width="69">GAA</th><th>DAY</th></tr></thead><tbody><tr><td>Limit</td><td>✔️</td><td>✔️</td><td>❌</td><td>✔️</td><td>✔️</td><td>✔️</td></tr><tr><td>Market</td><td>❌</td><td>❌</td><td>❌</td><td>✔️</td><td>❌</td><td>❌</td></tr><tr><td>Stop</td><td>❌</td><td>❌</td><td>❌</td><td>❌</td><td>❌</td><td>❌</td></tr><tr><td>StopLimit</td><td>❌</td><td>❌</td><td>❌</td><td>❌</td><td>❌</td><td>❌</td></tr></tbody></table>
