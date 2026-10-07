# Introduction

A trading model supported by the exchange where a Quote or Quotes are sent by dealers in response to a “Request for Quote(RFQ)” submitted by other initiators.\
Dealers/initiators can be any MPs or Traders of MPs.&#x20;

* All RFQ are public (The system currently doesn’t support private RFQs), have an expiration time after which the RFQ and all related Quotes get automatically cancelled by the system.&#x20;
* All Quotes are private, firm and executable.&#x20;
* The Quotes are executable exclusively by the party initiating the RFQ and the execution is triggered by initiator accepting a Quote.&#x20;

RFQ workflows supported by the system:

* Initiator creates an RFQ
* Dealer replies to an RFQ with Quotes
* Initiator accepts(hits/lifts) a Quote
* Dealer cancels a Quote
* Initiator cancels an RFQ
* RFQ expiration

**RFQ workflow diagram**

```mermaid
sequenceDiagram
    actor initiator as Initiator
    participant exchange as Exchange
    actor dealer1 as Dealer1
    actor dealer2 as Dealer2
    actor dealer3 as Dealer3
    initiator ->> exchange: Submit RFQ
    exchange ->> initiator: RFQ Accepted
    exchange ->> dealer1: RFQ Created
    exchange ->> dealer2: RFQ Created
    exchange ->> dealer3: RFQ Created
    dealer1 ->> exchange: Submit Quote 1
    exchange ->> dealer1: Quote 1 Accepted
    exchange ->> initiator: Quote 1 Created
    dealer2 ->> exchange: Submit Quote 2
    exchange ->> dealer2: Quote 2 Accepted
    exchange ->> initiator: Quote 2 Created
    initiator ->> exchange: Accepts Quote 2 (Dealer2)
    exchange ->> initiator: Quote 2 Executed & Trade Reports
    exchange ->> dealer2: Quote 2 Executed & Trade Reports
    exchange ->> dealer1: Quote 1 Canceled
    exchange ->> initiator: Quote 1 Canceled
    exchange ->> initiator: RFQ Ended
    exchange ->> dealer1: RFQ Ended
    exchange ->> dealer2: RFQ Ended
    exchange ->> dealer3: RFQ Ended

```

