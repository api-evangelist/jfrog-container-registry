---
title: "Unlock the Power of Agents with JFrog’s Skills and MCP Tools"
url: "https://jfrog.com/blog/ai-agents-jfrog-skills-mcp-tools/"
date: "Tue, 21 Apr 2026 15:45:19 +0000"
author: "zoer"
feed_url: "https://jfrog.com/blog/feed/"
---
<p><img alt="" class="aligncenter wp-image-165843 size-full" height="300" src="https://media.jfrog.com/wp-content/uploads/2026/04/20200724/Blog_Agentic_Workflows_Your_Way_863x300.png" width="863" /></p>
<p>Agents are writing code, suggesting dependencies, and reviewing PRs, without any knowledge about your trusted package sources, security posture, or governance policies. When agents operate without supply chain context, they introduce risk, create rework, and weaken the guardrails DevSecOps teams rely on to ship with confidence.</p>
<p>JFrog is changing that. Today, we’re launching an official set of <a href="https://github.com/jfrog/jfrog-skills" rel="noopener" target="_blank">JFrog Platform Skills</a> and expanding our suite of <a href="https://docs.jfrog.com/integrations/docs/jfrog-mcp-server-tools">MCP tools</a> and plugins to give autonomous agents full visibility into your JFrog-managed software supply chain. Every action your agent takes is now grounded in enterprise context, ensuring smarter, safer, and more sustainable software delivery.</p>
<h2>What Agents Can Do with Supply Chain Context</h2>
<p>With JFrog’s Skills, MCP tools, and plugins, your agents can actively participate in DevSecOps workflows analyzing, validating, and acting across your software supply chain. You ask in natural language and the agents use JFrog’s artifact, security, and governance data to take secure and compliant actions automatically.</p>
<p>Here are some of the high-impact workflows that become possible:</p>
<h3>Secure and Manage Your Software Supply Chain</h3>
<p>Enable agents to proactively secure your pipelines by querying JFrog&#8217;s security and curation data before vulnerable or tampered artifacts enter builds or production. Agents gain full visibility into CVEs, provenance, checksums, and build origins, shifting security left without slowing delivery. You get faster, safer releases with security built in at every step, not bolted at the end.</p>
<p><strong>Ask your agent: </strong></p>
<ul>
<li><em>Tell me about CVE-2021-44228</em></li>
<li><em>Which packages in libs-release-local have critical CVEs? Check curation status for the top 3 and whether they have been downloaded despite the vulnerability.</em></li>
</ul>
<h3>Ensure Governance and Compliance</h3>
<p>Let your agents keep you in check. Surface curation status, flag license risks, pull audit events, and identify packages that were downloaded or used despite being flagged. Every action is automatically aligned with your organization’s policies and governance stays intact even as automation scales.</p>
<p><strong>Ask your agent:</strong></p>
<ul>
<li><em>Is commons-compress 1.21 approved?</em></li>
<li><em>Curation audit events from the last 7 days</em></li>
</ul>
<h3>Trace Builds and Verify Provenance</h3>
<p>Agents can trace exactly where every artifact originated, verify its integrity, and surface the full chain of custody when something looks wrong.  They can instantly retrieve build origins, VCS commits, and checksum verification, accelerating root cause analysis without manual log digging, so developers can stay focused on building and shipping.</p>
<p><strong>Ask your agent:</strong></p>
<ul>
<li><em>Which build produced payment-service-1.4.2.jar? Show me build info and VCS commit.</em></li>
<li><em>Verify that ./lib/my-artifact.jar has not been tampered with. Check it against Artifactory and show me its build provenance.</em></li>
</ul>
<h3>Optimize Storage and Supply Chain Costs</h3>
<p>Need to understand what team, project, or artifacts are driving usage and storage costs? Agents can keep your artifact ecosystem lean by identifying stale or oversized artifacts, detecting SNAPSHOT buildup, and highlighting cleanup opportunities before storage costs grow out of control.</p>
<p><strong>Ask your agent:</strong></p>
<ul>
<li><em>Show me the largest files across all local repositories</em></li>
<li><em>Find artifacts in libs-release-local not downloaded in the last 3 months, larger than 1MB. Flag any SNAPSHOT buildup.</em></li>
</ul>
<p></p>
<h2>BYOA &#8211; Bring Your Own Agent: Your Agents, Your Way</h2>
<p>JFrog enables a BYOA approach by providing multiple secure and flexible ways to bring supply chain intelligence to your agents. Use our skills or MCP tools  individually or combine them, depending on what your team needs.</p>
<h3>JFrog’s Platform Skills Integrate into Your AI Eco-System</h3>
<p>We’ve created detailed skills that provide your coding agents deep, domain-specific knowledge of the JFrog Platform so you can simply ask what you need in natural language and the right action happens.</p>
<p>Skills are open source and can be installed into your preferred coding agents, including Cursor, Claude Code, or others with a single <a href="https://github.com/jfrog/jfrog-skills" rel="noopener" target="_blank">command</a>.</p>
<p>Every request is routed through the JFrog CLI or the expanding suite of MCP tools, ensuring that responses remain accurate, contextual, and fast.</p>
<h3>JFrog’s MCP Server</h3>
<p>If you prefer to access JFrog Platform capabilities via MCP, our <a href="https://jfrog.com/blog/introducing-jfrog-mcp-server/">MCP Server</a> is already available. Additional tools and capabilities are added to the JFrog MCP Server on an ongoing basis. Check our <a href="https://docs.jfrog.com/integrations/docs/jfrog-mcp-server-tools">docs</a> for the latest.</p>
<p>Enable in the JFrog Platform UI under: <strong>Platform</strong> → <strong>Integrations</strong> → <strong>Tools &amp; Integrations</strong> → <strong>MCP Server</strong> and follow the <a href="https://docs.jfrog.com/integrations/docs/enable-the-jfrog-mcp-server">docs</a> to get started.</p>
<h3>JFrog’s Plugin for Agents</h3>
<p>For teams that want plug-and-play simplicity, JFrog’s plugin for coding agents bundles JFrog’s Platform Skills and MCP tools into a zero-configuration package. Authenticate once via OAuth, and your agent is immediately aware of your supply chain, no CLI setup or token management required.</p>
<p>Coming soon for Claude Code, Cursor, VSCode with Copilot, and more coding agents. The plugin will also include native support for the <a href="https://jfrog.com/ai-catalog/mcp-registry">JFrog MCP Registry</a>, giving organizations the ability to discover, govern, and control which MCP servers are approved for use across their teams.</p>
<h2>What&#8217;s Coming Next</h2>
<p>Agentic DevSecOps is here. Agents are not just writing code, they are actively managing builds, enforcing governance, strengthening security, and optimizing your supply chain with full awareness of your environment.</p>
<p>New <a href="https://docs.jfrog.com/integrations/docs/jfrog-skills">Skills</a>, <a href="https://docs.jfrog.com/integrations/docs/jfrog-mcp-server-tools">MCP tools</a> and plugins ship every week, bringing more capabilities to your agents as the platform evolves.</p>
<p>Ready to see it in action? Join our upcoming webinar: <a href="https://leap.jfrog.com/WN-MoFu-Bin-26-05-Agentify-Your-DevSecOps-CS-AM-lp.html">Agentic DevSecOps Workflows with JFrog</a></p>
