# Data Management

There are multiple places to store data on the Topanga system and can be categorized into 3 locations kinds:

* Topanga Filesystem
* Session Specific
* HPC Cluster filesystem

Files can be downloaded from an interactive app to your laptop:
Download: Use the App's "Download" button (in Jupyter file explorer).

## Topanga Persistent Storage
| Path | Max Disk Capacity|
|---|---|
|`/home/folder_name`|1 TB|


For data storage that will be persistent across multiple sessions, create a *storage folder*. Cost is based on active storage so use when you need to temporarily store data between sessions. For longer term data storage, it may be more cost efficent to use the /project2 or /scratch1 [filesystems](#hpc-cluster-filesystem).

Because of limited capacity please keep usage **reasonable and proportional**. Users may be subject to **cleanup requests or per-user quotas**.

### Creating a storage folder
To create a storage folder using the backend.ai interface. 


![test](images/data-management/create_new_storage_folder.png)

**Usage Mode**: Sets the purpose of the folder.
* General: Defines a folder for storing various data in a general-purpose manner.
* Models: Defines a folder specialized for model serving and management. If this mode is selected, it is also possible to toggle the folder's copy availability.
* Auto Mount: Folders automatically mounted when a session is created. If selected, the folder name must start with a dot ('.').

The **Folder name** will determine the system path that the directory will be available at in each session.

### Mounting a storage folder
The storage folder can be mounted during the session creation process. 

![test](images/data-management/add_storage_folder_to_session.png)
Under the 'Data & Storage' section, folders selected will be available during the session.
## Session Specific Data

| Path | Max Disk Capacity|
|---|---|
|`/home/work`|50 GB|

The "home" directory for every session is located at `/home/work`. Data here is temporarily stored on the Topanga filesystem and will be deleted when the session ends. **Do not store important or long-term data here.**

/home/work is not persistent:
What is it? Your desktop environment's default folder.
Rule: Treat this like a scratchpad. Any file saved here vanishes when you destroy the session.
Use for: Cache files, temporary downloads, logs.
Topanga Persistent Storage: e.g. /home/work/storage (a directory you created and mounted during session creation):
What is it? Your virtual hard drive mounted from the cloud storage.
It can use up to 1TB and you can create several directories as long as the total size fits within 1TB.
Rule: This is your Save Location.
Use for: Code repositories, datasets, trained models (model.h5, checkpoint.pt).

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

### Persistent Storage with Virtual Folders

Topanga connects your sessions to persistent storage through **Virtual Folders (vFolders)**. These folders can be mounted into your compute sessions regardless of which compute node the session runs on, making it easier to reuse code, data, and results across sessions. Virtual folders also support sharing and per-user or per-project quotas.

## Uploading Data to Topanga

You can use the web browser UI to transfer a few files into your Topanga directories. For more than a few files,uploading data through the command line or SFTP client is supported with a few extra steps.

### Download SSH Key

You will need an SSH Key for each unique session. To create one first create a file browser session and open up the session info when it starts.


![test](images/data-management/sftp_step1.png)

Click on "See App Dialog" and click on "SSH/SFTP"

![test](images/data-management/sftp_step2.png)
On the bottom right, click "Download SSH Key". You should then see a file named `id_container`.


![test](images/data-management/sftp_step3.png)


### Connecting to your SFTP Session

On the SSH/SFTP App Dialog Menu you will get information about what Username, Host, and port number to use to connect. Unless otherwise directed, the defaults are:

| Host| User| Port|
|---|---|---|
|topanga.carc.usc.edu|work| Unique per session|

#### FileZilla
Open the Site Manager and fill in the Host and Port information provided by the SSH/SFTP Dialog Menu. The path to your `id_container` SSH Key may vary. 
![test](images/data-management/sftp_filezilla_setup.png)



#### Cyberduck

Create a new Bookmark and fill in the Host and Port information provided by the SSH/SFTP Dialog Menu. The path to your `id_container` SSH Key may vary. 
![test](images/data-management/sftp_cyberduck_setup.png)

#### Command line:

```
chmod 600 ./id_container
sftp -i ./id_container -P $PORT_NUMBER -o StrictHostKeyChecking=no -o UserKnownHostsFile=/dev/null work@topanga.carc.usc.edu
```
`$PORT_NUMBER` is given in SSH/SFTP App Dialog Menu



