# Welcome

Welcome to the Exberry Exchange API documentation. This comprehensive guide is designed for developers and market participants looking to integrate with our state-of-the-art exchange platform. Whether you're building automated trading systems, creating real-time market data analytics, or managing complex order flows, our API provides the robust and reliable access you need.

Leveraging industry-standard WebSocket and FIX protocols, the Exberry API offers:

* **Real-time Trading & Order Management:** Submit, track, modify, and cancel a diverse range of order types, including Limit, Market, Stop, and sophisticated time-in-force options, while receiving instant execution reports.
* **Market Data:** Access live and historical market data, including order book depth, time & sales, and instrument status updates, to power your insights and strategies.
* **Drop Copy & Reporting:** Obtain real-time copies of trading activity and detailed historical reports for reconciliation and analysis.
* **Advanced Business Features:** Explore advanced functionalities like Request for Quote (RFQ), Self-Trade Prevention (STP), Implied Orders for enhanced trading capabilities and more.

This documentation will guide you through every aspect of our API, from initial connection to advanced trading workflows, ensuring you can harness the full power of the Exberry system.

## Sandbox Configurations

<table><thead><tr><th width="170">API</th><th>Endpoint &#x26; Configurations</th></tr></thead><tbody><tr><td><p>Trading</p><p>Market Data</p><p>Private Data</p></td><td><pre><code>wss://exchange-gateway.uat.exberry-uat.io
</code></pre></td></tr><tr><td>Reporting </td><td><pre><code>wss://api-gateway.uat.exberry-uat.io
</code></pre></td></tr><tr><td>FIX</td><td><pre><code>"host": "fix-gateway.uat.exberry-uat.io",
"port": 11080,
"targetCompId": "EXBERRY"
</code></pre></td></tr></tbody></table>

\
