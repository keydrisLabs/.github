# Keydris

**Proof of authority for AI agents.**

Keydris is an authorization control plane for AI-agent actions. An operator assigns an agent bounded authority through policy; when the agent acts through a governed integration, Keydris evaluates that specific action — at action time — and returns a decision: **ALLOW**, **REJECT**, or **APPROVAL REQUIRED** (which resolves to ALLOW or REJECT; it is not a terminal state). Decisions are recorded as reviewable evidence, separately from what the tool then did.

[keydris.com](https://keydris.com)

---

## What we do

Agents increasingly act on real systems — MCP servers, APIs, repositories, workspaces. Keydris answers the question that has to be resolved before any of that is safe: **was this agent authorized to take this action?**

- Authority is assigned as policy and checked when the action happens, not only when a session starts.
- Changing a policy changes the next decision — without redeploying the agent or touching its code.
- The agent carries a short-lived session identity (a KIT), never the connected credential. How credentials are handled depends on the integration mode; the product's core idea is the separation of authority from the credential or mechanism used to execute an authorized action.
- Decisions and execution outcomes are recorded as separate, reviewable evidence. An allowed action can still fail at the provider; the record says both things.

## The KIT Reader

On Reader-style MCP integrations, the receiving server can start with no stored credential. A single-use, action-scoped **KIT action token** arrives inside the MCP request (`params._meta["keydris/kit_action_token"]`). The server redeems it at the Keydris gateway together with the actual action context — method, action name, parameters — and, if policy allows, the gateway releases the credentials that one call needs, applied to that one outbound request. The Reader holds configuration only, never per-request state; a refusal comes back as a readable answer in the tool result, not a crash. It is implemented for Node and Python. What a server does with a released credential after the call is outside the Reader's scope.

## Status

Keydris is in **Developer Preview** — no invite required. The current platform fails closed: when a governed action cannot be verified, it does not proceed. There is no production SLA yet, and the current public material makes no latency claim. The documented boundaries are part of the product.

## Getting started

- Website: [keydris.com](https://keydris.com)
- CLI: [`@keydris/cli` on npm](https://www.npmjs.com/package/@keydris/cli)

---

<sub>Authority before action.</sub>
