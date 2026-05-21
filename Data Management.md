
There are multiple places to store data on the Topanga system and can be categorized into 3 locations kinds:

* Session Specific
* Topanga Filesystem
* HPC Cluster filesystem

## Session Specific Data

| Path | Max Disk Capacity|
|---|---|
|`/home/work`|50 GB|

The "home" directory for every session is located at `/home/work`. Data here is temporarily stored on the Topanga filesystem and will be deleted when the session ends. **Do not store important or long-term data here.**

## Topanga Persistent Storage
| Path | Max Disk Capacity|
|---|---|
|`/home/folder_name`|1 TB|


For data storage that will be persistent across multiple sessions, create a *storage folder*. Cost is based on active storage so use when you need to temporarily store data between sessions. For longer term data storage, it may be more cost efficent to use the /project2 or /scratch1 [filesystems](#hpc-cluster-filesystem).

Because of limited capacity please keep usage **reasonable and proportional**. Users may be subject to **cleanup requests or per-user quotas**.


### Creating a storage folder
To create a storage folder using the backend.ai interface. 


![test](images/create_new_storage_folder.png)

**Usage Mode**: Sets the purpose of the folder.
* General: Defines a folder for storing various data in a general-purpose manner.
* Models: Defines a folder specialized for model serving and management. If this mode is selected, it is also possible to toggle the folder's copy availability.
* Auto Mount: Folders automatically mounted when a session is created. If selected, the folder name must start with a dot ('.').

The **Folder name** will determine the system path that the directory will be available at in each session.





### Mounting a storage folder
The storage folder can be mounted during the session creation process. 

![test](images/add_storage_folder_to_session.png)
Under the 'Data & Storage' section, folders selected will be available during the session.
## HPC Cluster Filesystem
| Path | Max Disk Capacity|
|---|---|
|`/home/$USER`|100 GB|
|`/project2/$PI_NAME_PROJECT_ID`|Quota dependent|
|`/scratch2/$USER`|10 TB |

These systems may offer:
* Larger capacity
* Better performance
* Project-level data sharing
