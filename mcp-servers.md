# MCP servers (for non‑engineers) — and how Claude Code uses them

**TL;DR:**
*MCP (Model Context Protocol) servers* are small helper programs that let AI tools (like **Claude Code**) safely talk to the apps, files, and data you already use—Excel, databases, ticketing systems, Git, cloud APIs, etc. Think of MCP as a **universal adapter**: instead of hard‑coding one‑off integrations, Claude Code can “plug into” any MCP server and instantly gain new, well-defined abilities.

---

## 1) What problem do MCP servers solve?

Without MCP, every AI integration is a bespoke project: new auth, new API shapes, new security questions. MCP standardizes the way an AI client (Claude Code) discovers **what it’s allowed to do**, **what tools exist**, **what arguments they take**, and **how results are returned**.

**Benefits:**

* **Safety:** Clear boundaries—Claude only sees and does what the MCP server exposes.
* **Reusability:** One protocol; many integrations.
* **Portability:** Swap clients or servers without rewriting everything.
* **Speed to value:** Turn your Excel sheets, databases, or internal services into instantly usable AI “tools”.

---

## 2) Core mental model

```
[You] ──talk to──> [Claude Code (client)]
                      │
                      ▼
           [MCP server(s): “tools”]
            • Excel
            • Databases
            • Git
            • Ticketing systems
            • Anything else you expose
```

* **Claude Code** = the *client* that wants to help you.
* **MCP server** = the *toolbox* that declares, “Here are the safe actions I allow, here’s how to call them.”
* **You** = the human in control (approve, monitor, and audit what’s exposed).

---

## 3) What exactly is inside an MCP server?

An MCP server typically exposes three things:

1. **Tools / functions**

   * e.g., `read_sheet(sheet="P&L Q2")`, `run_sql(query="…")`, `create_ticket(summary="…")`
2. **Resources / data**

   * e.g., list of spreadsheets, tables, repositories
3. **Prompts / templates (optional)**

   * Reusable prompt snippets that the client can ask the AI to fill in.

The protocol defines **how** those are described, discovered, called, streamed, and errored—so Claude Code can use them without custom glue code.

---

## 4) How Claude Code uses MCP servers (at a high level)

When Claude Code starts up (or when you add a new MCP server), it:

1. **Discovers** the server (from a config file or environment variables).
2. **Asks for capabilities:** “What tools do you expose?”
3. **Shows you** (or auto-loads) those tools in the UI.
4. **Calls the tools on demand** when you ask for something that requires them.
5. **Streams results back** (so you can see partial progress).
6. **Logs / audits** (so you can track what happened, if configured).

---

## 5) Real-world examples you might care about

* **Excel MCP server**
  Let Claude Code *read a sheet, transform data, write results back*, and even build formulas—without emailing CSVs around.
* **Database MCP server**
  Run parameterized queries, summarize results, generate migration scripts—safely and with least privilege.
* **Issue tracker / ticketing (Jira, ServiceNow, Freshservice)**
  Auto-create tickets, comment on issues, generate changelogs.
* **Git MCP server**
  Draft PRs, review diffs, generate commit messages—while respecting repo permissions.
* **Internal APIs**
  Wrap any in-house system behind MCP to give Claude structured, controlled access.

---

## 6) Security & governance (non-negotiables)

* **Principle of least privilege:** Only expose what’s needed.
* **Auditing:** Log each call (who/what/when).
* **Auth & secrets:** Keep credentials outside Claude (the MCP server handles them).
* **Environment separation:** Different servers (or modes) for dev, staging, prod.
* **Prompt hygiene:** Don’t let the model craft raw SQL against prod unless you’ve explicitly decided that’s ok.

---

## 7) Installing & wiring one up (conceptual steps)

> Exact steps vary per server, but the flow is similar:

1. **Install the server**

   * e.g., `pip install excel-mcp-server` or `npm i some-mcp-server`.
2. **Configure it**

   * Point it at the resource (Excel files, DB connection string, API token).
3. **Tell Claude Code about it**

   * Add it to Claude Code’s configuration (often a JSON/YAML entry) so it can discover & connect.
4. **Approve & test tools**

   * Run a simple call (e.g., “List all Excel sheets”) to verify access and permissions.
5. **Use it naturally**

   * “Hey Claude, summarize the ‘Revenue by State’ sheet and graph the top 5 states.”

### Tiny (illustrative) config snippet

```jsonc
{
  "mcpServers": {
    "excel": {
      "command": "excel-mcp-server",
      "args": ["--path", "/Users/you/FinanceWorkbook.xlsx"]
    }
  }
}
```

*(This is just a shape example — real configs vary.)*

---

## 8) Typical workflow inside Claude Code

1. **You ask:**

   > “Compare last quarter’s revenue to this quarter’s in the Excel file and show me any >5% changes.”

2. **Claude decides:**

   * “I need the Excel MCP tool `read_sheet` and maybe `write_sheet`.”

3. **Claude calls the tool(s):**

   * Reads the sheets, computes differences, writes a new tab named `Variance_QoQ`.

4. **Claude reports back:**

   * Human-readable summary + a link to the updated sheet.

---

## 9) Common gotchas

* **Mis-scoped permissions:** Accidentally exposing production write access. Start read-only.
* **Ambiguous tool names:** Name tools clearly (`run_safe_sql`, not just `run`).
* **No rate limits / timeouts:** Long-running tasks should stream progress or fail gracefully.
* **Lack of monitoring:** If you can’t answer “who did what, when?” you’ll regret it.

---

## 10) Glossary

* **MCP (Model Context Protocol):** The standard “language” that lets AI clients and servers talk safely and consistently.
* **Client:** The AI assistant interface (Claude Code) using the tools.
* **Server:** A program that exposes tools/data to the client via MCP.
* **Tool:** A callable action the AI can use (e.g., `read_sheet`, `run_sql`).
* **Resource:** A data object the AI can reference (e.g., a file, table, repo).

---

## 11) Quick FAQ

**Q: Do I need to be a developer to benefit?**
*A little setup helps, but once an MCP server is configured, non-technical users can ask natural questions and let Claude Code do the heavy lifting.*

**Q: Can Claude see everything on my machine?**
*No. Claude only sees what the MCP server explicitly exposes.*

**Q: Is this only for coding?**
*No. Finance, operations, HR, and legal can all benefit—especially with MCP servers for Excel, BI tools, and ticketing systems.*

**Q: How is this different from “plugins”?**
*MCP is an **open, language-agnostic protocol**. You can host your own, chain many together, and keep everything behind your firewall.*

---

## 12) The one-sentence takeaway

**MCP servers turn Claude Code into a secure, general-purpose “do-er” for your organization’s real data and systems—without the chaos of one-off integrations.**

