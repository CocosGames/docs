# GitSync (PA & Above)

### Requirements

* Available only in **Powered Ascent** Plans and Above.

## Setup your Config File

To configure git sync you will need to add the following ```arena.gitsync.json``` JSON file to the root folder of your Server Code and `Deploy` it to your application. 

!!! NOTE   
    - **REMINDER** you must click `Deploy` to push the updated ```arena.gitsync.json``` to your application servers before the GitSync service will see the changes.

The syncing service will pull your Git repository for a new check-in in the requested branch approximately every 2 minutes. 

## Example of arena.gitsync.json
```
{
    "serverDir": "upload",
    "buildType": "arena",
    "gitServer": "github.com/Lucid-Sight-Inc/testsyncrepo.git",
    "gitUser": "USER",
    "gitPassword": "PASS",
    "branch" : "testbranch",
    "redeployOnChange": true,
    "overrideGitURL": "",
    "repoReset" : false
}
```

**JSON Attributes:**

- **serverDir:** This is the location from the root of your repo of where the upload code exists. If you will be using 'npm run build' to create your deployment code then put the directory where this command will output your files.

- **buildType:** You have two options for this attribute. 
    - `arena` - This will run ```npm install && npm run build``` before attempting to copy files from the serverDir. 
    - `none` - Directly copies files from serverDir folder without running any build commands. Use this option if your server code is already complied and ready to run on Arena Cloud before your check-in.

- **gitServer:** Your git repo url.

- **gitUser:** A user account that has at least read access to your repo.

- **gitPassword:** Password ***(Password MUST BE URL encoded if it has any special characters)***

- **branch:** The branch name to pull from.

- **redeployOnChange:** If true, your new code will be immediately deployed to your game servers and they will be restarted, ending any existing games on those servers (Graceful rolling updating will be implemented in the next few weeks).

- **overrideGitURL: ** For any non-standard / non-https URLs for your repo (We do not recommend using this option).

- **repoReset:** This will force delete the local copy of your repo on your git sync server. This is useful if you are changing the REPO to point to a new location. You will need to keep it on for one update cycle, then it can be turned off.

## Checking Status

You can check on the status of your sync or look for any errors using the ***Deployments*** section of your application dashboard. Select logs next the to the server label **Git Sync Service** to see recent logs / errors.

![Arena Application Management View](../../images/git-sync-logs.jpg)

## Troubleshooting
If you have a merge conflict or other unknown / critical error in the sync process we recommend you select **Restart** from the deployments screen for the **Git Sync Service**. Restarting the GitSync service will clear the local repo and pull a fresh copy upon restart.

If you are not including a `arena.env` in your Git Repo do be aware that the file created on the Source Code section of the dashboard will be removed on every new Git Sync. If you would like to keep a file that persists you will need to create and deploy `arena.secret.env` file. This file is stored permanently and cannot be deleted via the source code dashboard, it can only be overwritten. To have this file deleted you must open a support ticket, to clear information on the file just deploy a blank version of it.
 
