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

<mark style="color:blue;">(NEW v1.40.0)</mark> By default all orders of an MP are evaluated for potential self matches. Optionally, an stpParty (a role and a source) can be defined at exchange level to restrict STP scope.&#x20;

If defined,&#x20;

* Different ids of the stpParty will have their own scopes to be evaluated for potential self matches.&#x20;
* Orders without an stpParty have their own scope independent from orders with stpParty objects.&#x20;
* An order can have multiple party objects with STP party role and source with different ids.

Note: stpParty is not defined by default. Contact the Exberry team if you wish to define it.

**Example:**

Exchange level "stpParty": { "source": "D", "role": 3 }

Active orders: side=Sell qty=5, price-10, stpAction=CancelIncomingOrder (of all orders)

<table><thead><tr><th width="119">OrderID</th><th width="84">MP</th><th>Parties</th></tr></thead><tbody><tr><td>1</td><td>1</td><td>[ {id=01, role=3, source=D} ]</td></tr><tr><td>2</td><td>1</td><td>[ {id=02, role=3, source=D} , {id=02, role=4, source=D} ]</td></tr><tr><td>3</td><td>1</td><td>No Parties</td></tr><tr><td>4</td><td>2</td><td>No Parties</td></tr></tbody></table>



Incoming orders: side=Buy qty=5, price-10 (of all orders)

<table><thead><tr><th width="119">OrderID</th><th width="84">MP</th><th width="325">Parties</th><th>Self Matching OrderIDs</th></tr></thead><tbody><tr><td>21</td><td>1</td><td>[ {id=01, role=3, source=D} ]</td><td>Yes [1]</td></tr><tr><td>22</td><td>1</td><td>[ {id=01, role=3, source=D} , <br>{id=02, role=3, source=D} ]</td><td>Yes [1, 2]</td></tr><tr><td>23</td><td>1</td><td>Null</td><td>Yes [3]</td></tr><tr><td>24</td><td>1</td><td>[ {id=02, role=5, source=D} ]</td><td>Yes [3]</td></tr></tbody></table>
