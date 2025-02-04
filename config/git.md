---
description: Git integration for PowerShell Universal.
---

# Git

PowerShell Universal is capable of synchronizing the configuration scripts with a remote git repository. You can use the [Configuration settings](settings.md) to setup git sync.

## Preparing for the first Git Sync

PSU Git sync does not handle merge conflicts automatically and you should ensure that you prepare for the first sync before configuring it. 

### Branch

By default, PowerShell Universal will sync with the `master` branch. If you wish to use a different branch, specify the `GitBranch` setting within your `appsettings.json`. 

### Authentication

You will need to configure authentication to your remote git repository. We recommend a personal access token.

### Method One: Pushing Local Files to Remote

{% hint style="info" %}
The default location for the local repository is `C:\ProgramData\UniversalAutomation\Repository`
{% endhint %}

You must ensure that your local folder is not a pre-existing git repository. Your repository directory should be simply a folder with your PowerShell Universal files. If a `.git` folder exists, and you do not wish to use this repository, you should delete it and PowerShell Universal will create a new repository.

If you are populating a new git repository, you need to ensure that the remote does not have any files in it.  It should be a completely bare repository. For example, when creating a repository on GitHub, do not select a license or readme.md file to be created. 

![](../.gitbook/assets/image%20%28283%29.png)

Once the repository has been created, you can retrieve the git remote URL and provide that to the PowerShell Universal `appsettings.json` file. 

![](../.gitbook/assets/image%20%28284%29.png)

Once you have the branch, git remote URL and credentials, you can provide them to your `appsettings.json` file and the git remote will be populated with your local files. 

### Method Two: Pulling from a Git Remote

{% hint style="info" %}
The default location for the local repository is `C:\ProgramData\UniversalAutomation\Repository`
{% endhint %}

You can configure PowerShell Universal to pull from a git remote. In this configuration, you must ensure that your local repository folder is completely empty. Any files within the folder will cause the git sync to fail and prevent it from recovering. 

Configure the `appsettings.json` file to include the branch, credentials and git remote URL that you are cloning. Once the fields have been set, you can start the PowerShell Universal service. The first thing the service will do is clone the repository and configure it locally. 

## Included Files

The files that are included with a git sync are any files within the local repository. This includes PS1 configuration files, pages XML files and any other files you may add manually. 

The following are not included: 

* appsettings.json
* database.db
* web.config
* PowerShell Universal Application Binaries

## Git Status Page

The git status page lists the synchronized repository, branch and the list of commits that have occurred. 

![](../.gitbook/assets/image%20%28275%29.png)

## Git Synchronization Behavior

### Two-Way

By default, git synchronization will work both ways. If you make changes within the PSU admin console, those changes will be committed and sync'd to the configured remote.

Any changes that are made in the remote will be pulled locally.

If you setup git sync with a preexisting git remote, the changes will be pulled and synchronized locally. You cannot have any changes locally.

If you setup git sync with a bare repository, the local changes will be sync'd and the repository will be initialized.

### One-Way

You can adjust the git synchronization behavior by changing the `GitSyncBehavior` setting in `appsettings.json`. When set to `OneWay`, the admin console and management API will become read-only. The PowerShell Universal system will pull from the remote but will never push or commit locally.

```javascript
    "Data": {
    "RepositoryPath": "%ProgramData%\\UniversalAutomation\\Repository",
    "ConnectionString": "%ProgramData%\\UniversalAutomation\\database.db",
    "DatabaseType": "LiteDB",
    "GitRemote": "https://github.com/myorg/myrepo.git",
    "GitUserName": "any",
    "GitPassword": "MYPAT----------------"
    "GitBranch": "dev",
    "GitSyncBehavior": "OneWay",
    "ConfigurationScript": ""
  },
```

## Personal Access Token

We recommend that you use a personal access token \(PAT\) over a user name and password. You can configure a personal access token by setting the password property in the `appsettings.json` or other configuration methods.

```javascript
    "Data": {
    "RepositoryPath": "%ProgramData%\\UniversalAutomation\\Repository",
    "ConnectionString": "%ProgramData%\\UniversalAutomation\\database.db",
    "GitRemote": "https://github.com/myorg/myrepo.git",
    "GitUserName": "any",
    "GitPassword": "MYPAT----------------"
  },
```

In GitHub, you can retrieve a personal access token by clicking your avatar in the top right, selecting Settings, Developer Settings and then Personal Access Tokens. 

When generating your access token, ensure that you select the Repo permissions. 

![](../.gitbook/assets/image%20%28282%29.png)

## User name and password

You can also configure a git remote to authenticate with a user name and password. Set the user name and password either with the `appsettings.json` file or another configuration method.

```javascript
    "Data": {
    "RepositoryPath": "%ProgramData%\\UniversalAutomation\\Repository",
    "ConnectionString": "%ProgramData%\\UniversalAutomation\\database.db",
    "GitRemote": "https://github.com/myorg/myrepo.git",
    "GitUserName": "myusername",
    "GitPassword": "mypassword"
  },
```

## Benefits of Git Sync vs Manual Git Sync

It is possible to manually git sync a repository. PowerShell Universal uses very basic commands when dealing with git. Any changes made to PowerShell Universal through the admin console or API invoke a `git commit` and the author is set to the identity of the user making the change. During a git sync operation, we first perform a `git pull` to ensure that we have the latest version of files on the remote. Next, we perform a `git push` to push up local commits that have happened since the last sync. 

You could achieve this functionality with a scheduled job. 

That said, one feature of the git sync is that it analyzes the commit to ensure that only files that were changed during the sync are reloaded. This will stop dashboards from auto-deploying when they haven't changed or APIs services from restarting when environments haven't been updated. So there is a performance gain here. 

The other issue is that due to the way that PowerShell Universal watches files \(with a `FileSystemWatcher`\) and the way that git updates files, the configurations will not reload automatically after a pull. You will have to ensure that you force the configurations to be reevaluated. 

