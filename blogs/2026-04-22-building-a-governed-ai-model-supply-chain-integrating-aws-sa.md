---
title: "Building a Governed AI Model Supply Chain: Integrating AWS SageMaker and the JFrog Platform"
url: "https://jfrog.com/blog/bulding-a-goverened-ai-supply-chain-with-jfrog-and-sagemaker/"
date: "Wed, 22 Apr 2026 13:16:07 +0000"
author: "drewt"
feed_url: "https://jfrog.com/blog/feed/"
---
<p><img alt="AI Model Governance with SageMaker and JFrog - 863x300" class="alignnone wp-image-165870 size-full" height="300" src="https://media.jfrog.com/wp-content/uploads/2026/04/21161413/AI-Model-Governance-with-SageMaker-and-JFrog-863x300-1.png" width="863" /></p>
<p>Amazon SageMaker accelerates the process of training and deploying machine learning models. However, as AI adoption scales from individual experiments to enterprise-wide production, the focus of leading Fortune 500 software development operations and security teams must shift from pure velocity to governance.</p>
<p>The question is no longer just &#8220;Can we ship this model?&#8221; but &#8220;How do we manage the lifecycle of what we ship?&#8221;</p>
<p>If your organization struggles with versioning, access control, and multi-environment promotion, your current storage setup might be holding you back. This post explores how integrating SageMaker’s infrastructure capabilities with <a href="https://jfrog.com/artifactory/">JFrog Artifactory</a> creates a governed, auditable, and <a href="https://jfrog.com/ai-catalog/">secure AI supply chain</a>.</p>
<h2>The Tension Between Speed and Governance</h2>
<p>In a typical AI model development project, there is a disconnect between how things are built and how they are secured.</p>
<ul>
<li><strong>The Developer View</strong>: You need to pull a Llama-3 model from Hugging Face, run a SageMaker training job on a p4d instance, and see if the weights improve. You don&#8217;t want to wait for a security ticket to download a model or install a library.</li>
<li><strong>The Platform View</strong>: Security teams need to know exactly what is inside that model. If you’re pulling weights from a public hub and combining them with internal data, you’re creating an artifact that needs <a href="https://docs.jfrog.com/governance/docs/evidence-management">documented attestation</a>.</li>
</ul>
<p>When teams try to bridge this gap using basic cloud storage, infrastructure problems emerge. Nevertheless, both operations and security agree that the main goal is to enable the developer to work at full speed while the platform automatically captures the audit trail in the background.</p>
<h3>Why S3 Might Not Be the Best Choice for AI Artifact Management</h3>
<p>Amazon S3, a high-performance, scalable, and secure object storage service, is excellent for high-durability storage, but it is essentially a &#8220;<a href="https://en.wikipedia.org/wiki/Bit_bucket" rel="noopener" target="_blank">bit bucket</a>.&#8221; In a standard SageMaker workflow, S3 stores your model.tar.gz. The problem arises when you try to scale that into an enterprise model registry.</p>
<ol>
<li><strong>Object Awareness</strong>: To S3, a model weights file and a raw log file are the same thing: bytes. It doesn&#8217;t understand that a model has a relationship to a specific dataset or a training container.</li>
<li><strong>Metadata Limits</strong>: S3 tags are fine for billing, but they aren&#8217;t built for querying AI governance-specific questions like: &#8220;Which models used this specific version of a dataset that we just found has a data-poisoning issue?&#8221;</li>
<li><strong>The Promotion Problem</strong>: In software, you don&#8217;t just &#8220;save&#8221; code to production; you promote it from Dev to Staging to Prod after it passes tests. S3 doesn&#8217;t have a native concept of &#8220;promotion.&#8221; You end up manually moving files between buckets, which breaks the versioning chain.</li>
</ol>
<h3>The Fragmentation Problem</h3>
<p>A production model serving an endpoint isn&#8217;t just one file. It&#8217;s a &#8220;triad&#8221; of moving parts, including:</p>
<ol>
<li><strong>Weights</strong> (usually in S3)</li>
<li><strong>Inference Code/Runtime</strong> (usually a Docker image in ECR)</li>
<li><strong>Dependencies</strong> (libraries like torch or transformers pulled from PyPI)</li>
</ol>
<p>Because these are spread across three different AWS services, your audit trail is fragmented. You have an SBOM for the container and a version for the weights, but no single &#8220;record of truth&#8221; linking them.</p>
<p>By centralizing the <strong>Model Triad</strong>, weights, inference code, and environment dependencies in JFrog Artifactory, you transition from simple file storage to a <strong>Production Deployment Graph</strong>. In a traditional S3-based workflow, these components are scattered across different services, making it nearly impossible to prove exactly which version of code ran which version of a model. By consolidating them into a single registry, you aren&#8217;t just managing files; you&#8217;re managing the immutable link between the model and the specific environment that runs it. This allows you to generate a combined, synchronized <strong>SBOM</strong> and <strong>AIBOM</strong> from one single source of truth.</p>
<h3>The Multi-Environment Reality</h3>
<p>Finally, very few organizations operate in a single unified cloud computing environment. Whether through mergers, hybrid-cloud requirements, or edge deployment needs, teams often find themselves managing models across multiple environments, such as:</p>
<ul>
<li><strong>Hybrid Cloud</strong>: Training on SageMaker GPUs but deploying to on-premises Kubernetes.</li>
<li><strong>Multi-Cloud</strong>: Consolidating model registries after an acquisition across AWS, GCP, and Azure.</li>
<li><strong>Edge Deployment</strong>: Managing model updates across thousands of IoT devices via AWS Greengrass or NVIDIA Jetson.</li>
</ul>
<p>Centralizing the &#8220;Source of Truth&#8221; in a platform-agnostic registry ensures that no matter where the artifact is used, the <strong>governance</strong> remains consistent.</p>
<h2>The JFrog Platform as the AI Unified Governance Layer</h2>
<p>To scale AI successfully, you must stop treating models differently from software. Integrating the <a href="https://jfrog.com/platform/">JFrog Platform</a> with SageMaker gives you a unified workflow. Data Scientists keep their velocity in SageMaker, while DevSecOps teams gain an auditable, scanned single source of truth.</p>
<p>This is based on three core capabilities:</p>
<p style="padding-left: 40px;">1. <strong>Centralized Model Registry</strong></p>
<p style="padding-left: 40px;">Instead of scattered buckets, JFrog Artifactory acts as your central model registry. It manages model artifacts with immutable versioning and structured environment separation. When a data scientist finishes a SageMaker training job, they push the model to Artifactory, attaching rich metadata, such as hyperparameters and training metrics, directly to the artifact.</p>
<p style="padding-left: 40px;">2. <strong>Curated Proxying for Hugging Face</strong></p>
<p style="padding-left: 40px;">Public repositories update constantly. <a href="https://jfrog.com/curation/">JFrog Curation</a> acts as a secure gateway and remote repository for Hugging Face teams to consume approved upstream models with built-in caching. If Hugging Face experiences an outage, your SageMaker training jobs continue running because JFrog cached the model locally.</p>
<p style="padding-left: 40px;">3. <strong>Consolidated Security for SBOMs &amp; AIBOMs</strong></p>
<p style="padding-left: 40px;"><a href="https://jfrog.com/xray">JFrog Xray </a>automatically generates a Software Bill of Materials (<a href="https://jfrog.com/learn/grc/sbom/">SBOM</a>) for the container and an AI Bill of Materials (<a href="https://jfrog.com/learn/ai-security/ai-bom/">AIBOM)</a> for the model. Together, they provide a single, auditable record of the entire supply chain, blocking malicious models before they ever reach your production environment.</p>
<p><img alt="SageMaker and JFrog MLRegistry - Diagram" class="alignnone wp-image-165864 size-large" height="837" src="https://media.jfrog.com/wp-content/uploads/2026/04/21153823/SageMaker-and-JFrog-MLRegistry-Diagram-1024x837.png" width="1024" /></p>
<h3>Integration Walkthrough &#8211; Orchestration, Training &amp; Inference</h3>
<p><em><strong>Note</strong>: This excerpt is taken from a hands-on walkthrough that includes the complete implementation code. Please refer to the<a href="https://github.com/jfrog/JFrogMLExamples/tree/main/sagemaker" rel="noopener" target="_blank"> JFrogML Examples GitHub repository</a> for details.</em></p>
<p>To begin with,  let’s focus on two core patterns:</p>
<ul>
<li><strong>Centralized Model Registry</strong>: Using Artifactory to manage model artifacts with immutable versioning and structured environment separation.</li>
<li><strong>Curated Proxying</strong>: Leveraging JFrog as a secure gateway for Hugging Face, enabling teams to consume approved upstream models with built-in caching and repeatability.</li>
</ul>
<p>With that in mind, the integration is divided into three stages:</p>
<ul>
<li><strong>Unified Model Orchestration</strong> &#8211; setting up environment configuration and externalizing secrets so that credentials are never hardcoded in your notebook or pipeline.</li>
<li> <strong>Secure Model Lineage &amp; Provenance</strong> &#8211; where the base model is pulled through Artifactory rather than directly from Hugging Face, and the resulting fine-tuned weights are registered directly to Artifactory upon completion.</li>
<li><strong>Dynamic Runtime Model Resolution</strong> &#8211; where the deployment endpoint dynamically loads a specific versioned model from Artifactory at runtime rather than pointing to a static S3 artifact.</li>
</ul>
<h4>1. Unified Model Orchestration</h4>
<p>The workflow originates within a SageMaker environment, utilizing Secret Externalization to eliminate the risk of hardcoded credentials. By referencing AWS Secrets Manager IDs as environment variables, we establish a secure &#8216;contract&#8217; that allows the notebook to authenticate dynamically during the training or inference execution.</p>
<table>
<tbody>
<tr>
<td>
<pre class="language-yaml"><code># In your SageMaker Notebook / Orchestrator
env = {
    # Credentials resolved in-container via AWS Secrets Manager
    "HF_TOKEN_SECRET_ID": "jfrog/hf_token",
    "JF_ACCESS_TOKEN_SECRET_ID": "jfrog/jf_token",

    # JFrog Artifactory target configuration
    "JF_URL": "https://.jfrog.io",
    "JF_REPO": "ml-models-dev-local",

    # Proxy Hugging Face through Artifactory for caching &amp; scanning
    "HF_ENDPOINT": "https://.jfrog.io/artifactory/api/huggingfaceml/hf-remote",
}
</code></pre>
</td>
</tr>
</tbody>
</table>
<h4>2. Secure Model Lineage &amp; Provenance</h4>
<p>Inside the train.py script, the integration re-routes standard Hugging Face calls to Artifactory and uses the FrogML SDK to handle the output based on:</p>
<p><strong>Proxied Base Model Download</strong><br />
By setting the HF_ENDPOINT environment variable, the Hugging Face snapshot_download logic is redirected. Instead of hitting the public Hugging Face Hub, the training job pulls the base model through an Artifactory Remote Repository. This ensures the weights are cached locally and scanned for vulnerabilities before they reach the SageMaker GPU instance.</p>
<p><strong>Model weights registration via SDK</strong><br />
Once fine-tuning is complete, the script uses frogml.log_model. This function serializes the model weights from the container&#8217;s local memory/filesystem and pushes them directly to an Artifactory Local Repository. This bypasses SageMaker’s default behavior of depositing a generic .tar.gz into S3, ensuring the model is immediately versioned and metadata-tagged in your registry.</p>
<table>
<tbody>
<tr>
<td>
<pre class="language-yaml"><code>
# training/train.py

