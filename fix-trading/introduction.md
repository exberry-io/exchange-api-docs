# Introduction

The FIX API meant to serve clients with FIX integration knowledge and experience, if this is not the case, it is recommended to use [Exberry proprietary API](../) which is easier to integrate.

Exberry FIX gateway enables participants:

* Trading:  allows to submit orders commands and to receive real-time information on executed trades.
* Drop Copy: Recieve real time orders activcity and trade reoprts&#x20;
* Market Data: consume diffrent types of market data\


The interface is a point-to-point service based on the technology and industry standards TCP/IP, FIXT and FIX.\
The session and application event models and messages are based on versions FIXT1.1 and FIX5.0 (SP 2) respectively.

Sandbox Configurations:&#x20;

```json
"host": "fix-sandbox-shared.staging.exberry-uat.io",
"port": 8888,
"targetCompId": "EXBERRY"
```
