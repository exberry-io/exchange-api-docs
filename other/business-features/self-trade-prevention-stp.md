# Self-Trade Prevention (STP)

#### General

Self-Trade Prevention is an optional functionality for the MP. The functionality may be used by MPs to avoid unintentional internal trading by preventing certain MP Orders from executing against each other.

MPs can choose to activate STP or not.&#x20;

Once activated at the MP level, STP can operate in the following modes:

* **MP Level:** Apply STP to all MP orders, with the specific STP action selected during MP setup.
* **Order Level:** Apply STP exclusively to specified orders where STP is explicitly requested.
* **MP & Order Levels:** Apply STP to all MP orders with the selected STP action from MP setup. Allow the flexibility of STP action selection at the order level.&#x20;

In the event of a potential self-trade, the following actions are available:

* **CancelIncomingOrder:** Cancel the incoming order&#x20;
* **CancelRestingOrder:** Cancel the resting order&#x20;
* **CancelBothOrders:** Cancel both the incoming and resting orders.

Note: This functionality is turned off by default. Contact the Exberry team if you wish to enable it in your environment.
