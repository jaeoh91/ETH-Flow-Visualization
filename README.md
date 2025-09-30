### Usage Instructions: ETH Flow Visualization

1. Parameters (in the visualization cell):
   - `CSV_PATH`: path to `transactions.csv`.
   - `OFAC_TAGGED`: dictionary of tagged / sanctioned addresses you want highlighted (red).
   - `MIN_ETH_EDGE`: set a threshold to hide tiny flows after aggregation.
   - `TOP_N_ADDRESSES`: limit graph to most significant addresses by total flow; keeps OFAC + focus addresses regardless.
   - `FOCUS_ADDRESSES`: addresses you always want included and highlighted (gold).
   - Time filtering: set `TIME_COLUMN`, `TIME_START`, `TIME_END` if a timestamp column exists.
   - `LIMIT_TXS`: read only top N rows for quick iteration.

2. Run the visualization cell. It will:
   - Aggregate transactions by (from_addr, to_addr).
   - Sum ETH (or `value` / `amount`). If no numeric column is found, each tx counts as 1.
   - Size nodes by total (in+out) ETH.
   - Scale edge width by aggregated ETH.
   - Save interactive HTML to `eth_flow_advanced.html` using PyVis or fallback to manual vis.js if PyVis fails.
   - Print success message with instructions to open the HTML file in your browser.

3. Run the helper analysis cell:
   - Prints top inbound/outbound counterparties for each OFAC/focus address.
   - Shows summary table with in/out/net ETH.

4. Extending tagging:
   ```python
   OFAC_TAGGED.update({
       "0xAnotherAddress": "SUSPECT_TAG"
   })
   ```
   Then rerun visualization.

5. Exporting / sharing:
   - The generated `eth_flow_advanced.html` is standalone; you can email or host it.
   - For static images, open the HTML, take a screenshot, or use browser print to PDF.

6. Performance tips:
   - Large graphs (>5k edges) get cluttered; use `TOP_N_ADDRESSES` or raise `MIN_ETH_EDGE`.
   - Consider pre-filtering `transactions.csv` to a date/event window.

7. Possible next enhancements (ask if you want help):
   - Add chain hop depth filtering from a seed address.
   - Compute centrality metrics (betweenness, pagerank) and color by role.
   - Integrate ENS resolution for addresses.
   - Add transaction hash sampling on edge hover.

8. Troubleshooting:
   - If you see `ValueError: CSV must contain columns ...`, verify column names match `from_addr` / `to_addr` exactly.
   - If edges appear but nodes have size ~ same: distribution might be flat; adjust scaling formula.
   - If PyVis fails with template errors, the code automatically falls back to manual vis.js HTML generation.
   - The visualization will always generate a working HTML file, either via PyVis or the fallback method.

Run cells in order: visualization (builds globals) -> analysis -> tweak parameters -> rerun.
