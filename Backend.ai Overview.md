# Backend.AI Overview

Backend.AI is an open source cloud resource management platform that makes it easy to utilize virtualized compute resource clusters in cloud or on-premises environments. Its container-based GPU virtualization technology supports the efficient use of GPUs by flexibly dividing one physical GPU so multiple users can use it at the same time. Beyond raw GPU sharing, Backend.AI offers a full-stack platform for AI/ML workloads — covering interactive development, distributed training, and model serving — with management features tailored for researchers, administrators, and DevOps teams.

---

## What Makes Backend.AI Special

Backend.AI stands apart from generic container or Kubernetes-based platforms because it is purpose-built for AI/ML and HPC workloads. Below are the capabilities that distinguish it.

### 1. Fractional GPU Virtualization 
Most platforms allocate GPUs as whole units, which wastes capacity for users who only need a slice. Backend.AI's GPU virtualization plug-in lets a single physical NVIDIA GPU be split among multiple containers — with isolated memory and compute shares — so a lab can fit many concurrent users on the same hardware without contention. This is one of the platform's signature features and a major reason organizations choose it over plain Kubernetes.


### 2. Cluster Compute Sessions for Distributed Training
Backend.AI natively supports multi-container cluster sessions for distributed computing and training. Containers in a cluster session are automatically wired together over a private network, with auto-generated SSH keys, predictable hostnames (`main1`, `sub1`, `sub2`, ...), and zero manual configuration. Users can simply `ssh sub1` from the main container — no key exchange, no firewall rules. Two modes are available:
- **Single Node:** All containers on one agent node (uses a local bridge network).
- **Multi Node:** Containers spread across multiple agents (uses an overlay network).

If a multi-node request can fit on a single agent, Backend.AI consolidates it automatically to minimize network latency. This is ideal for frameworks like PyTorch DDP, TensorFlow MultiWorker, Horovod, and MLflow.


### 3. Multiple Session Types Tailored to AI Workflows
Rather than one-size-fits-all jobs, Backend.AI offers three workload types:
- **Interactive sessions** — Jupyter, VS Code, R Studio, Terminal, MLflow, TensorBoard, Microsoft NNI for live experimentation.
- **Batch sessions** — Queued long-running jobs such as full training runs.
- **Inference / Model Serving** — Deploy trained models as autoscaling API endpoints.

### 4. Storage Integration and Virtual Folders
Backend.AI integrates with different file systems. Users interact with these through **Virtual Folders (vFolders)** — cloud folders that auto-mount into any container regardless of which node it runs on, with fine-grained sharing and per-user/project quotas.



## Accessing the Portal & Authentication

To begin using the Backend.AI platform at USC, navigate to the following web address:

