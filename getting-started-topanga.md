# Getting Started with Topanga

### Request Access

Request access to Topanga AI Computing from the [CARC User Portal](https://hpcaccount.usc.edu). An active Topanga allocation gives a CARC project access to launch Topanga sessions and sets the project's cloud computing limits, quota, status, and end date.

Before requesting an allocation, make sure you have or belong to an active CARC project.

1. Go to the CARC User Portal: [https://hpcaccount.usc.edu](https://hpcaccount.usc.edu).
2. Sign in with your USC NetID and complete Duo if prompted.
3. Select the project that should receive Topanga access or create a new project.
4. On the Project Detail page, select the blue **Update Project Information** button at the topn to update your billing contact information and full worktag number in the format of company.costcenter.ppgg (e.g., USC.CA123456.PG1234567). Topanga allocations will not be approved without this information.
5. On the Project Details page again, scroll down to the **Allocations** table.
5. Click the green **Request Resource Allocation** button.
6. Choose **Topanga AI Computing Service (Cloud)** from the resource menu.
7. Add a justification explaining why the project needs Topanga access. Your justification should briefly include:
   * Project ID and PI name
   * Research group, class, or user group that will use Topanga
   * Expected workload, such as Jupyter notebooks, GPU training, batch jobs, model serving, or class exercises
   * Expected CPU, GPU, memory, runtime, and concurrent session needs
   * Start date, deadline, or renewal reason, if relevant
8. Submit the request.

After approval, the allocation will appear in the project's allocation table with one of the following statuses:
   * **New**: The request has been submitted and is waiting for CARC review.
   * **Active**: CARC has approved the request and the project can use Topanga.
   * **Expiring**: The access is close to its end date. Use the renewal action in the allocation table if the project still needs Topanga access. For instructions on annual project and allocation renewals, please visit our [website](https://www.carc.usc.edu/user-guides/project-and-allocation-management/annual-project-review)
   * **Ended** or **inactive**: The access is no longer available. Renew it or submit a new request, if needed.

The allocation table shows the resource name, resource type, information, quantity, quota usage, annual cost if applicable, status, end date, and available actions.

If **Topanga AI Computing Service (Cloud)** is not available in the resource menu, submit a CARC support ticket under the **Account** category and ask for Topanga cloud access for the project. Include the project ID, PI, users who need access, requested access size, and reason for the request.

### Logging in with USC Credentials

Once the allocation status is **Active**, sign in with **Single Sign-On (SSO)** through USC Shibboleth.

1. **Open the Topanga portal**
   [Topanga Portal](https://topanga.carc.usc.edu)

   Topanga is only be accessible via a connection to either USC's Secure network or a USC VPN. Instructions for setting up a VPN connection can be found at the following links:

   * [Windows](https://itservices.usc.edu/anyconnect/windows/)
   * [MacOS](https://itservices.usc.edu/anyconnect/mac/)
   * [Linux](/user-guides/quick-start-guides/anyconnect-vpn-setup)

2. **Use USC Shibboleth Authentication**
   You will be redirected to the standard USC sign-in page. Enter your **USC NetID** and password, and complete the **Duo 2FA** prompt.

3. **Open the Dashboard**
   After authentication, you will be directed to the **Dashboard**, which features various quick start cards.

4. **Log Out**
   When you log out, you will be returned to the **Topanga login page**. To log back into the Dashboard, click the **Login with USC** button on the login card rather than using the standard email/password fields.

### Dashboard

The dashboard is your primary entry point for launching tasks. It features quick-action cards to manage data and compute workloads.

![Topanga Start Page](images/start_page.png)

#### Quick start cards

* **Create New Storage Folder:** Create a persistent storage folder, also called a virtual folder or vFolder, that can be mounted into your compute sessions. You can upload code, datasets, notebooks, and results to this folder before starting a session, then access those files inside Jupyter, VS Code, RStudio, or a terminal as if they were on a local disk. Files in a storage folder remain available after a session ends. A storage folder can also expand your available storage and provide a faster storage option for your work. Creating a storage folder is optional, but it is recommended if you plan to reuse data or save results.

* **Start Interactive Session:** Launch a live computing environment for hands-on work. Interactive sessions are useful for exploring data, testing code, running notebooks, debugging scripts, or working in a terminal. Depending on the available environment, you can start tools such as Jupyter Notebook, VS Code, RStudio, or a shell terminal, choose the CPU/GPU resources you need, and mount your storage folders so your files are available during the session.

* **Start Batch Session:** Submit a script or job to run in the background. Batch sessions are useful for longer-running work such as model training, simulations, data preprocessing, or other tasks that do not require constant interaction. You can choose the environment and resources, mount any needed storage folders, and review logs or output files after the job starts or completes.

* **Start From URL:** Start a session from an existing project or notebook hosted online, such as a GitHub or GitLab repository. This is useful when you want to quickly work from shared code, course materials, example notebooks, or a research project repository without setting everything up manually from scratch.

* **Start Model Service:** Deploy a trained model as an inference service so it can receive requests and return predictions through an API endpoint. This is useful for testing, sharing, or running AI models after training. You can choose the model environment and compute resources needed for the service, then monitor and stop the service when it is no longer needed.

#### Navigation sidebar

- **Start:** Open the quick-action launcher.
- **Dashboard:** View resource usage and active session counts.
- **Data (Storage):** Manage virtual folders, sharing, and quotas.
- **Sessions (Workload):** Monitor running tasks, view per-container logs, and access running apps.
- **My Environments:** Manage your custom container images.
- **Chat (Playground):** Use LLM interaction and experimental AI tools.
- **Serving:** Deploy and manage model inference endpoints.
- **Model Store:** Browse and publish reusable trained models.

#### Top bar menu

- **Project Selector:** Switch between projects, such as `default`, to manage project-specific quotas. A user can belong to multiple projects within a domain.
- **Session Timer:** Monitor system time and active runtimes.

### Key Concepts (Quick Reference)

| Concept | Meaning |
|---|---|
| **User** | A person logged into Topanga. Roles include normal user, domain admin, and superadmin. |
| **Domain** | A top-level resource boundary, such as an organization or affiliate group. |
| **Project** | A working unit inside a domain. Users can belong to multiple projects. |
| **Compute Session** | An isolated running workspace where your selected apps, code, and resources are available. |
| **Image** | A pre-built software environment with runtimes, tools, and ML frameworks. |
| **Virtual Folder (vFolder)** | A persistent storage folder that can be mounted into compute sessions. |
| **Application Service** | An app, such as Jupyter, VS Code, Terminal, or TensorBoard, launched inside a compute session. |

### Getting help

If you need additional assistance getting started with CARC and Topanga, please see the [User Support page](/user-support) for more information.

## Additional Resources

- [CARC User Portal](https://hpcaccount.usc.edu)
- [CARC Allocation Types and Requests](https://www.carc.usc.edu/user-guides/project-and-allocation-management/allocation-types-and-requests.html)