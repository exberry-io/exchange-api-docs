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



<mark style="color:blue;">(NEW v1.62.0)</mark> By default all orders of an MP are evaluated for potential self matches. Optionally, stpParties (each party with a role and a source) can be defined at exchange level to change STP scope.&#x20;

* When multiple STP parties are configured for an exchange, the system evaluates them in the order they were defined.
* Different IDs within STP Parties maintain their own evaluation scopes for potential self-matches.
* Orders without STP Parties operate within an MP-level scope, independent of orders associated with STP Party objects.
* Orders with STP Parties are evaluated for self-trades across all STP-enabled MPs.
* An order can include multiple party objects containing different IDs across STP party roles and sources.
* For orders with multiple STP Parties, only one party (role and source) is selected to evaluate self-trade eligibility.
* If multiple IDs exist for the selected party, all corresponding IDs will be evaluated for potential self-trades.

Note: stpParties are not configured by default. Contact the Exberry team if you wish to configure them in your environment.

**Example:**

Exchange level "stpParties":&#x20;

\[{ "source": "D", "role": 42 },&#x20;

{ "source": "D", "role": 38 }]

STP enabled for MPs 1,2, 3, 4, Disabled for MP 5

Active orders on below table: side=Sell qty=5, price=10:

<table><thead><tr><th width="101.12109375">OrderID</th><th width="107.4482421875">MP</th><th>Parties</th></tr></thead><tbody><tr><td>1</td><td>1</td><td>[{source=D, role=42, id=01}]</td></tr><tr><td>2</td><td>1</td><td><p>[{source=D, role=42, id=01} , </p><p>{source=D, role=42, id=02}]</p></td></tr><tr><td>3</td><td>1</td><td><p>[{source=D, role=42, id=01} , </p><p>{source=D, role=42, id=02} , </p><p>{source=D, role=4, id=02}]</p></td></tr><tr><td>4</td><td>2</td><td>[{source=D, role=42, id=01}]</td></tr><tr><td>5</td><td>1</td><td>[{source=D, role=42, id=02}]</td></tr><tr><td>6</td><td>3</td><td>NA</td></tr><tr><td>7</td><td>4</td><td>[{source=D, role=4, id=02}]</td></tr><tr><td>8</td><td>5</td><td>[{source=D, role=42, id=01}]</td></tr><tr><td>9</td><td>1</td><td><p>[source=D, role=42, id=12} , </p><p>{source=D, role=38, id=02}]</p></td></tr><tr><td>10</td><td>4</td><td>[{source=D, role=38, id=02}]</td></tr></tbody></table>



Incoming orders: side=Buy qty=5, price=10 (of all orders)

<table><thead><tr><th width="83.853515625">OrderID</th><th width="70.0009765625">MP</th><th width="345.3984375">Parties</th><th>Self-Matching OrderIDs</th></tr></thead><tbody><tr><td>21</td><td>1</td><td>[{source=D, role=42, id=01}]</td><td>[1, 2, 3, 4]</td></tr><tr><td>22</td><td>1</td><td><p>[{source=D, role=42, id=01},</p><p>{source=D, role=42, id=02}]</p></td><td>[1, 2, 3, 4, 5]</td></tr><tr><td>23</td><td>1</td><td><p>[{source=D, role=42, id=01},</p><p>{source=D, role=42, id=02},</p><p>{source=D, role=4, id=02}]</p></td><td>[1, 2, 3, 4, 5]</td></tr><tr><td>24</td><td>2</td><td>[{source=D, role=42, id=01}]</td><td>[1, 2, 3, 4]</td></tr><tr><td>25</td><td>3</td><td><p>[{source=D, role=42, id=01}, </p><p>{source=D, role=42, id=02}, </p><p>{source=D,  role=38, id=02}]</p></td><td>[1, 2, 3, 4, 5]</td></tr><tr><td>26</td><td>1</td><td>NA</td><td>None</td></tr><tr><td>27</td><td>1</td><td>[{source=D, role=5, id=02}]</td><td>None</td></tr><tr><td>28</td><td>3</td><td>NA</td><td>[6]</td></tr><tr><td>29</td><td>3</td><td>[{source=D, role=5, id=02}]</td><td>[6]</td></tr><tr><td>30</td><td>4</td><td>[{source=D, role=42, id=02}]</td><td>[2, 3, 5]</td></tr><tr><td>31</td><td>4</td><td>NA</td><td>[7]</td></tr><tr><td>32</td><td>4</td><td>[{source=D, role=5, id=01}]</td><td>[7]</td></tr><tr><td>33</td><td>4</td><td>[{source=D, role=42, id=01}]</td><td>[1, 2, 3, 4]</td></tr><tr><td>34</td><td>1</td><td><p>[{source=D, role=42, id=11}, </p><p>{source=D, role=38, id=02}]</p></td><td>None</td></tr><tr><td>35</td><td>1</td><td>[{source=D, role=38, id=02}]</td><td>[10]</td></tr></tbody></table>

<br>

<mark style="color:red;">(REMOVED v1.62.0)</mark> <mark style="color:blue;">(NEW v1.40.0)</mark> ~~By default all orders of an MP are evaluated for potential self matches. Optionally, an stpParty (a role and a source) can be defined at exchange level to restrict STP scope.~~&#x20;

~~If defined,~~&#x20;

* ~~Different ids of the stpParty will have their own scopes to be evaluated for potential self matches.~~&#x20;
* ~~Orders without an stpParty have their own scope independent from orders with stpParty objects.~~&#x20;
* ~~An order can have multiple party objects with STP party role and source with different ids.~~

~~Note: stpParty is not defined by default. Contact the Exberry team if you wish to define it.~~

~~**Example:**~~

~~Exchange level "stpParty": { "source": "D", "role": 3 }~~

~~Active orders: side=Sell qty=5, price-10, stpAction=CancelIncomingOrder (of all orders)~~

<table><thead><tr><th width="119">OrderID</th><th width="84">MP</th><th>Parties</th></tr></thead><tbody><tr><td><del>1</del></td><td><del>1</del></td><td><del>[ {id=01, role=3, source=D} ]</del></td></tr><tr><td><del>2</del></td><td><del>1</del></td><td><del>[ {id=02, role=3, source=D} , {id=02, role=4, source=D} ]</del></td></tr><tr><td><del>3</del></td><td><del>1</del></td><td><del>No Parties</del></td></tr><tr><td><del>4</del></td><td><del>2</del></td><td><del>No Parties</del></td></tr></tbody></table>



~~Incoming orders: side=Buy qty=5, price-10 (of all orders)~~

<table><thead><tr><th width="119">OrderID</th><th width="84">MP</th><th width="325">Parties</th><th>Self Matching OrderIDs</th></tr></thead><tbody><tr><td><del>21</del></td><td><del>1</del></td><td><del>[ {id=01, role=3, source=D} ]</del></td><td><del>Yes [1]</del></td></tr><tr><td><del>22</del></td><td><del>1</del></td><td><del>[ {id=01, role=3, source=D} ,</del> <br><del>{id=02, role=3, source=D} ]</del></td><td><del>Yes [1, 2]</del></td></tr><tr><td><del>23</del></td><td><del>1</del></td><td><del>Null</del></td><td><del>Yes [3]</del></td></tr><tr><td><del>24</del></td><td><del>1</del></td><td><del>[ {id=02, role=5, source=D} ]</del></td><td><del>Yes [3]</del></td></tr></tbody></table>

