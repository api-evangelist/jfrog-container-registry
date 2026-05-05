---
title: "Automate NIST SSDF Compliance: A Technical Guide to Policy as Code in JFrog AppTrust"
url: "https://jfrog.com/blog/automate-nist-ssdf-compliance-with-jfrog-apptrust/"
date: "Thu, 16 Apr 2026 13:15:07 +0000"
author: "drewt"
feed_url: "https://jfrog.com/blog/feed/"
---
<p><img alt="Automate NIST SSDF Compliance_863x300" class="alignnone wp-image-165567 size-full" height="300" src="https://media.jfrog.com/wp-content/uploads/2026/04/16142221/Automate-NIST-SSDF-Compliance_863x300.png" width="863" /></p>
<p>For many engineering and security teams, <a href="https://csrc.nist.gov/projects/ssdf" rel="noopener" target="_blank">NIST SP 800-218</a> (Secure Software Development Framework, or SSDF) compliance feels like a hurdle that is too difficult to overcome. To meet these and other emerging regulations and be effective in today’s DevSecOps environment, organizations are moving toward codifying these standards into machine-readable rules, also known as <a href="https://jfrog.com/blog/governance-that-ships-embedding-policy-as-code-into-your-system-of-record/?_gl=1*1ct82do*_up*MQ..*_ga*MTM1MzE1OTM1MS4xNzc2MjY5MTY4*_ga_SQ1NR9VTFJ*czE3NzYyNjkxNjckbzEkZzAkdDE3NzYyNjkxNjckajYwJGwwJGg4NjM4NDQyMTI.">Policy as Code (PaC)</a>.</p>
<p>Most tools force you into closed-set, rigid policies that are difficult to adapt to your specific software development needs. By using Rego, the industry-standard language for the Open Policy Agent (OPA) engine within <a href="https://jfrog.com/apptrust/">JFrog AppTrust</a>, you gain the precise control necessary to natively enforce any custom governance rule without compromising how your organization’s software development workflows.</p>
<h2>How Do You Map NIST SSDF Requirements to JFrog AppTrust?</h2>
<p>NIST (The U.S. National Institute of Standards and Technology) designed the SSDF framework around four pillars:</p>
<ul>
<li>PO &#8211; Preparing the Organization</li>
<li>PS &#8211; Protecting the Software</li>
<li>PW &#8211; Producing Well-Secured Software</li>
<li>RV &#8211; Responding to Vulnerabilities</li>
</ul>
<p>JFrog products integrate directly into these pillars to provide automated evidence for compliance. The following table demonstrates how AppTrust specifically addresses critical NIST SSDF requirements through automated integrations.</p>
<table>
<tbody>
<tr>
<td><strong>NIST Requirement</strong></td>
<td><strong>Objective</strong></td>
<td><strong>JFrog AppTrust Integration</strong></td>
</tr>
<tr>
<td>PS.3.2</td>
<td>Validating <a href="https://jfrog.com/learn/grc/sbom/">SBOM</a> Attestation</td>
<td>Integrates with <a href="http://jfrog.com/xray">JFrog Xray</a> for deep-scan SBOM evidence, facilitating fast high fidelity validation.</td>
</tr>
<tr>
<td>PW.5.1</td>
<td>Validating Secure Coding Practices</td>
<td>Verifies attestations from ecosystem partners like <a href="https://jfrog.com/integrations/sonar/">SonarQube</a> or Troj.ai, enabling you to demonstrate the use of multiple tools to prove your secure coding model.</td>
</tr>
<tr>
<td>PW.2.2</td>
<td>Automating Release Approvals</td>
<td>Automates <a href="https://jfrog.com/jfrog-and-servicenow/">ServiceNow</a> Change-Request approval checks, enabling faster consistent automated workflows.</td>
</tr>
<tr>
<td>PW.6</td>
<td>Hardening and Build Process Security</td>
<td>Validates binary-level analysis from JFrog Advanced Security and <a href="https://jfrog.com/help/r/jfrog-and-github-integration-guide/jfrog-evidence-management-with-github-actions">GitHub</a> provenance, demonstrating the consistent application of security best practices.</td>
</tr>
</tbody>
</table>
<p>&nbsp;</p>
<p>With this capability, you can translate complex NIST SSDF requirements into logic that is stored along with your software. Whether you need to enforce two-person code reviews (NIST PW.6.2) or verify signed build provenance (NIST PS.3.1), you can now create a script for these rules in Rego and let JFrog AppTrust handle the enforcement.</p>
<h2>How Do You Automate NIST SSDF with JFrog AppTrust?</h2>
<p>Translating frameworks into code requires precise execution. Below, we break down the exact Rego code you can use in JFrog AppTrust to automate your compliance today.</p>
<h4>Step 1: Validating SBOM Attestation (PS.3.2)</h4>
<p>NIST requirement PS.3.2 mandates &#8220;collecting, maintaining, and sharing provenance data for all components,&#8221; which teams typically achieve using a Software Bill of Materials (SBOM). JFrog AppTrust uses deep-scan SBOMs created automatically by JFrog Xray with new releases to provide a Single Point of Truth.</p>
<pre class="language-yaml"><code>
package curation.policies
    import rego.v1
