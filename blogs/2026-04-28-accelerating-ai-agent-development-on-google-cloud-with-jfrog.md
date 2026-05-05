---
title: "Accelerating AI Agent Development on Google Cloud with JFrog MCP Registry"
url: "https://jfrog.com/blog/accelerating-ai-agent-development-google-cloud-jfrog-mcp-registry/"
date: "Tue, 28 Apr 2026 16:11:59 +0000"
author: "zoer"
feed_url: "https://jfrog.com/blog/feed/"
---
<p><img alt="" class="aligncenter wp-image-166202 size-full" height="300" src="https://media.jfrog.com/wp-content/uploads/2026/04/28175746/863x300-8-1.png" width="863" /></p>
<p>Developers building agentic AI on Google Cloud have powerful infrastructure at their fingertips: Gemini 3 for reasoning, Google’s Agent Development Kit (ADK) for orchestration, and a rapidly expanding ecosystem of Model Context Protocol (MCP) servers that connect agents to data and tools. So why are so many teams still waiting weeks to ship their first agent to production?</p>
<p>The bottleneck isn’t the technology. It’s governance. Every new MCP server a developer wants to use needs to be reviewed, and without automated tooling to vet those servers, that review lands in the security team’s queue. The result is a familiar stalemate: developers eager to build, security teams unwilling to approve what they can’t see, and a growing shadow ecosystem of unreviewed servers running under the radar of both.</p>
<p><a href="https://jfrog.com/ai-catalog/mcp-registry">JFrog MCP Registry</a> is purpose-built to break this stalemate, giving Google Cloud teams a governed, self-serve path to MCP adoption, without asking security to accept blind trust.</p>
<h2>The Real Cost of Ungoverned MCP Adoption</h2>
<p>MCP servers aren’t passive data connectors. They execute commands with real privileges inside your environment. On Google Cloud, Gemini-powered agents use MCP servers to query BigQuery, interact with Google Kubernetes Engine, manage Cloud Storage, and execute complex multi-step workflows. That access is valuable. It’s also dangerous when left unmanaged.</p>
<p>Three risks justify security’s caution:</p>
<ul>
<li><strong>Supply chain exposure</strong> — MCP servers pulled from GitHub repositories, community catalogs, or direct vendor downloads may contain malicious code or vulnerabilities that weren’t visible at install time. Without a governance layer, developers connect agents to servers that have never been scanned.</li>
<li><strong>Over-privileged agent access</strong> — MCP servers often grant agents broader access than any task requires. Without tool-level permissions, an agent with access to a BigQuery MCP server may be able to query, delete, or modify data far beyond its intended scope. And at machine speed, the blast radius is large.</li>
<li><strong>Shadow AI</strong> — Blanket bans don’t stop MCP adoption; they push it underground. Developers find workarounds. MCP servers run locally on developer machines, completely outside security’s view. Gartner recommends that security leaders implement a centralized MCP server registry with layered security controls, because the answer to ungoverned adoption isn’t less access, it’s structured, auditable access.</li>
</ul>
<p>Understanding these risks is what makes the security team’s “no” rational and a governed “yes” possible.</p>
<h2>How JFrog MCP Registry Solves the Problem</h2>
<p>JFrog MCP Registry is the industry’s first enterprise-scale control plane built to govern MCP servers across the full <a href="https://jfrog.com/platform/">Agentic Software Supply Chain</a>. It sits inside the JFrog AI Catalog, the single system of record for all enterprise AI assets: models, agent skills, and MCP servers, unified alongside your traditional software artifacts. Here is how the governance layers work:</p>
<p><strong>Perimeter defense via JFrog Curation<br />
</strong><a href="https://jfrog.com/curation/">JFrog Curation</a> automatically vets every MCP server against your security, compliance, and operational policies before it can be used. Malicious or non-compliant servers are blocked at the gate, before they ever reach a developer’s machine.</p>
<p><strong>Granular, tool-level permissions<br />
</strong>JFrog MCP Registry enforces permissions at the individual tool level, not just the server level. A GCP team building a Gemini-powered analytics agent can access exactly the BigQuery read tools they need, without gaining access to tools that modify or delete data.</p>
<p><strong>Frictionless IDE integration<br />
</strong>JFrog MCP Registry integrates directly with Cursor, VS Code, and Claude Code through a lightweight CLI Gateway. Developers get a self-serve catalog of pre-approved MCP servers they can connect to instantly, no security tickets, no waiting. Governance happens invisibly, at the point of request.</p>
<p><strong>Complete MCP visibility<br />
</strong><a href="https://jfrog.com/ai-catalog/">JFrog AI Catalog</a> gives security teams a full inventory of every MCP server in use across the organization: which servers are active, what tools they expose, and what policies apply. For the first time, the CISO can answer the question that previously had no answer: what do our agents actually have access to?</p>
<h2>Velocity and Governance Are Not a Tradeoff</h2>
<p>When MCP governance is automated and built into the development workflow, the security review that used to slow everything down becomes invisible. Consider what changes for GCP teams:</p>
<table>
<tbody>
<tr>
<td><b>Stage</b></td>
<td><b>Without JFrog MCP Registry</b></td>
<td><b>With JFrog MCP Registry</b></td>
</tr>
<tr>
<td><span style="font-weight: 400;">Security review</span></td>
<td><span style="font-weight: 400;">Manual ticket</span></td>
<td><span style="font-weight: 400;">Automated policy engine</span></td>
</tr>
<tr>
<td><span style="font-weight: 400;">Server discovery</span></td>
<td><span style="font-weight: 400;">Manual search across GitHub, vendor docs</span></td>
<td><span style="font-weight: 400;">Self-serve catalog inside the IDE</span></td>
</tr>
<tr>
<td><span style="font-weight: 400;">Access control</span></td>
<td><span style="font-weight: 400;">Server-level, all-or-nothing</span></td>
<td><span style="font-weight: 400;">Granular tool-level permissions</span></td>
</tr>
<tr>
<td><span style="font-weight: 400;">Visibility</span></td>
<td><span style="font-weight: 400;">No system of record</span></td>
<td><span style="font-weight: 400;">Full inventory in JFrog AI Catalog</span></td>
</tr>
</tbody>
</table>
<p>&nbsp;</p>
<p>Your teams on Google Cloud can adopt new Gemini capabilities faster than competitors still processing manual reviews. Your security team can say “yes” to AI adoption without accepting blind trust. Your agents are only as trustworthy as what they consume, build, and ship — and JFrog governs all of it in a single source of truth.</p>
<p>JFrog MCP Registry is available now as part of the JFrog AI Catalog. To see how it can eliminate your security bottleneck and accelerate time-to-agent on Gemini, <a href="https://jfrog.com/ai-catalog/demo/">schedule a demo</a> with a JFrog expert, <a href="https://jfrog.com/start/">take an online tour</a>, or <a href="https://jfrog.com/start-free/">start a free trial</a> of the JFrog Software Supply Chain Platform.</p>
