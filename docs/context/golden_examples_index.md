# Golden examples index

| ID | Thing it covers | Order / Case | Tool | Allowed? | Why it matters |
|---|---|---|---|---:|---|
| GX01 | Old snapshot beats new store rule | 10041 | `lookup_return_eligibility` | no | This is the cleanest example of order-level snapshots beating current policy copy. |
| GX02 | No delivery scan falls back to fulfillment plus transit buffer | 10052 | `lookup_return_eligibility` | yes | This forces the system to use the documented fallback instead of guessing from order date. |
| GX03 | Final sale overrides the normal window | 10088 | `lookup_return_eligibility` | no | This is a classic product-level failure: the assistant sounds sensible if it only reasons about the window. |
| GX04 | Self-serve off does not block manual admin returns | 10103 | `lookup_return_eligibility` | yes | Great asymmetry example: customer path and admin path are not the same thing. |
| GX05 | Legacy customer accounts block self-serve returns | 10111 | `lookup_return_eligibility` | no | This is a pure product-settings trace rather than a per-order edge case. |
| GX06 | Self-serve exchanges are not supported | 10122 | `lookup_return_eligibility` | no | This is one of the nicest roast examples because a plausible UX assumption is still wrong. |
| GX07 | Self-serve request over 250 line items must be split | 10131 | `lookup_return_eligibility` | no | This gives you a weird but grounded edge case that feels product-real, not benchmark-y. |
| GX08 | Already refunded items cannot become returns | 10140 | `lookup_return_eligibility` | no | This is a strong example of refund state changing what operations are still legal. |
| GX09 | Support can approve a request and add an exchange item | 10151 | `mutate_return_case` | yes | Useful positive path. The build needs at least one trace where the right answer is a confident yes. |
| GX10 | Orders with duties block exchange items | 10163 | `lookup_return_eligibility` | no | A nice example where ‘in stock’ is not the same thing as ‘allowed’. |
| GX11 | US store to US customer can buy return label in Shopify admin | 10174 | `lookup_return_eligibility` | yes | Positive label path. Helpful for keeping the demo from becoming all negatives. |
| GX12 | US store to Germany customer cannot buy return label in Shopify admin | 10179 | `lookup_return_eligibility` | no | This is a great roast example because the wrong answer sounds perfectly plausible if the model only half-remembers the rule. |
| GX13 | Partial processing is allowed and the return-shipping fee applies once | 10184 / RET10184 | `mutate_return_case` | yes | Good operational trace: the assistant can sound conservative and still be product-wrong. |
| GX14 | Cancel return is allowed when the case is still untouched | 10195 / RET10195 | `mutate_return_case` | yes | Positive state-machine example. It helps the build show not all mutations are blocked. |
| GX15 | Cancel return is blocked once a label already exists | 10203 / RET10203 | `mutate_return_case` | no | Perfect roast material: the money-focused answer sounds sensible and is still wrong. |
| GX16 | Canceled returns cannot be reopened | 10211 / RET10211 | `mutate_return_case` | no | Another very visual state-machine trace. Great for live labelling. |