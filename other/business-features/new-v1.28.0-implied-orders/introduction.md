# Introduction

Implied orders enhance the liquidity of the exchange by generating orders into and out of the parent instruments and increasing the probability of orders being executed.

* The feature is optional and can be configured separately at the instrument level.
* Parent instrument is an instrument configured in a system where the category is “Strategy”, which means it is a combination of at least two leg instruments.
* The orders used to generate implied orders are referred to as “Base Orders”.

### List of strategies supporting implied orders:

1. Spreads
2. Strips

### Implied order types supported by the system:

1. Implied In Orders - Implied Orders are generated into the parent instrument using the liquidity of the leg instruments.
2. Implied Out Orders - Implied Orders are generated out of the parent instrument using the liquidity of the parent instrument and one of the leg instruments.

