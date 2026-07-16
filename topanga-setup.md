# Topanga Set Up Process

```mermaid
flowchart TD
    Start(["Start"])
    Prereq{"PI has active<br/>CARC project?"}
    FixPrereq["Set Up CARC Project"]
    WorkTag{"USC Worktag Present<br/>for Project?"}
    AddTag["Add USC Worktag<br/>for Project"]
    ReqAccess["Request Topanga Access<br/>hpcaccount.usc.edu"]
    Approved(["Access Request Approved<br/>(Status: Active)"])

    Start --> Prereq
    Prereq -- "No" --> FixPrereq --> WorkTag
    Prereq -- "Yes" --> WorkTag
    WorkTag -- "No" --> AddTag --> ReqAccess
    WorkTag -- "Yes" --> ReqAccess
    ReqAccess --> Approved
```

## Steps

1. Confirm the **PI has an active CARC project** — if not, set one up.
2. Confirm the **USC work tag** is present for your project — if not, add it.
3. **Request Topanga Access** via the CARC User Portal ([https://hpcaccount.usc.edu](https://hpcaccount.usc.edu)).
4. Once both checks pass, the **Access Request is Approved**.