**Portal URL:** [https://backendai.carc.usc.edu/start](https://backendai.carc.usc.edu/start)

### Logging in with USC Credentials
For all USC users, it is recommended to use the **Single Sign-On (SSO)** method. This ensures you are authenticated through the university's secure system.

1. **Visit the Start Page**
   Go to [backendai.carc.usc.edu/start](https://backendai.carc.usc.edu/start).

2. **Select the USC Login Option**
   Instead of using the standard email/password fields, click the link labeled **LOGIN WITH USC** at the bottom of the card.

   ![Backend AI Login Interface](images/login.png)

3. **USC Shibboleth Authentication**
   You will be redirected to the standard USC sign-in page. Enter your **USC NetID** and password, and complete the **Duo 2FA** prompt if requested.

4. **Redirection**
   Once verified, you will be automatically returned to the Backend.AI portal with your session active.

---

## The Start Page (Landing Page)

The **Start Page** is your primary entry point for launching tasks. It features quick-action cards to manage data and compute workloads.

![Backend AI Start Page](images/start_page.png)

### Quick Start Cards
- **Create New Storage Folder:** Initialize virtual folders (vFolders) and upload datasets. vFolders auto-mount into any container regardless of node.
- **Start Interactive Session:** Launch real-time environments like Jupyter Notebook, VS Code, R Studio, or a shell terminal.
- **Start Batch Session:** Queue background tasks for predefined scripts or long-running training jobs.
- **Start From URL:** Import projects directly from GitHub, GitLab, or remote notebooks.
- **Start Model Service:** Deploy trained models as API endpoints for inference, with autoscaling.

### Cluster Compute Sessions (Advanced)
When launching a session, you can also create a **cluster session** for distributed training:

1. In the session creation dialog, set the resources per container (e.g., 4 CPUs allocates 4 cores to *each* container).
2. Choose **Cluster mode**: `Single Node` or `Multi Node`.
3. Set **Cluster size** (e.g., 3 creates one `main1` plus two `sub` containers).
4. Mount your data vFolder and click **LAUNCH**.

Each container is given environment variables like `BACKENDAI_CLUSTER_HOST`, `BACKENDAI_CLUSTER_HOSTS`, `BACKENDAI_CLUSTER_ROLE`, and `BACKENDAI_CLUSTER_SIZE` so your distributed training script can discover its peers automatically.

> **Tip:** Total resources required = (per-container resources) × (cluster size). Sessions stay in `PENDING` if the cluster doesn't have enough free capacity.

---

## Navigation & UI

The interface is organized into logical functional areas via the sidebar and top bar.

### Navigation Sidebar
- **Start:** The quick-action launcher (current page).
- **Dashboard:** High-level overview of resource usage and active session counts.
- **Data (Storage):** Interface for managing virtual folders, sharing, and quotas.
- **Sessions (Workload):** Monitor running tasks, view per-container logs, and access running apps.
- **Serving:** Deploy and manage model inference endpoints.
- **Model Store:** Browse and publish reusable trained models.
- **Chat (Playground):** Interface for LLM interaction and experimental tools.
- **Import & Run:** Launch sessions directly from a Git repository or notebook URL.
- **My Environments:** Manage your custom container images.
- **Statistics:** Historical resource usage data.
- **Agent Summary:** Visibility into agent node health.

### Top Bar Features
- **Project Selector:** Toggle between different projects (e.g., "default") to manage quotas. A user can belong to multiple projects within a domain.
- **Session Timer:** Monitor system time and active runtimes.

---

## Key Concepts (Quick Reference)

| Concept | Meaning |
|---|---|
| **User** | A person logged into Backend.AI. Roles: normal user, domain admin, superadmin. |
| **Domain** | Top-level authority and resource boundary (e.g., an organization or affiliate). |
| **Project** | A working unit inside a domain. Users can belong to multiple projects. |
| **Compute Session** | An isolated container with full root-like rights inside it; invisible to other users. |
| **Image** | A pre-built container snapshot with runtimes and ML frameworks. |
| **Virtual Folder (vFolder)** | A persistent cloud folder that auto-mounts into any container. |
| **Application Service** | The Jupyter / VS Code / Terminal / TensorBoard apps you launch inside a session. |

---

## System Info (USC CARC)

- **Organization:** USC University of Southern California
- **Facility:** Center for Advanced Research Computing
- **Backend.AI WebUI Version:** 25.15 (latest stable, as of 2026)

> ### Technical Note
> If you are unable to reach the login page, please verify that you are connected to the USC network or have the **USC VPN** active, as required by CARC security policies.

---

## Further Reading

- Official WebUI User Guide: <https://webui.docs.backend.ai/en/latest/index.html>
- Cluster Compute Sessions: <https://webui.docs.backend.ai/en/latest/cluster_session/cluster_session.html>
- Model Serving: <https://webui.docs.backend.ai/en/latest/model_serving/model_serving.html>
- Mounting vFolders: <https://webui.docs.backend.ai/en/latest/mount_vfolder/mount_vfolder.html>