# 1. Resolve secrets and configure environment
os.environ["HF_TOKEN"] = _get_secret_value(os.environ["HF_TOKEN_SECRET_ID"])
os.environ["JF_ACCESS_TOKEN"] = _get_secret_value(os.environ["JF_ACCESS_TOKEN_SECRET_ID"])

# 2. Download foundation model through the Artifactory Proxy
# This ensures a scanned, cached base model is used for fine-tuning
local_path = snapshot_download(repo_id=config.MODEL_ID)

# ... [Standard SageMaker Training / SFTTrainer Logic] ...

# 3. Serialize and Log to Artifactory
# This captures metrics, hyperparameters, and weights in one governed bundle
frogml.huggingface.log_model(
    model=model_to_log,
    tokenizer=tokenizer,
    repository=os.environ["JF_REPO"], 
    model_name=config.JF_MODEL_NAME,
    parameters=params_to_log,
    metrics=final_metrics,
)
</code></pre>
</td>
</tr>
</tbody>
</table>
<h4>3. Dynamic Runtime Model Resolution</h4>
<p>In a standard deployment, an endpoint often points to a static S3 URI. In our governed model, we replace that with a dynamic call to Artifactory using the InferenceSpec. This guarantees that the endpoint loads the exact, immutable version of the weights that was promoted, approved, and is currently running in end-user environments.</p>
<table>
<tbody>
<tr>
<td>
<pre class="language-yaml"><code>
# inference.py (simplified)

