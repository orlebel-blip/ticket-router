# Build Dana’s Bakery order graph

Create a Google Colab notebook named `dana_bakery - class.ipynb`. Use Python and LangGraph, install LangGraph with an initial `%pip` cell, and display the compiled graph with `draw_mermaid_png()`. Use deterministic customer replies. The notebook must run without an LLM, API key, or local files.

## Input and result

Support `graph.invoke({"item": "babka", "quantity": 2, "day": "Thursday", "city": "Jerusalem"})`. Normalize text comparisons with trimming and case folding. Reject unknown products, discontinued croissants, and quantities that are not positive integers. Reject missing or unrecognized weekdays and non-text cities. Treat a missing, null, or blank city as pickup.

Return six keys on every path: `status` ("accepted" or "rejected"), `item` (the actual product sold), `delivery` ("pickup" or "delivery"), `total` (shekels including delivery), `message` (the customer reply), and `steps` (the ordered trace). For rejection, use a null item, "pickup" as the delivery placeholder, and a zero total. A rejected order has no fulfillment.

## Order rules

| Item | Unit price (₪) | Starting stock |
| --- | ---: | ---: |
| challah | 18 | 10 |
| babka | 45 | 0 |
| rugelach (box) | 30 | 3 |
| sourdough (loaf) | 25 | 4 |

Evaluate each order against the same starting stock. Check whether the full quantity is available. If it is unavailable, follow the chain babka → rugelach → sourdough → challah, checking at most two substitutes. Accept the first candidate with enough stock at its own unit price and the original quantity. Reject when the chain ends or both permitted substitutes fail.

Discount the baked-goods subtotal by 15% only if it exceeds ₪150 and the day is not Friday. Add ₪20 for Jerusalem delivery after calculating the discount. Pickup is free. For any other city, keep the order for pickup and explain the switch. Mention any substitution in the acceptance reply and give a warm, specific reason for rejection.

## Graph and acceptance checks

Use a TypedDict state. Route from validation to rejection or stock checking. Route each stock check to pricing, substitution followed by another stock check, or rejection. Follow pricing with fulfillment and acceptance. End after either acceptance or rejection. Record each visited node in order, including the item and outcome for each stock check.

Include runnable example assertions for direct acceptance, each substitution depth, both stock cancellation conditions, and invalid orders. Cover all three fulfillment outcomes, the strict ₪150 threshold in a pricing check, the weekday discount, the Friday exception, and the delivery fee after discount. Check the six output keys, totals, ordered traces, and relevant reply text. Two babkas for Thursday delivery in Jerusalem must return two boxes of rugelach for ₪80.
