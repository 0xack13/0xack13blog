---
title: "Azure Devops Ssh Tasks"
date: 2019-06-26T14:49:47-04:00
draft: true
---

I took me some time to figure out that there is a difference in the type of the supported SSH keys in the task SSH to run a shell command or to copy files over SSH. It turns out that copy files over SSH supports PEM files which can be generated using the following command:

```bash
[azure@remotelinux ~]$ ssh-keygen -t rsa -m PEM
```
Using the generated private key, add the information to the `Service Connections` in the `Project Settings`.

The task should look like the below:

```bash
- task: CopyFilesOverSSH@0
  inputs:
    sshEndpoint: 'azurevm'
    sourceFolder: '$(Agent.BuildDirectory)'
    contents: '**'
    targetFolder: '/home/azure'
```

The SSH task to execute shell commands support default types of OpenSSH private keys which don't necessiraly have to be PEM files. It's also worth mentioning that the PEM files are support by the SSH tasks to execute shell commands. So it's safe to say that you can also use PEM files for any SSH tasks.

```bash
- task: SSH@0
  inputs:
    sshEndpoint: 'azurevm'
    runOptions: 'commands'
    commands: 'ls -alt'
```