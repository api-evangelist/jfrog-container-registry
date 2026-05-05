---
title: "AzureML and JFrog: Securing the Model Lifecycle"
url: "https://jfrog.com/blog/azureml-and-jfrog-securing-the-model-lifecycle/"
date: "Fri, 13 Mar 2026 22:48:33 +0000"
author: "zoer"
feed_url: "https://jfrog.com/blog/feed/"
---
<p><img alt="AzureML Integration" class="aligncenter wp-image-163982 size-full" height="300" src="https://media.jfrog.com/wp-content/uploads/2026/03/14002714/863-x-300-1.png" width="863" /></p>
<p><a href="https://azure.microsoft.com/en-us/products/machine-learning" rel="noopener" target="_blank">Azure Machine Learning</a> (AzureML) is a powerhouse for model experimentation and high-scale compute. However, for most organizations, the challenge isn’t building models; it’s the complex journey from a notebook to a secure, governed, and production-ready application.</p>
<p>When models and dependencies reside in unmanaged silos, you lose the traceability required for production. This fragmentation creates <a href="https://jfrog.com/blog/how-to-detect-and-eliminate-shadow-ai-in-5-steps/">Shadow AI</a> risks: unvetted Python packages, lack of version control, and models that might be malicious, non-compliant, or cannot be audited. To move from experimentation to enterprise execution, you need a secure, traceable path from the first line of training code to the final production model.</p>
<h2>The Problem: When AI and Software Development Exist in Silos</h2>
<p>Most organizations treat AI development as separate from their standard software development pipelines. AI/ML teams often pull packages and open-source models directly from public repositories (like Hugging Face) and store trained models in basic cloud storage buckets.</p>
<p>While convenient, these alternatives lack essential enterprise rigor:</p>
<ul>
<li><strong>Security blind spots:</strong> Public packages often contain vulnerabilities or malicious code that bypasses standard CI/CD checks.</li>
<li><strong>Lack of provenance: </strong>Cloud buckets do not inherently provide the immutability or versioning needed for reliable rollbacks or &#8220;day two&#8221; operations.</li>
<li><strong>Compliance gaps: </strong>Without an AI Bill of Materials (AI-BOM), you cannot prove the origin or safety of a model during a regulatory audit.</li>
</ul>
<p>To bridge this gap, the <a href="https://jfrog.com/artifactory/">JFrog Software Supply Chain Platform</a> acts as a trusted &#8220;AI Registry&#8221; for AzureML. By treating AI assets as standard software artifacts, you gain the security of a unified supply chain without slowing down experimentation.</p>
<p>In this guide, we’ll walk through a 4-step workflow to combine AzureML’s compute power with the security scanning capabilities to create a production-ready AI pipeline.</p>
<p><img alt="JFrog x Microsoft Partner" class="aligncenter wp-image-165364 size-full" height="250" src="https://media.jfrog.com/wp-content/uploads/2026/03/07204210/image2-1.png" width="719" /></p>
<h2>A 4-Step Guide to a Governed AI Pipeline</h2>
<p>To make this integration practical, we’ll follow the lifecycle of a model with AzureML and JFrog from initial build to production deployment. This 4-step guide ensures that every binary and dependency is scanned, versioned, and immutable.</p>
<p><img alt="AzureML Integration with the JFrog Platform" class="aligncenter wp-image-163979 size-large" height="512" src="https://media.jfrog.com/wp-content/uploads/2026/03/14002349/image3-1024x512.png" width="1024" /></p>
<h3>Step 1: Secure the Build Foundation</h3>
<p>The security of an AI model starts with its dependencies. Instead of pulling Python packages directly from public repositories, which exposes you to supply chain risks, you route all requests through the JFrog Platform. This creates a secure proxy that caches and scans every package before it ever touches your training environment.</p>
<p><strong>In the Build phase</strong>, add the following to your <code>pip.conf</code> file to ensure all your dependencies and base packages are safely pulled from Artifactory (replace with your actual values):</p>
<pre style="background-color: white; padding: 0px 10px 0px 10px;"><code> 
[global]

index-url = https://&lt;username&gt;:&lt;access-token&gt;@mycompany.jfrog.io/artifactory/api/pypi/pypi-virtual/simple

trusted-host = mycompany.jfrog.io

</code></pre>
<p>Continue to build your training image as usual, pushing it to Artifactory when done:</p>
<pre class="language-bash" style="background-color: black; padding: 0px 10px 0px 10px;"><code> 
# Enable BuildKit
export DOCKER_BUILDKIT=1

# Set variables
export ARTIFACTORY_HOST=mycompany.jfrog.io
export ARTIFACTORY_DOCKER_REPO=docker-virtual
export TAG=1.0.0

