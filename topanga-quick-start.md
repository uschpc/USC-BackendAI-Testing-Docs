# Topanga Quick Start Guide

```mermaid
flowchart TD
    Start(["Start"])
    ReqAccess["Request Topanga Access<br/>hpcaccount.usc.edu"]
    Prereq{"PI has active CARC project<br/>and annual review complete?"}
    FixPrereq["Complete Project Review /<br/>Set Up CARC Project"]
    WorkTag{"USC Worktag Configured?"}
    AddTag["Add USC Worktag<br/>for Project"]
    Approved["Access Request Approved<br/>(Status: Active)"]
    VpnSso["Connect to USC VPN and<br/>Log in via USC SSO / Duo"]
    SelectProject["Select Project<br/>(Top Bar)"]
    CreateFolder["Create Storage Folder<br/>and Upload Code/Data (optional)"]
    Launch["Launch Compute Session"]
    Interactive["Interactive Session"]
    Batch["Batch Session"]
    Inference["Inference Session"]
    Work["Work on Research Project"]
    RunApps["Run Apps and Load<br/>Software Modules (Lmod)"]
    UploadData["Upload and Manage Data<br/>in Mounted Folder"]
    ServeModel["Serve Model and<br/>Generate API Token"]
    Lifecycle["Pause or Terminate Session"]
    End(["End"])

    Start --> ReqAccess
    ReqAccess --> Prereq
    Prereq -- "No" --> FixPrereq --> ReqAccess
    Prereq -- "Yes" --> WorkTag
    WorkTag -- "No" --> AddTag --> ReqAccess
    WorkTag -- "Yes" --> Approved
    Approved --> VpnSso --> SelectProject --> CreateFolder --> Launch
    Launch --> Interactive
    Launch --> Batch
    Launch --> Inference
    Interactive --> Work
    Batch --> Work
    Inference --> ServeModel --> Work
    Work --> RunApps
    Work --> UploadData
    RunApps --> Lifecycle
    UploadData --> Lifecycle
    Lifecycle --> End
```

## Steps

1. **Request Topanga Access** via the CARC User Portal ([hpcaccount.usc.edu](https://hpcaccount.usc.edu)).
2. Confirm the **PI has an active CARC project** and the **annual project review is complete** — required before a request can be approved.
3. **Add a USC work tag** to your project if it isn't already configured — also required for approval.
4. Once approved, **connect to the USC VPN** and **log in via USC SSO / Duo** at [topanga.carc.usc.edu](https://topanga.carc.usc.edu).
5. **Select your project** in the top bar.
6. Optionally **create a storage folder** and upload code/data ahead of time.
7. **Launch a compute session**: Interactive, Batch, or Inference.
   - Inference sessions additionally require creating a model-definition file and generating an API token before serving traffic.
8. **Work on your research project** — run applications, load software modules (Lmod), and upload/manage data in your mounted storage folder.
9. **Pause or terminate the session** when finished to manage cost and preserve state appropriately.
