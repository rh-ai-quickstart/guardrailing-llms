# Deploy a privacy-focused AI assistant

Build a healthcare AI assistant that ensures your large language model has multiple layers of protection, including PII detection and content moderation.

## Detailed description 

This quickstart includes a healthcare AI assistant demo showing how
guardrails could help protect HIPAA-compliant applications.

The demo tests a patient services AI with four protection layers:
1. **PII Detection** - Protects Social Security Numbers and medical IDs
2. **Content Moderation** - Blocks inappropriate language  
3. **Prompt Injection Protection** - Prevents system manipulation
4. **Gibberish Detection** - Filters out nonsense queries

For example, here's how PII detection works in action:

![diagram.png](docs/images/wb0.png)

Explore the complete interactive demo in `docs/healthcare-guardrails.ipynb`.

The LLM Guardrails quickstart is a quick-start template for deploying multiple layers of protection for LLM applications using TrustyAI's orchestrator and specialized detector services.

This quickstart includes a Helm chart for deploying:

- A Llama 3.2 3B Instruct model with GPU acceleration.
- Multiple AI safety detectors: gibberish detection, prompt injection detection, and hate/profanity detection.
- TrustyAI GuardrailsOrchestrator for coordinating safety checks.
- Configurable detection thresholds and routing policies.

### Architecture diagrams

![architecture.png](docs/images/architecture.png)


## Requirements 

### Recommended hardware requirements 

- GPU with +24GiB vRAM for main LLM
- CPU cores: 12+ cores total (4 for LLM + 8 for detectors) with 4th Gen Xeon® Scalable® processors or newer
- Memory: 24Gi+ RAM total
- Storage: 10Gi

### Minimum hardware requirements 

- NVIDIA GPU with 24GiB vRAM required for main LLM
- CPU cores: 8+ cores total
- Memory: 16Gi+ RAM total
- Storage: 5Gi 

### Minimum software requirements

- Red Hat OpenShift 4.19+
- Red Hat OpenShift Service Mesh 3+
- Red Hat OpenShift AI 3.4.0+
    - KServe needs to be enabled
    - TrustyAI operator needs to be enabled (`GuardrailsOrchestrator` CRD is provided)

### Required user permissions

- Cluster admin permissions are required

## Install

### Clone the repository

```
git clone https://github.com/rh-ai-quickstart/guardrailing-llms.git && cd guardrailing-llms/
```

### Create a new project

```bash
PROJECT="guardrails-demo"

oc new-project ${PROJECT}
``` 

### Enable the TrustyAI operator

The Helm chart deploys a `GuardrailsOrchestrator` custom resource, which requires the TrustyAI operator CRD to be registered in the cluster. The CRD is bundled in `helm/crds/` and Helm will install it automatically, but the TrustyAI operator itself must be running to reconcile the resource.

Enable TrustyAI in the OpenShift AI DataScienceCluster before installing:

```bash
oc patch datasciencecluster default-dsc --type=merge \
  -p '{"spec":{"components":{"trustyai":{"managementState":"Managed"}}}}'
```

Wait for the operator to be ready:

```bash
oc wait --for=condition=Available deployment/trustyai-service-operator-controller-manager \
  -n redhat-ods-applications --timeout=300s
```

### Install with Helm

```bash
export DEVICE="gpu" # options: [gpu, cpu]
helm install ${PROJECT} helm/ --namespace ${PROJECT} --set device=$DEVICE
```

### Wait for the pods to be ready

```bash
oc get pod -n ${PROJECT}
```

You should see an output similar to:
<pre>
NAME                                                         READY   STATUS      RESTARTS   AGE
gibberish-detector-predictor-578fc59776-www4s                2/2     Running     0          25h
gorch-sample-5f95f587fd-wmk4x                                3/3     Running     0          51m
guardrails-workbench-0                                       2/2     Running     0          93m
guardrails-workbench-clone-repo-96jhd                        0/1     Completed   0          93m
ibm-hate-and-profanity-detector-predictor-846758cfb5-wnlnd   2/2     Running     0          25h
llama-32-3b-instruct-predictor-c8d55bd58-lctjn               2/2     Running     0          18m
prompt-injection-detector-predictor-7d784957f9-f2x5g         2/2     Running     0          25h
</pre>

### Test

You can get the OpenShift AI Dashboard URL by:
```bash
oc get routes rhods-dashboard -n redhat-ods-applications
```

Once inside the dashboard, navigate to Data Science Projects -> guardrails-demo (or what you called your ${PROJECT} if you changed from default).

![OpenShift AI Projects](docs/images/wb1.png)

Inside the project you can see Workbenches, open up the one for guardrails-workbench.

![OpenShift AI WB](docs/images/wb2.png)

Open the workbench, inside of the Jupyter Notebook folder, you'll see the `guardrailing-llms` repository already cloned, go to `docs/healthcare-guardrails.ipynb` and follow the instructions.

![OpenShift AI Jupyter Notebook](docs/images/wb3.png)

Enjoy!

### Delete

```bash
helm uninstall ${PROJECT} --namespace ${PROJECT} 
```

## References 

- [Red Hat documentation](https://docs.redhat.com/en/documentation/red_hat_openshift_ai_self-managed/2.23/html/monitoring_data_science_models/configuring-the-guardrails-orchestrator-service_monitor)


## Tags 

* **Industry:** Healthcare
* **Product:** OpenShift AI 
* **Use case:** PII detection 