default should_allow := false
release_evidence := [evidence | some evidence in input.data.evidenceConnection[_]]

# PaC: Enforce SBOM Evidence (NIST PS.3.2)
should_allow := true if {
# Check Xray SBOM evidence existence
	some evidence in release_evidence
     evidence.node.predicateType == "https://jfrog.com/evidence/cyclonedx/sbom/v1.6"
}
explanation := "NIST Violation (PS.3.2): SBOM evidence is required for provenance data compliance. No SBOM evidence found from Xray." if {
	should_allow == false
}

allow := {
    "should_allow": should_allow,
    "explanation": explanation
}
</code></pre>
<p>&nbsp;</p>
<h4>Step 2: Validating Secure Coding Practices (PW.5.1)</h4>
<p>Under PW.5.1, you must prove automated security testing was performed to find vulnerabilities. JFrog AppTrust allows you to ingest and verify <a href="https://jfrog.com/evidence/">evidence</a> from ecosystem partners like SonarQube or Troj.ai, ensuring that no code reaches production without a passing security scan.</p>
<pre class="language-yaml"><code>
package curation.policies
import rego.v1
default should_allow := false
#artifact_evidence := [evidence | some evidence in input.data.artifactsConnection[_].node.evidenceConnection[_]]
artifact_evidence := [evidence.node.evidenceConnection.edges[_] | some evidence in input.data.artifactsConnection.edges]
    
# PaC: Ecosystem Attestation Check (NIST PW.5.1)
should_allow := true if {
    # AppTrust checks for attestations from partner tools (SonarQube)
some evidence in artifact_evidence
     evidence.node.predicateType == "https://sonar.com/evidence/sonarqube/v1"
}

explanation := "NIST Violation (PW.5.1): Verified SonarQube analysis results are missing." if {
	should_allow == false
}
allow := {
    "should_allow": should_allow,
    "explanation": explanation
}
</code></pre>
<p>&nbsp;</p>
<h4>Step 3: Automating Release Approvals (PW.2.2)</h4>
<p>Process-based evidence, such as change management approvals, is often the hardest to track. To satisfy PW.2.2, JFrog AppTrust merges technical results with business processes by automating a check to ensure a ServiceNow Change-Request approval was included in the release bundle. Note: JFrog AppTrust can also trigger workflows to retrieve decisions from ServiceNow automatically when they are made.</p>
<pre class="language-yaml"><code>
package curation.policies
import rego.v1
default should_allow := false
release_evidence := [evidence | some evidence in input.data.evidenceConnection[_]]

