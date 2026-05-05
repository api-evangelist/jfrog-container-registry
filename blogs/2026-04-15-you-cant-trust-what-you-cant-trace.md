---
title: "You Can’t Trust What You Can’t Trace"
url: "https://jfrog.com/blog/cant-trust-what-you-cant-trace/"
date: "Wed, 15 Apr 2026 13:14:54 +0000"
author: "drewt"
feed_url: "https://jfrog.com/blog/feed/"
---
<p><img alt="IDC White Paper - DevSecOps Modernization" class="alignnone wp-image-165500 size-full" height="301" src="https://media.jfrog.com/wp-content/uploads/2026/04/15142223/IDC-White-Paper-DevSecOps-Modernization_863x300-1-1.png" width="865" /></p>
<p>Picture this: Your security team finishes an AI vendor evaluation. The offering looks ironclad, with content filtering, output guardrails, and a stellar <a href="https://www.ibm.com/think/topics/red-teaming" rel="noopener" target="_blank">red-teaming</a> report. Everyone leaves the meeting satisfied, and another governance box is checked.</p>
<p>Six months later, a production incident hits. An AI agent, powered by a model your team &#8220;vetted,&#8221; starts executing unauthorized deletions in your CRM.</p>
<p>As the war room assembles, the gap reveals itself:</p>
<ul>
<li><strong>The Model</strong>: It was fine-tuned locally on customer data last quarter, but the approval was informal. No one can produce a lineage report.</li>
<li><strong>The <a href="https://jfrog.com/learn/ai-security/mcp-registry/">MCP</a></strong>: The model was granted &#8220;superpowers&#8221; via a Model Context Protocol connection to your production database, but the credentials were never rotated, and the connection has no owner.</li>
<li><strong>The Skills</strong>: The <code style="color: green;">skills.md</code> file governing the agent’s logic was updated by a developer three weeks ago to &#8220;move faster&#8221;, but it was never peer-reviewed.</li>
</ul>
<p>A monitoring alert was actually sent days ago, but because there was no defined escalation path for this specific &#8220;AI Asset,&#8221; it was ignored.</p>
<p><strong>The model itself was safe, yet the system could not be trusted.</strong></p>
<p>This distinction, between a safe model and a trusted, governed AI system, is the gap that is widening across the enterprise. It’s the difference between a chatbot that talks and an agent that acts &#8211; and unfortunately, it is the gap that most governance conversations fail to address.</p>
<h2>Why is Trust the Real Problem in AI Governance?</h2>
<p>When organizations talk about AI governance, they usually start with safety. Can the model be manipulated? Does it produce harmful outputs? Is it compliant with internal and industry policies? These are the real questions for AI development environments going forward, and, fortunately, progress is being made by AI providers in addressing these issues through improvements to their safety mechanisms and controls.</p>
<p>But safety is a property of the <strong>model</strong>, while trust is a property of the <strong>system</strong>.</p>
<p>An AI system with a reliable trust layer is one where, at any moment, you can determine ownership for all AI assets in production, including:</p>
<ul>
<li>Where it came from</li>
<li>Who approved it</li>
<li>What it was trained on</li>
<li>Who owns its behavior</li>
<li>What happens when something goes wrong</li>
</ul>
<p>Safety can tell you whether a model was built responsibly. Trust, however, tells you whether your organization can stand behind it or not.</p>
<p><a href="https://jfrog.com/software-supply-chain-state-of-union/">JFrog&#8217;s internal research</a> puts numbers to this gap. Surprisingly, over a third of companies still rely on manual efforts to maintain their lists of approved ML models. Unfortunately, this creates exactly the kind of inconsistency and uncertainty that makes trust impossible to sustain at scale. When securing ML models depends on someone remembering to update a spreadsheet &#8211; that’s not governance &#8211; it’s more like implementation based on the rock anthem “<a href="https://www.youtube.com/watch?v=lDK9QqIzhwk" rel="noopener" target="_blank">Living on a Prayer</a>.”</p>
<h2>The Shadow AI Problem Is a Trust Problem</h2>
<p>The gap becomes more acute when you factor in how AI actually enters an organization. It rarely starts with a formal evaluation but rather with a developer integrating a model from a public registry,  a data scientist fine-tuning datasets on a local machine, or a team connecting directly to an external API provider because it was faster than waiting for approval.</p>
<p>Risk is further compounded when AI agents are given &#8216;superpowers&#8217; via the Model Context Protocol (MCP) that grant them access to restricted systems to accomplish their predefined tasks. r. Without a trust layer, your organization may find itself with unvetted models executing unreviewed instructions with access to your most sensitive data.</p>
<p>Safety tells you if a model was built responsibly. Trust tells you if the logic pairing of agentic skills with specific tools and resources is putting your organization at risk or not.</p>
<p>This situation is also known as “<a href="https://jfrog.com/learn/ai-security/shadow-ai/">Shadow AI</a>”, where ungoverned AI assets can create dangerous blind spots around compliance, data exposure, and supply chain risk. Today,  it&#8217;s no longer a rare phenomenon, but is de-facto how most enterprise AI adoption actually begins and why it is so challenging to scale.</p>
<p>The reason Shadow AI is a trust problem, and not just a security problem, is what it tells you about the underlying system: There is no single source of truth for what AI is running in your organization. If you don&#8217;t know what&#8217;s there, you can&#8217;t govern it. If you can&#8217;t govern it, you can&#8217;t trust it. And if you can&#8217;t trust it, you might be putting your entire organization at risk.</p>
<p>The bottleneck for enterprise AI isn’t a lack of resources; it’s a lack of trust. Organizations see what AI can do, but aren&#8217;t yet confident they can control how it uses their data, what it reveals about their customers, and how to control access to their systems. This isn’t a technology gap &#8211; it’s a governance gap. Closing it requires moving beyond basic safety and establishing a trust layer built on three operational pillars: <strong>Security, Management, and Governance</strong>.</p>
<h2>Visibility Without Accountability Is Just Noise</h2>
<p>The knee-jerk reaction is to solve the trust gap by throwing more point solutions at it. While this might relieve some symptoms, it does not get to the root cause of the problem. Additional tools may provide more scanners, improved monitoring, and better dashboards, but no matter how many tools you add, they still do not result in a single source of truth.</p>
<p>Tooling creates visibility &#8211; accountability creates trust.</p>
<p>Governance fails not because tools are missing, but because accountability disappears between lifecycle stages and across complex, multi-tool integrations.</p>
<p>This is the same pattern many organizations experienced in the early days of application security, when at first, tools created visibility, but later on, <a href="https://jfrog.com/tool-consolidation/">adding more tools</a> just resulted in more confusion. Governance only becomes real once ownership is clearly defined across the AI delivery process. The rise of DevSecOps wasn&#8217;t a tooling shift. It was an ownership shift in how responsibility was embedded across the delivery lifecycle.</p>
<p>The same inflection point is here for AI. A drift alert with no escalation owner is just noise. An approved model list maintained manually is an illusion of control. A governance policy that lives in a document and not in the pipeline is a decoration.</p>
<p>Trusted AI requires accountability woven continuously into the lifecycle, not bolted on after deployment.</p>
<h2>What Does Trusted AI Actually Look Like?</h2>
<p>Trust isn&#8217;t a feeling; it’s a byproduct of <strong>Security, Management, and Governance</strong>. It is built through traceability of an AI asset&#8217;s origins, consistent enforcement of its use, and clear ownership of its behavior at every handoff.</p>
<p>That means treating AI models the same as the other artifacts in your software supply chain.  Only provenance tracked from source to production with automated security scans,  enforced policy gates prior to promotion, and a complete audit trail from code to production can provide a modicum of trust in the AI era.</p>
<p>It also means that Shadow AI must be replaced by real visibility and governance, not just manual guesswork.  In a truly secured and governed environment, the question of which model is running in production and who approved it must be answered in a matter of seconds &#8211; not hours.</p>
<h2>Trust Is the Real Competitive Advantage</h2>
<p>As AI adoption accelerates, the organizations that pull ahead won&#8217;t simply be the ones with the most capable models; they&#8217;ll be the ones who can utilize AI at scale without losing control.</p>
<p>That&#8217;s what Trusted AI means in practice. Not a claim on a vendor&#8217;s website, but a property of the system you&#8217;ve built around it &#8211; verifiable, auditable, and defensible under pressure.</p>
<p>This trust layer serves as a critical gatekeeper between the model and the enterprise, orchestrating model integrity, enforcing granular policies, and generating the immutable evidence required for true AI governance. That&#8217;s where trust is either built or quietly undermined, one ungoverned model at a time.</p>
<p><strong>Model safety is a feature. Trusted AI is the system built around it.</strong></p>
<h2>How JFrog Helps Build Trust Into Your AI Lifecycle</h2>
<p>Safety is just the starting point. To truly scale AI, you need a system that ensures every model is traceable, secure, and governed from development to production.</p>
<p>Don’t let Shadow AI cause gaps in your governance. <a href="https://jfrog.com/ai-catalog/">Explore the JFrog AI Catalog</a> to see how you can manage and secure AI assets in your software supply chain, ensuring total visibility and accountability at every stage.</p>
<p>Ready to build a defensible AI strategy? <a href="https://jfrog.com/whitepaper/devops-from-code-to-compliance-2026-guide/">Download the 2026 Trusted AI Playbook</a> or <a href="https://jfrog.com/platform/schedule-a-demo/">Schedule a Demo of the JFrog Platform</a> today.</p>
