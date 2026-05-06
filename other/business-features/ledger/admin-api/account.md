# Account

The API documentation is [here](https://www.postman.com/exberry-team/admin-api/folder/upj9xr4/accounts-api). Below additional details apply.

### Additional fields/values of the Account entity.

<table><thead><tr><th width="186.0869140625">Parameter</th><th width="113.95247395833331">Type</th><th>Description</th></tr></thead><tbody><tr><td>type</td><td>eNum</td><td><ul><li><strong>Ledger</strong> - Accounts used to hold assets. (allowed only when ledger is enabled)</li><li><mark style="color:blue;">(NEW v1.56.0)</mark> <strong>System</strong> - Account used to maintain balances of the operator in case of trade settlement of Binary Event Contract. (can have only 1 account)</li></ul></td></tr><tr><td>ledgerAccountId</td><td>Int</td><td><p>Id of the Linked Ledger Account<br></p><p>Required for type=Trading AND when ledger is enabled, else not allowed.</p></td></tr></tbody></table>