# PaC: ServiceNow Process Enforcement
should_allow := true if {
# Check if ServiceNow Change-Request approval evidence was 
	some evidence in release_evidence
     evidence.node.predicateType == "https://servicenow.com/change-approval/v1"
}
explanation := "Governance Block: Final release requires a ServiceNow evidence." if {
should_allow == false
}
allow := {
"should_allow": should_allow,
"explanation": explanation
}
</code></pre>
<h4></h4>
<h4>Step 4: Hardening and Build Process Security (PW.6)</h4>
<p>Requirement PW.6 is about ensuring the build process uses security flags like ASLR and DEP. JFrog Advanced Security performs binary-level analysis to check if the security features mentioned in PW.6 &#8211; such as stack canaries, NX bits, and ASLR &#8211; are actually enabled in the final executable, and you can use these checks as a control gate in JFrog AppTrust.</p>
<p>By combining GitHub Provenance, you create a powerful &#8220;Code-to-Binary&#8221; chain of trust where GitHub provides how the code was built (PS.3.2), while JFrog proves the resulting binary is secure and hardened (PW.6).</p>
<p>To satisfy NIST requirements, you must not only verify that a provenance file exists, but also verify it is signed by the builder and that the builder ID, repository, and git branch match your expectations.</p>
<pre class="language-yaml"><code>
package curation.policies
    import rego.v1
    release := input.data
    release_evidence := [evidence | some evidence in release.evidenceConnection[_]]
    artifact_evidence := [evidence.node.evidenceConnection.edges[_] | some evidence in release.artifactsConnection.edges]
    build_evidence := [evidence.evidenceConnection.edges[_] | some evidence in release.fromBuilds]
    all_layers_evidences := array.concat(release_evidence, array.concat(artifact_evidence, build_evidence))
    
    default exists := false
    
    exists if {
        some predicate in all_layers_evidences        
        startswith(predicate.node.predicateType, input.params.predicateType)
        startswith(predicate.node.predicate.runDetails.builder.id, input.params.GitHubOrg)
        predicate.node.predicate.buildDefinition.internalParameters.github.runner_environment == "github-hosted"
        predicate.node.predicate.buildDefinition.externalParameters.workflow.ref == "refs/heads/main"
        some dependency in predicate.node.predicate.buildDefinition.resolvedDependencies
        startswith(dependency.uri, input.params.codeRepo)
        endswith(dependency.uri, "@refs/heads/main")
    }
        
    allow := {"should_allow": exists}
</code></pre>
<h2>What are the Benefits of Evidence-Based Governance?</h2>
<p>Today’s software supply chains move too fast for manual audits, necessitating a shift toward a system where security is integrated into the binary itself. Transitioning to evidence-based governance provides the granular control required to meet the stringent demands of NIST SSDF framework without stalling development, including:</p>
<ul>
<li><strong>Physical Enforcement</strong>: If your specific Rego conditions are not met, releases are physically blocked from promotion.</li>
<li><strong>Self-Evident Integrity</strong>: You no longer have to hope the process was followed; the artifact carries its own evidence of integrity.</li>
<li><strong>Proactive Security Posture</strong>: Your organization moves from a reactive, manual posture to automated governance.</li>
</ul>
<p>By natively embedding these controls into your existing workflows, you eliminate the friction between security and velocity. This architectural shift ensures that your software supply chain is not only compliant by design but also provides documented evidence to comply with emerging industry and governmental regulations.</p>
<h2>How JFrog Automates and Simplifies Compliance</h2>
<p>JFrog AppTrust moves you away from brittle DIY scripts and pre-defined inflexible templates toward a system that immutably binds evidence from your entire ecosystem to a build in <a href="http://jfrog.com/artifactory">JFrog Artifactory</a>. With JFrog Artifactory as your System of Record, this proof stays with the release as it moves from development to staging to production giving you an audit trail that proves exactly what was built, how it was validated, and why it can be trusted for production.</p>
<p>By moving to a <a href="https://jfrog.com/learn/grc/devgovops/">DevGovOps</a> model with JFrog AppTrust, you realize two critical benefits:</p>
<ol>
<li><strong>Acceleration of  development velocity</strong> by removing “stop-and-search” delays</li>
<li><strong>Establishing immutable trust</strong> at the artifact, build and release levels.</li>
</ol>
<p>With JFrog as the trust layer across your software supply chain, achieving NIST SSDF compliance is no longer a monumental task; it is the natural outcome of a properly governed, automated platform.</p>
<p>Ready to automate NIST SSDF compliance? <a href="https://jfrog.com/platform/schedule-a-demo/">Contact us for a technical deep dive</a> to see how JFrog AppTrust enforces Policy-as-Code and binds verifiable compliance evidence directly to every release.</p>
