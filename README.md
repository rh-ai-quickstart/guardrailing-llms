# Guardrailing LLMs

Welcome to the LLM Guardrails quickstart!
Use this to quickly deploy a comprehensive AI safety framework with TrustyAI orchestrator and multiple detector services.  
To see how it's done, jump straight to [installation](#install). 

## Detailed description 

The LLM Guardrails quickstart is a quick-start template for deploying a comprehensive AI safety framework within Red Hat OpenShift AI. It's designed to provide multiple layers of protection for LLM applications using TrustyAI's orchestrator and specialized detector services.

This quickstart includes a Helm chart for deploying:

- A Llama 3.2 3B Instruct model with GPU acceleration.
- Multiple AI safety detectors: gibberish detection, prompt injection detection, and hate/profanity detection.
- TrustyAI GuardrailsOrchestrator for coordinating safety checks.
- Configurable detection thresholds and routing policies.

Get started quickly with a protected LLM deployment featuring automated safety detection and content filtering.

### Architecture diagrams

- TODO

![Architecture diagram coming soon](assets/images)

### References 

- TODO

## Requirements 

### Recommended hardware requirements 

- GPU required for main LLM: +24GiB vRAM
- CPU cores: 12+ cores total (4 for LLM + 8 for detectors)
- Memory: 24Gi+ RAM total
- Storage: 10Gi

### Minimum hardware requirements 

- GPU required for main LLM: 1 x NVIDIA GPU with 4GB+ VRAM  
- CPU cores: 8+ cores total
- Memory: 16Gi+ RAM total
- Storage: 5Gi 

### Required software  

- TODO

### Required permissions

- Standard user. No elevated cluster permissions required

## Install

**Please note before you start**

This example was tested on Red Hat OpenShift 4.16.24 & Red Hat OpenShift AI v2.16.2.  

### Clone

```
git clone https://github.com/rh-ai-quickstart/guardrailing-llms.git && \
    cd guardrailing-llms/  
```

### Create the project

```bash
PROJECT="guardrails-demo"

oc new-project ${PROJECT}
``` 

### Install with Helm

```bash
helm install guardrailing-llms helm/ \
    --namespace ${PROJECT} 
```

## Uninstall

```bash
helm uninstall guardrailing-llms --namespace ${PROJECT} 
```
