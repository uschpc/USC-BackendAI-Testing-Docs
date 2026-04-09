Cesar:
Objective: Instructions on data persistence and mounting file systems.

##  Managing Data (Virtual Folders)

Data in Virtual Folders is **persistent**, whereas files inside the container root are deleted after the session ends.

* **Upload:** Use the **Data** tab to upload datasets.
* **Share:** You can share folders with teammates for collaborative projects.

> **Important:** Always save your work in the mounted virtual folder paths (e.g., `/home/jovyan/work`) to ensure it isn't lost.

---

There are multiple places to store data on the Topanga system and can be categorized into 3 locations kinds:

* Session Specific
* Topanga Filesystem
* HPC Cluster filesystem

## Session Specific Data

| Path | Max Disk Capacity|
|---|---|
|`/home/work`|?? GB|

The "home" directory for every session is located at `/home/work`. Data here is stored locally on the Topanga system and will be deleted when the session ends.
## Topanga Filesystem
| Path | Max Disk Capacity|
|---|---|
|`/home/folder_name`|?? GB|


For data storage that will be persistent across multiple sessions, create a *Storage Folder* using the backend.ai interface. 


![test](images/create_new_storage_folder.png)
The **Folder name** will determine the system path that the directory will be available at in each session.

**Usage Mode** refers to ....!


The storage folder can be mounted during the session creation process. 

![test](images/add_storage_folder_to_session.png)
Under the 'Data & Storage' section, folders selected will be available during the session.
## HPC Cluster Filesystem
| Path | Max Disk Capacity|
|---|---|
|`/home/$USER`|100 GB|
|`/project2/$PI_NAME_PROJECT_ID`|?? GB|
|`/scratch2/$USER`|10 TB |

