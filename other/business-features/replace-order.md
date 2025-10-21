# Replace Order

<mark style="color:blue;">(NEW v1.48.0)</mark> \
Replace Order API allows modifying selected parameters of an order that is currently resting on the book. When this request is processed, the system updates the order’s attributes to match the values provided in the request.

If the request only reduces the order quantity, it is processed without losing time priority. Other fields may either remain unchanged (using the same values as the original order) or be omitted from the request.

You may also reduce `Display Quantity` together with `Quantity`, provided that\
`Δ Quantity ≥ Δ Display Quantity` — this ensures no transfer occurs from the working _displayed_ quantity to the _hidden_ quantity — and the order will still retain its time priority.

For any other type of change, the system will update the order’s attributes accordingly, but the order will lose its time priority.
