# Account

The API documentation is [here](https://www.postman.com/exberry-team/admin-api/folder/upj9xr4/accounts-api). Below additional details apply.

### Additional fields/values of the Account entity.

<table><thead><tr><th width="186.0869140625">Parameter</th><th width="113.95247395833331">Type</th><th>Description</th></tr></thead><tbody><tr><td>type</td><td>eNum</td><td><ul><li><strong>Position</strong> - Accounts is used to maintain positions (allowed only when positions are enabled)</li></ul></td></tr><tr><td>positionAccountId</td><td>Int</td><td><p>Id of the Linked Position Account</p><p>Required for account type=Trading AND when  positions are enabled, else not allowed.</p></td></tr><tr><td>positionCalculation</td><td>eNum</td><td><p>Optional</p><p>One of the below values: </p><ul><li>Net - system calculates positions for this account using net calculations</li></ul></td></tr></tbody></table>

