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
* <mark style="color:blue;">(NEW v1.41.0)</mark> **OPG** - At the Opening
  * Order is kept in Suspended state until it becomes eligible to be injected
  * injected to the order book at beginning of the Auction Call of the auction with allowedTimeInForces = OPG
* <mark style="color:blue;">(NEW v1.41.0)</mark> **ATC** - At the Close
  * Order is kept in Suspended state until it becomes eligible to be injected
  * injected to the order book at beginning of the Auction Call of the auction with allowedTimeInForces = ATC

Availability During Continuous Trading:

<table><thead><tr><th width="160">Type</th><th width="85" align="center">GTC</th><th width="75" align="center">GTD</th><th width="79" align="center">FOK</th><th width="78" align="center">IOC</th><th width="76" align="center">GAA</th><th width="63">DAY</th><th width="64">OPG</th><th>ATC</th></tr></thead><tbody><tr><td>Limit</td><td align="center">✔️</td><td align="center">✔️</td><td align="center">✔️</td><td align="center">✔️</td><td align="center">❌</td><td>✔️</td><td>✔️</td><td>✔️</td></tr><tr><td>Market</td><td align="center">❌</td><td align="center">❌</td><td align="center">✔️</td><td align="center">✔️</td><td align="center">❌</td><td>❌</td><td>✔️</td><td>✔️</td></tr><tr><td>Stop</td><td align="center">❌</td><td align="center">❌</td><td align="center">✔️</td><td align="center">✔️</td><td align="center">❌</td><td>❌</td><td>❌</td><td>❌</td></tr><tr><td>StopLimit</td><td align="center">✔️</td><td align="center">✔️</td><td align="center">✔️</td><td align="center">✔️</td><td align="center">❌</td><td>✔️</td><td>❌</td><td>❌</td></tr><tr><td><mark style="color:blue;">(NEW v1.38)</mark><br>MarketToLimit</td><td align="center">✔️</td><td align="center">✔️</td><td align="center">✔️</td><td align="center">✔️</td><td align="center">❌</td><td>✔️</td><td>❌</td><td>❌</td></tr></tbody></table>

<mark style="color:blue;">NEW v1.33.0</mark> Availability During Auction:

Note: IOC is available for market and limit orders,  Limit IOC is identical to Limit GAA (both are cancelled if not executed when auction completed).

<table><thead><tr><th width="163">Type</th><th width="88">GTC</th><th width="79">GTD</th><th width="65">FOK</th><th width="87">IOC</th><th width="69">GAA</th><th width="66">DAY</th><th width="64">OPG</th><th>ATC</th></tr></thead><tbody><tr><td>Limit</td><td>✔️</td><td>✔️</td><td>❌</td><td>✔️</td><td>✔️</td><td>✔️</td><td>✔️</td><td>✔️</td></tr><tr><td>Market</td><td>❌</td><td>❌</td><td>❌</td><td>✔️</td><td>❌</td><td>❌</td><td>✔️</td><td>✔️</td></tr><tr><td>Stop</td><td>❌</td><td>❌</td><td>❌</td><td>❌</td><td>❌</td><td>❌</td><td>❌</td><td>❌</td></tr><tr><td>StopLimit</td><td>❌</td><td>❌</td><td>❌</td><td>❌</td><td>❌</td><td>❌</td><td>❌</td><td>❌</td></tr><tr><td><mark style="color:blue;">(NEW v1.38)</mark><br>MarketToLimit*</td><td>❌</td><td>❌</td><td>❌</td><td>❌</td><td>❌</td><td>❌</td><td>❌</td><td>❌</td></tr></tbody></table>

<mark style="color:blue;">(NEW v1.38)</mark> \*Resting MarketToLimit orders entered prior to the auction are considers for the auction.