# Login to Artifactory Docker registry
docker login ${ARTIFACTORY_HOST}
# Enter your JFrog username and access token when prompted

# Build and push
docker build \
  --platform linux/amd64 \
  -t ${ARTIFACTORY_HOST}/${ARTIFACTORY_DOCKER_REPO}/azureml-training:${TAG} \
  -f docker/Dockerfile \
  --secret id=pipconfig,src=${PIP_CONFIG_FILE} \
  --build-arg BASE_IMAGE="${ARTIFACTORY_HOST}/${ARTIFACTORY_DOCKER_REPO}/python:3.11-slim" \
  --push \
  .
</code></pre>
<p>&nbsp;</p>
<h3>Step 2: Trusted Training in AzureML</h3>
<p>With your environment secured, you now instruct AzureML to run the Docker image, pulling it from Artifactory.</p>
<p><strong>In the Train phase</strong>, create a connection in your training pipeline like so:</p>
<pre class="language-python" style="background-color: black; padding: 0px 10px 0px 10px;"><code> 
credentials = UsernamePasswordConfiguration(username=username, password=access_token) # Store credentials in KeyVault and rotate for security
 
ws_connection = WorkspaceConnection(
  name="JFrogArtifactory",
  target=,
  type="GenericContainerRegistry",
  credentials=credentials
)

env = Environment(
  image=docker_image # Training image will be pulled from the connection (Artifactory) 
)

</code></pre>
<p><img alt="" class="aligncenter wp-image-163980 size-large" height="559" src="https://media.jfrog.com/wp-content/uploads/2026/03/14002518/image2-1024x559.png" width="1024" /></p>
<h3>Step 3: Centralize Models in Artifactory</h3>
<p>Once training is complete, the final model must enter your secure software supply chain. <strong>In the Upload phase</strong>, using the <a href="https://jfrog.com/blog/frogml-sdk-the-gateway-to-model-governance/">frogml SDK</a>:</p>
<pre class="language-python" style="background-color: black; padding: 0px 10px 0px 10px;"><code> 
import frogml

frogml.files.log_model(
  source_path=model_path, # resulting model path from the training
  repository=ml_repo_name, # Artifactory repository where the model will be stored
  model_name=model_name,
  version=version,
  properties=properties, # key value pairs representing model metadata
  dependencies=dependencies,
  code_dir=code_dir
)
</code></pre>
<p>By logging the model to the JFrog Platform, you create an immutable record. Unlike loose cloud storage, the platform ensures that your model version remains exactly the same from the moment it leaves the training cluster to the moment it hits production.</p>
<p><img alt="" class="aligncenter wp-image-163981 size-large" height="618" src="https://media.jfrog.com/wp-content/uploads/2026/03/14002556/image1-1024x618.png" width="1024" /></p>
<h3>Step 4: Immutable Deployment</h3>
<p>Finally, to <strong>deploy</strong> the model, download the model required version from the Artifactory Machine Learning repository:</p>
<pre class="language-python" style="background-color: black; padding: 0px 10px 0px 10px;"><code> 
frogml.files.load_model(
  repository=ml_repo_name,
  model_name=model_name,
  version=version,
  target_path=target_path
)
</code></pre>
<p>Check out <a href="https://github.com/jfrog/JFrog-AzureML-integration" rel="noopener" target="_blank">GitHub for a full example</a> of the model lifecycle with AzureML and JFrog.</p>
<h2>Key Takeaways: Security by Design</h2>
<p>By treating AI models as primary citizens within the JFrog Platform, you eliminate the friction between software and AI development.</p>
<ul>
<li><strong>Full reproducibility:</strong> You know exactly which libraries, base images, and datasets went into every model version.</li>
<li><strong>Integrated security:</strong> Models are continuously and automatically scanned for vulnerabilities and license compliance throughout their entire lifecycle.</li>
<li><strong>Unified governed supply chain:</strong> <a href="https://jfrog.com/ai-catalog/">Manage AI models</a> under the same RBAC and audit policies as your standard software applications.</li>
</ul>
<h2>Conclusion: Bridge the Gap to Production</h2>
<p>The path to production-ready AI doesn’t have to be a bottleneck. By integrating AzureML with the JFrog Platform, you provide your A/ML teams with the tools they thrive in while maintaining the security and governance practices your enterprise requires.</p>
<p><strong>Ready to secure your AI supply chain? </strong>View the <a href="https://docs.jfrog.com/integrations/docs/azure-machine-learning-integration-with-jfrog-platform">technical documentation</a> or explore our <a href="https://jfrog.com/start/">interactive product tour</a>.</p>
