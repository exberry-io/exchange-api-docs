# Introduction

Market Participants can use these APIs to report their off-exchange trades into the exchange.&#x20;

The system supports 2 reporting workflows.

1. **Locked In** - one MP reports both sides of the trade. This can be used when MPs have pre-agreements with counterparty MPs to report trades as final without any counterparty involvement.&#x20;
2. **Alleged System Match** - both MPs report the trade. The trade becomes final if the system is able to match the terms of the trade. Following fields should be equally reported from both parties to match:
   * Instrument
   * Trade type
   * Price
   * Quantity
   * Buy MP&#x20;
   * Sell MP
   * External Trade Id

```mermaid
sequenceDiagram
title Locked-in Flow
    actor mp1 as MP1
    participant exberry as Exberry
    actor mp2 as MP2
    actor all_mps as All MPs
        mp1 ->> exberry: Reports the trade
        exberry ->> mp1: Trade Report
        exberry ->> mp2: Trade Report
        exberry ->> all_mps: Market Data Updates

```

```mermaid
sequenceDiagram
title Alleged-System Match Flow
    actor mp1 as MP1
    participant exberry as Exberry
    actor mp2 as MP2
    actor all_mps as All MPs
  
        mp1 ->> exberry: Reports the trade
        exberry ->> mp1: Trade Report Acknowledgement
        exberry ->> mp2: Alleged Trade
        mp2 ->> exberry: Reports the trade
        exberry ->> exberry: Alleged Trade Match
        exberry ->> mp1: Trade Report
        exberry ->> mp2: Trade Report
        exberry ->> all_mps: Market Data Updates
    
```

