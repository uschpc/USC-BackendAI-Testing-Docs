# Topanga Set Up Process

```mermaid
flowchart TD
    Start(["Start"])
    ReqAccess["Request Topanga Access<br/>https://hpcaccount.usc.edu"]
    Prereq{"PI has active<br/>CARC project?"}
    FixPrereq["Set Up CARC Project"]
    WorkTag{"USC Worktag Configured?"}
    AddTag["Add USC Worktag<br/>for Project"]
    Approved(["Access Request Approved<br/>(Status: Active)"])

    Start --> ReqAccess
    ReqAccess --> Prereq
    Prereq -- "No" --> FixPrereq --> ReqAccess
    Prereq -- "Yes" --> WorkTag
    WorkTag -- "No" --> AddTag --> Approved
    WorkTag -- "Yes" --> Approved
```

## Steps

1. **Request Topanga Access** via the CARC User Portal ([https://hpcaccount.usc.edu](https://hpcaccount.usc.edu)).
2. Confirm the **PI has an active CARC project** — if not, set one up and re-submit the request.
3. Confirm the **USC work tag** is configured for your project — if not, add it.
4. Once both checks pass, the **Access Request is Approved**.
