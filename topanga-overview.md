# Topanga AI Computing Overview

Topanga AI Computing is The Center for Advanced Research Computing's (CARC) premiere pay-as-you-go computing platform for running AI/ML, data science, and research workloads in container-based sessions. Each session is a dedicated, isolated environment where you can choose the CPU or GPU resources you need and only pay for the time you use. Powered by [Backend.AI](https://webui.docs.backend.ai/en/latest/index.html), Topanga is an easy way to access CARC-managed computing resources on demand.

Topanga supports several ways to run your work:

* **Interactive sessions:** Launch tools such as Jupyter, VS Code, RStudio, or a terminal for live exploration and debugging.
* **Batch sessions:** Run longer jobs in the background, such as training runs, simulations, or data processing scripts.
* **Model services:** Deploy trained models as API endpoints for inference.

[Log In To Topanga](https://topanga.carc.usc.edu)

Topanga is only be accessible via a connection to either USC's Secure network or a USC VPN. Instructions for setting up a VPN connection can be found at the following links:

* [Windows](https://itservices.usc.edu/anyconnect/windows/)
* [MacOS](https://itservices.usc.edu/anyconnect/mac/)
* [Linux](/user-guides/quick-start-guides/anyconnect-vpn-setup)

### Request access

Topanga is available to all CARC users. You must belong to an active project and request access to Topanga through the [CARC user portal](https://hpcaccount.usc.edu/). For instructions on requesting access, refer to the Getting Started with Topanga section. 

::: {.alert .alert-info}
*Topanga is a ***paid resource***. Once access to the platform is acquired, ***any user*** on the project can request a paid session that will get billed to the associated project's account.*
:::

### Key features

Topanga supports common research workflows, from interactive exploration to larger AI and HPC jobs. It can share GPU resources efficiently, connect multiple compute containers for distributed training, provide different session types for notebooks, terminals, batch jobs, and model serving, and make project files available through persistent virtual folders.

#### Efficient GPU Sharing

Some research tasks only need part of a GPU. Topanga can split a physical NVIDIA GPU into fractional GPUs (fGPUs) across multiple sessions with isolated memory and compute shares, helping more users run GPU workloads at the same time. This is useful for labs, classes, and exploratory work where a full GPU may not be necessary.

#### Distributed Training Sessions

Topanga supports multi-container sessions for distributed computing and training. Containers in a cluster session are automatically connected over a private network, with ready-to-use hostnames (`main1`, `sub1`, `sub2`, ...) and generated SSH access. From the main container, users can connect to a worker container with `ssh sub1` without manually exchanging keys or configuring firewall rules.

Two modes are available:

- **Single node:** All containers run on one compute node, using a local bridge network.
- **Multi-node:** Containers can run across multiple compute nodes, using an overlay network.

If a multi-node request can fit on a single compute node, Topanga keeps the containers together to reduce network latency. This setup is useful for frameworks such as PyTorch DDP, TensorFlow MultiWorker, Horovod, and MLflow.

### Available sessions

Topanga offers several preset session configurations&mdash;both CPU-focused and GPU-focused&mdash;to meet different needs. 

| **Session name** | **CPU cores** | **Memory** | **Fractional GPUs (fGPUs)** | **Shared Memory** | **Price per hour**|
|---|---|---|---|---|---|
| cpu-2xs | 1  | 6 GB   | 0 | 1 GB | $0.04 |
| cpu-xs  | 2  | 12 GB  | 0 | 1 GB | $0.08 |
| cpu-sm  | 4  | 24 GB  | 0 | 1 GB | $0.14 |
| cpu-md  | 8  | 48 GB  | 0 | 2 GB | $0.25 |
| cpu-lg  | 16 | 96 GB  | 0 | 2 GB | $0.50 |
| cpu-xl  | 32 | 192 GB | 0 | 4 GB | $1.00 |
| cpu-2xl | 62 | 368 GB | 0 | 4 GB | $1.80 |
| gpu-2xs | 2  | 24 GB  | 0.1 | 1 GB  | $0.35 |
| gpu-xs  | 4  | 48 GB  | 0.2 | 2 GB  | $0.70 |
| gpu-sm  | 8  | 96 GB  | 0.5 | 2 GB  | $1.50 |
| gpu-md  | 16 | 128 GB | 1.0 | 4 GB  | $3.00 |
| gpu-lg  | 32 | 256 GB | 2.0 | 8 GB  | $6.00 |
| gpu-xl  | 48 | 512 GB | 3.0 | 12 GB | $9.00 |
| gpu-2xl | 62 | 746 GB | 4.0 | 16 GB | $12.00 |

### Persistent Storage with Virtual Folders

Topanga connects your sessions to persistent storage through **Virtual Folders (vFolders)**. These folders can be mounted into your compute sessions regardless of which compute node the session runs on, making it easier to reuse code, data, and results across sessions. Virtual folders also support sharing and per-user or per-project quotas.

### Additional resources

- Official Backend.AI WebUI User Guide: <https://webui.docs.backend.ai/en/latest/index.html>
- Cluster Compute Sessions: <https://webui.docs.backend.ai/en/latest/cluster_session/cluster_session.html>
- Model Serving: <https://webui.docs.backend.ai/en/latest/model_serving/model_serving.html>
- Mounting vFolders: <https://webui.docs.backend.ai/en/latest/mount_vfolder/mount_vfolder.html>