class DevopsAssistantInferenceSpec(InferenceSpec):
    def load(self, model_dir: str):
        # 1. Resolve credentials at runtime via AWS Secrets Manager
        jf_token_secret_id = self._get_secret_id("JF_ACCESS_TOKEN_SECRET_ID")
        os.environ["JF_ACCESS_TOKEN"] = self._get_secret_value(jf_token_secret_id)

        # 2. Retrieve the specific, immutable version from Artifactory
        # This version is typically passed as an environment variable (MODEL_VERSION)
        model, tokenizer = frogml.huggingface.load_model(
            model_name=config.JF_MODEL_NAME,
            repository=config.JF_REPO,
            version=self.model_version,
        )
        
        return {"generator": pipeline("text-generation", model=model, tokenizer=tokenizer)}
</code></pre>
</td>
</tr>
</tbody>
</table>
<h2>Unified Governance of Software Application and AI Artifacts with JFrog</h2>
<p>Amazon SageMaker is an exceptional platform for experimentation, training, and managed inference. JFrog Artifactory complements it by making model consumption and distribution easy to control at scale.</p>
<p>Ultimately, this integration resolves the trade-off between agility and auditability: AI Engineers maintain the velocity required for innovation, while Operations and DevSecOps teams gain the comprehensive lifecycle control and visibility necessary for enterprise-grade quality and security.</p>
<p>This approach is an especially good fit for organizations that:</p>
<ul>
<li>Support <strong>multiple AI teams</strong> and need a consistent way to publish and reuse models</li>
<li>Operate under <strong>security/compliance requirements</strong> and want tighter control over what enters the environment</li>
<li>Run across <strong>multiple clouds, regions, or platforms</strong>, and need a reliable system of record for model artifacts</li>
</ul>
<p>To try it yourself, start with the implementation examples in the <a href="https://github.com/jfrog/JFrogMLExamples/tree/main/sagemaker" rel="noopener" target="_blank">JFrog ML Examples</a> GitHub repo. For the framework-specific integration used in this guide, see the <a href="https://docs.jfrog.com/artifactory/docs/machine-learning-repositories" rel="noopener" target="_blank">FrogML documentation</a>.</p>
<p>For a personalized demonstration, please <a href="https://jfrog.com/platform/schedule-a-demo/?_gl=1*k9iu2s*_up*MQ..*_ga*Mzg3OTk2MDc2LjE3NzY2MDU0NzM.*_ga_SQ1NR9VTFJ*czE3NzY2MDgwMjYkbzIkZzAkdDE3NzY2MDgwMzgkajQ4JGwwJGgyMTI5MjA2NzY0">contact our team</a> at your convenience.</p>
