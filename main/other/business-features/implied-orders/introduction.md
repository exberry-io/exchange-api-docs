# Introduction

Implied orders enhance the liquidity of the exchange by generating orders into and out of the parent instruments and increasing the probability of orders being executed.

* The feature is optional and can be configured separately at the instrument level.
* Parent instrument is an instrument configured in a system where the category is “Strategy”, which means it is a combination of at least two leg instruments.
* The orders used to generate implied orders are referred to as “Base Orders”.

### Strategies Supporting Implied Orders

1. Spreads - Simultaneous purchase and sale (or vice versa) for 2 instruments, can be used for calendar spread as well as inter commodity spread.
2. Strip (Equal Price) - Simultaneous purchase or sale of all leg instruments at the same price.
3. Strip (Average Price) - Simultaneous purchase or sale of all leg instruments at the average of the price of the legs.

### Implied Order Types&#x20;

1. Implied In Orders - Implied Orders are generated into the parent instrument using the liquidity of the leg instruments.
2. Implied Out Orders - Implied Orders are generated out of the parent instrument using the liquidity of the parent instrument and one of the leg instruments.

