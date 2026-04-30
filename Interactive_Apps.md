In Backend.ai, an Interactive App is a web-based interface, such as JupyterLab, Visual Studio Code, or RStudio, that runs inside your interactive compute session and is securely exposed to your browser. It transforms a raw container into a familiar, productive workspace.

## Phase 1: Launching Interactive Apps
Unlike a standard Linux shell, Interactive Apps require specific software configurations. Backend.ai simplifies this by bundling them into "Environments" and "Images" or allowing you to add them dynamically.

1. The Launch Dialog
When you click Sessions or Start, you will see a section labeled "Start" or "Start Sessions"

![Start Session](../images/interactive_apps/launch_dialog_01.png)

Choose an "Interactive Session", then an environment of your choice. i.e., Pytorch. 
You can set environment variables, such as the Hugging Face token HF_TOKEN or others.
Then, in the "Resource Allocation" group, choose a GPU resource group and the size you need.

Then you can set a persistent storage location. It is a good place to store your models or data. You can set mountpoints in your home directory (/home/work)

2. Port Configuration
Backend.ai handles the networking for you, but understanding the flow helps:

Internal Port: The App runs on a specific port inside the container (e.g., Jupyter on 8888).
External Port: Backend.ai assigns a random secure port on the proxy server for you to access it from the outside.
The Proxy: You never connect directly to the Agent node. You connect to the Backend.ai Proxy, which forwards your traffic securely.

3. Click Start.

## Phase 2: Accessing & Authentication
Connecting to an interactive app is different from connecting to a terminal. It requires secure authentication tokens to prevent unauthorized access to your data.

1. The Launch Process
Wait for the Session status to turn green and say "Running".
In the session list, choose the newly created session.
Then, in the upper right corner, you will see icons; choose the one with four squares. 

![Session Icons](../images/interactive_apps/session_icons_01.png.png)


A list of available Apps will appear. 

![Apps List](../images/interactive_apps/apps_01.png)

In this example, we will choose JupyterLab. Click on the JupyterLab icon, and you will connect to the JupyterLab App with a kernel already containing PyTorch.

![Apps List](../images/interactive_apps/jupyterlab_01.png)




## Phase 3. The Workflow

1. The Filesystem Hierarchy
When you open Jupyter, you are looking at the file system of the Session Container.

/home/work is not persistent:
What is it? Your desktop environment's default folder.
Rule: Treat this like a scratchpad. Any file saved here vanishes when you destroy the session.
Use for: Cache files, temporary downloads, logs.
/home/work/cache (Persistent; assuming you created and mounted a storage there during session creation):
What is it? Your virtual hard drive mounted from the cloud storage.
Rule: This is your Save Location.
Use for: Code repositories, datasets, trained models (model.h5, checkpoint.pt).

2. Workflow: Saving Data Correctly
In JupyterLab:

Navigate: In the file explorer panel, click your-project-name (e.g., cache).
Create: Create a new notebook or script here.
Run: Execute your code.
Verify: Refresh the file browser in the App. Ensure the file is visible inside /home/work.
Finish: Stop your session.
Result: Your notebook file persists in the cache, ready for the next session.

The App is running on the server, not your local computer.
To get a file from the App to your laptop:
Download: Use the App's "Download" button (in Jupyter file explorer).
Clone: Use Git to push code to a remote repository (GitHub/GitLab) from within the App terminal.

## Phase 4: App Lifecycle vs. Session Lifecycle
It is vital to understand the relationship between the App and the Session.

1. Stopping the App
Action: Closing the browser tab or clicking "Disconnect" in the dashboard.
Result: The web interface closes.
Compute Cost: The session continues running on the backend. You are still paying for the GPU/CPU.
Memory: Your code, loaded variables, and model state remain in RAM. You can reconnect later exactly where you left off.
2. Pausing the Session
Action: Clicking "Pause" in the Session dashboard.
Result: The container is frozen.
Compute Cost: Reduced cost (resources released from active compute, but state kept).
App Access: Apps become inaccessible until you resume the session.
3. Destroying the Session
Action: Clicking "Destroy" in the Session dashboard.
Result: The container is deleted. All running Apps are terminated.
Data: Files in /home/work are lost. Files in /mnt/vfolders are safe.

Cost Saving Tip: If you leave work for the day, Pause the session. Do not just close the Jupyter tab. Closing the tab saves the App connection, but the server keeps billing you for the GPU. Pause the session to freeze costs while keeping your state.

Advanced App Features
1. Port Forwarding (Service Apps)
Sometimes you need to expose a custom web service (like a local Flask or Streamlit app) running inside your Jupyter session.

Install the App inside your session (e.g., pip install streamlit).
Run the App locally inside the Jupyter terminal: streamlit run app.py --server.port 8501.
Open the "Apps" tab in the Backend.ai dashboard.
Add Port: Manually add port 8501 to the list of exposed ports.
Access: Click the new link to view your Streamlit app in the browser securely.
2. Environment Variables
Apps sometimes need configuration (e.g., API keys).

In the Launch Dialog, there is an Environment Variables section.
Add API_KEY and SECRET_TOKEN.
These will be available to the App and your code inside the session.
3. Multiple Users, One App
If you use a "Team VFolder", multiple members can launch sessions with that folder attached. However, do not share one single session between two people simultaneously.

Bad: Two people clicking the same Jupyter link at the same time.
Good: Two people launching separate Sessions, both mounting the same VFolder. Backend.ai ensures file locking and prevents corruption.
Checklist: Before You Close the App
When you are finished for the day, run through this mental checklist:

Data Check: Did I save my notebook to /mnt/vfolders?
Connection Check: Did I Pause the session, or did I just close the browser tab? (Pause to save money/state).
Security Check: Did I clear my terminal history? (Run history -c or check .bash_history).
Resource Check: Did I accidentally leave a GPU-intensive session running while sleeping?
Summary
Interactive Apps on Backend.ai bridge the gap between powerful cloud infrastructure and your daily workflow. By mastering the distinction between the Session (the hardware state) and the App (the interface), you gain the ability to develop securely, save persistently, and scale efficiently.

## Remember: Your App is temporary. The storage lasts between sessions. Save wisely.