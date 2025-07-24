# Example Workflow for Claude Code + Excel MCP Server

A fast path from *Brainstorm* ➔ *Plan* ➔ *Build* ➔ *Review* ➔ *Repeat*—without ever opening Excel.

---

### Why Excel MCP Server?

The **Excel MCP Server** exposes a rich tool set—create workbooks, read/write data, style ranges, build charts, even generate pivot tables—through simple JSON calls that any LLM agent (Claude Code, GPT‑4o, etc.) can invoke. It runs locally or as a lightweight HTTP service, so there’s **no need for desktop Excel or an O365 license on the server side**. ([GitHub][1])

---

## The 5‑Step C‑Suite Workflow

| Phase             | Your Role (Minutes) | What the Agent Does                                                                                                                                                                                                                                           |
| ----------------- | ------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **1. Brainstorm** | 5 min               | • You describe the business question in plain English—“Show me quarterly EBITDA versus forecast with a 12‑month rolling average”.<br>• The agent refines goals, clarifies edge cases, and surfaces required data sources.                                     |
| **2. Plan**       | 10 min              | • Agent drafts an **`analysis_plan.md`** that lists every Excel action it must take (import CSVs, clean fields, add formulas, build a chart).<br>• You skim the plan, reorder priorities, and add strategic guardrails (e.g., “Red‑flag any variance > 5 %”). |
| **3. Build**      | 0 min (hands‑off)   | • Claude Code calls Excel MCP Server tools—`create_workbook`, `write_data_to_excel`, `apply_formula`, `create_chart`, etc.—executing each step in the plan. ([GitHub][1])                                                                                     |
| **4. Review**     | 5 min               | • Agent pauses, returns the workbook path plus a short executive summary.<br>• You open the file (desktop Excel, Numbers, or browser preview) and request tweaks (“Combine bars and line into a dual‑axis chart; highlight Q2 drop”).                         |
| **5. Repeat**     | < 2 min per cycle   | • Type “continue” or dictate new questions. The agent loops back to **Plan** for the delta and reruns only the affected steps.                                                                                                                                |

> **Outcome:** You spend \~20 minutes of strategic input, and the model spends the rest crunching numbers, formatting tables, and drafting insights.

---

### Quick Technical Setup (your team can handle once)

1. **Install & launch the server**

```bash
uvx excel-mcp-server stdio   # recommended transport
```

2. **Expose it to the agent**

```bash
claude mcp add excel uvx excel-mcp-server stdio
```

---

### Guardrails for Accurate, Board‑Ready Output

| Guardrail           | How to Implement                                                                                 |
| ------------------- | ------------------------------------------------------------------------------------------------ |
| **Data Integrity**  | Add a `validate_formula_syntax` and `data_validation` step to the plan.                          |
| **Version Control** | Instruct the agent to save each pass as `filename_YYYY‑MM‑DD_vN.xlsx` before overwriting.        |
| **Audit Trail**     | Append a “Changes” sheet—auto‑populated by the agent—with timestamp, action, and user comment.   |
| **Confidentiality** | Run the server inside your network; block outbound file writes except to approved shared drives. |

---

### Example One‑Liner to Kick Things Off

> **“Claude, create a workbook that pulls last month’s revenue and expense exports, cleans the data, calculates EBITDA by region, flags any variance above 5 %, and builds a dashboard sheet with a stacked bar + 12‑mo rolling line. Pause for review.”**

---

## Key Takeaways for Leaders

1. **Front‑load thinking, not formatting**—let the model handle cell references and charts.
2. **Keep the loop tight**—a two‑page plan beats a 20‑page spec.
3. **Demand an executive summary with every run**—you get insight, not screenshots.
4. **Iterate quickly**—your strategic follow‑up questions become the next plan.

Adopt this Brainstorm → Plan → Build → Review → Repeat rhythm and you’ll turn ad‑hoc spreadsheet requests into a disciplined, auditable pipeline—freeing you to focus on the decisions that move the business.

[1]: https://github.com/haris-musa/excel-mcp-server "GitHub - haris-musa/excel-mcp-server: A Model Context Protocol server for Excel file manipulation"
