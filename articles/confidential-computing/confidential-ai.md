---
title: Confidential AI
titleSuffix: Azure Confidential Computing
description: Confidential AI services and solutions
services: virtual-machines
author: kapilv
ms.service: azure-virtual-machines
ms.subservice: azure-confidential-computing
ms.topic: concept-article
ms.date: 05/17/2023
ms.author: kapilv
# Customer intent: "As a data scientist, I want to utilize confidential AI technologies for training and deploying models, so that I can protect sensitive data and ensure compliance while enhancing the performance and accuracy of AI applications."
---

# Confidential AI
AI has been shaping several industries such as finance, advertising, manufacturing, and healthcare well before the recent progress in generative AI. Generative AI models have the potential to create an even larger impact on society. Microsoft has been at the forefront of defining the [principles of Responsible AI](https://www.microsoft.com/ai/responsible-ai) to serve as a guardrail for responsible use of AI technologies. Confidential computing and confidential AI are a key tool to enable security and privacy in the Responsible AI toolbox.

## What is Confidential AI?
Confidential AI is a set of hardware-based technologies that provide cryptographically verifiable protection of data and models throughout the AI lifecycle, including when data and models are in use. Confidential AI technologies include accelerators such as general purpose CPUs and GPUs that support the creation of Trusted Execution Environments (TEEs), and services that enable data collection, pre-processing, training and deployment of AI models. Confidential AI also provides tools to increase trust, transparency, and accountability in AI deployments.

## What scenarios does Confidential AI address? 

Confidential AI addresses several scenarios spanning the entire AI lifecycle.

-   **Confidential Training**. Confidential AI protects training data, model architecture, and model weights during training from advanced attackers such as rogue administrators and insiders. Just protecting weights can be critical in scenarios where model training is resource intensive and/or involves sensitive model IP, even if the training data is public. With confidential training, models builders can ensure that model weights and intermediate data such as checkpoints and gradient updates exchanged between nodes during training aren't visible outside TEEs.

-   **Confidential Fine-tuning**. It's common to fine-tune generic AI models using domain specific, private data to improve precision for specific tasks. For example, a financial organization may fine-tune an existing language model using proprietary financial data. Confidential AI can be used to protect proprietary data and the trained model during fine-tuning.

-   **Confidential Multi-party Training**. Confidential AI enables a new class of [multi-party training scenarios](./multi-party-data.md). Organizations can collaborate to train models without ever exposing their models or data to each other, and enforcing policies on how the outcomes are shared between the participants.

-   **Confidential Federated Learning**. Federated learning has been proposed as an alternative to centralized/distributed training for scenarios where training data cannot be aggregated, for example, due to data residency requirements or security concerns. When combined with federated learning, confidential computing can provide stronger security and privacy. For example, gradient updates generated by each client can be protected from the model builder by hosting the central aggregator in a TEE. Similarly, model developers can build trust in the trained model by requiring that clients run their training pipelines in TEEs. This ensures that each client’s contribution to the model has been generated using a valid, pre-certified process without requiring access to the client’s data.

-   **Confidential Inferencing**. A typical model deployment involves several participants. Model developers are concerned about protecting their model IP from service operators and potentially the cloud service provider. Clients, who interact with the model, for example by sending prompts that may contain sensitive data to a generative AI model, are concerned about privacy and potential misuse. Confidential inferencing enables verifiable protection of model IP while simultaneously protecting inferencing requests and responses from the model developer, service operations and the cloud provider. For example, confidential AI can be used to provide verifiable evidence that requests are used only for a specific inference task, and that responses are returned to the originator of the request over a secure connection that terminates within a TEE.

## What are the industry use cases for Confidential AI?

With Azure Confidential Computing, customers and partners are building Confidential AI solutions addressing many use cases.

1.  **Speech and face recognition**. Models for speech and face recognition operate on audio and video streams that contain sensitive data. In some scenarios, such as surveillance in public places, consent as a means for meeting privacy requirements may not be practical. Confidential AI allows data processors to train models and run inference in real-time while minimizing the risk of data leakage.

2.  **Anti-money laundering/Fraud detection**. Confidential AI allows multiple banks to combine datasets in the cloud for training more accurate AML models without exposing personal data of their customers. Models trained using combined datasets can detect the movement of money by one user between multiple banks, without the banks accessing each other's data. Through confidential AI, these financial institutions can increase fraud detection rates, and reduce false positives.

3.  **Assisted diagnostics and predictive healthcare**. Development of diagnostics and predictive healthcare models requires access to highly sensitive healthcare data. Getting access to such datasets is both expensive and time consuming. Confidential AI can unlock the value in such datasets, enabling AI models to be trained using sensitive data while protecting both the datasets and models throughout the lifecycle.

## Why confidential computing?

The effectiveness of AI models depends both on the quality and quantity of data. While much progress has been made by training models using publicly available datasets, enabling models to perform accurately complex advisory tasks such as medical diagnosis, financial risk assessment, or business analysis require access to private data, both during training and inferencing.

There are many privacy-preserving technologies protect private data that can protect data in use. For example, data can be scrubbed and de-identified before sharing. However, de-identification alone has been shown to be brittle, and in some cases can reduce utility. Other approaches such as fully homomorphic encryption (FHE) and secure multi-party computation (MPC) can limit expressiveness or introduce significant performance overheads. 

Confidential computing can unlock access to sensitive datasets while meeting security and compliance concerns with low overheads. With confidential computing, data providers can authorize the use of their datasets for specific tasks (verified by attestation), such as training or fine-tuning an agreed upon model, while keeping the data protected. Confidential training can be combined with differential privacy to further reduce leakage of training data through inferencing. Model builders can make their models more transparent by using confidential computing to generate non-repudiable data and model provenance records. Clients can use remote attestation to verify that inference services only use inference requests in accordance with declared data use policies. 

## What are the options to get started? 

### ACC platform offerings that help enable Confidential AI
Azure already provides state-of-the-art offerings to secure data and AI workloads. You can further enhance the security posture of your workloads using the following Azure Confidential computing platform offerings.

-   [Confidential VMs on SNP](./confidential-vm-overview.md) and [TDX](./tdx-confidential-vm-overview.md) (in limited preview). CPU based AI workloads, for example data pre-processing, and training and inferencing for smaller models can use Confidential VMs based on SNP and TDX to protect sensitive code and data in use.

-   [Confidential Containers on ACI](/azure/container-instances/container-instances-confidential-overview).
    Confidential Containers on ACI are another way of deploying containerized workloads on Azure. In addition to protection from the cloud administrators, confidential containers offer protection from tenant admins and strong integrity properties using container policies. This makes them a great match for low-trust, multi-party collaboration scenarios. See [here](https://github.com/microsoft/confidential-ai) for a sample demonstrating confidential inferencing based on unmodified NVIDIA Triton inferencing server.

For AI workloads that rely on GPUs, Microsoft and NVIDIA are collaborating to bring confidential computing to NVIDIA GPUs. [Azure Confidential GPU VMs](https://azure.microsoft.com/blog/azure-confidential-computing-with-nvidia-gpus-for-trustworthy-ai/) based on AMD SEV-SNP and A100 GPUs are currently in [limited preview](https://aka.ms/accgpusignup).

### ACC partner solutions that enable Confidential AI

Use a partner that has built Confidential AI solutions on top of the Azure confidential computing platform.

-   [Anjuna](https://www.anjuna.io/use-case-solutions) provides a confidential computing platform to enable various use cases for organizations to develop machine learning models without exposing sensitive information.

-   [Beekeeper AI](https://www.beekeeperai.com/) enables healthcare AI through a secure collaboration platform for algorithm owners and data stewards. BeeKeeperAI uses privacy-preserving analytics on multi-institutional sources of protected data in a confidential computing environment. The solution supports end-to-end encryption, secure computing enclaves, and Intel's latest SGX enabled processors to protect the data and the algorithm IP.

-   [Fortanix](https://www.fortanix.com/platform/confidential-ai) provides a confidential computing platform that can enable confidential AI, including multiple organizations collaborating together for multi-party analytics.

-   [Mithril Security](https://www.mithrilsecurity.io/) provides tooling to help SaaS vendors serve AI models inside secure enclaves, and providing an on-premises level of security and control to data owners. Data owners can use their SaaS AI solutions while remaining compliant and in control of their data.

-   [Opaque](https://opaque.co/) provides a confidential computing platform for collaborative analytics and AI, giving the ability to perform analytics while protecting data end-to-end and enabling organizations to comply with legal and regulatory mandates.
