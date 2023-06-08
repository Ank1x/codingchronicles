---
title: "Bash to Powershell - Part 1"
date: 2023-05-16T16:12:51+05:30
description: Commands translated from Linux Bash to Cross-platform Powershell
tags: 
  - Powershell
  - Bash
draft: true
---

{{< figure src="/images/icons8-bash-to-powershell-3.png" alt="bash to powershell" height="64px" width="192px" >}}

Linux users have long been accustomed to using their terminals for everything from simple tasks like navigating the file system to complex tasks like administering servers. Windows users, on the other hand, have traditionally been limited to the Windows Command Prompt, which is a clunky and outdated tool.

However, all of that changed with the introduction of PowerShell. PowerShell is a powerful task automation tool and a scripting language that allows you to automate tasks and manage Windows systems from the command line. It is similar to Bash or Zsh in terms of features.

In addition, with the introduction of Powershell 6.0, it has become cross-platform, allowing you to use the same commands on Linux, Windows and MacOS.

In this series, I will document learning Powershell commands to do mostly simple stuff that Linux users do in bash. I will also include several use cases whenever possible. 

In case anyone wants to follow along, I will be using Powershell 7.2(LTS) version, but the commands should work on versions 7.1 and higher. You can find the installation instructions for Powershell [here](https://learn.microsoft.com/en-us/powershell/scripting/install/installing-powershell). All the Bash commands are tested on Ubuntu 22.04 running in WSL 2 under Windows 10 22H2. All Powershell commands were tested under the same WSL configuration and on Windows 10 22H2.


 I have changed the default font of Powershell to 'Cascadia Code' by clicking on the top bar and going to Defaults followed by selecting the Font tab. To do this first you need to take ownership of the shortcut in Windows 10. You can also adjust the width and height of the default Powershell Window here if you wish to. Changes take effect when you exit and restart Powershell. Please note that Linux is used here interchangeably while referring to the Bash terminal.

## Basic Files and Folders Listing

Linux users are no doubt familiar with the ```ls``` command to list contents of the current directory. On Powershell, this command is known as ```Get-ChildItem```. However, it doesn't display only the folders in a row based format like the ```ls``` command does (on Linux), instead this command is more equivalent to the ```ls -al``` command on Linux. Also, note that you can actually use the ```ls``` command on Powershell as a shorthand or an alias for ```Get-ChildItem```. But, for cross-platform compatibility, we will prefer the other alias for ```Get-ChildItem```, which is ```gci```.

>Note: To get a list of all aliases for Powershell commands, enter ```alias | Out-File aliases.txt``` in Powershell, and then you can open the ```aliases.txt``` file in Notepad or any other text editor.

- The only way to get a result similar to the base ```ls``` command on Linux is to pipe the output of the ```Get-ChildItem``` command to another command ```Format-Wide``` for formatting the output in a horizontal fashion. See [this](https://superuser.com/q/1325217/1799623) thread for details. We can also use the alias ```fw``` for ```Format-Wide``` to shorten this even further. The full command is ```Get-ChildItem -Attribute Directory | Format-Wide Name -Autosize```.

- In Linux, files and folders starting with ```.``` are hidden by default and are ignored by ```ls```. To include these hidden items, we need to use the ```-a``` option. To achieve a similar result on Windows to view all files, including hidden and system files, we need to use the command ```Get-ChildItem -Force ```, which can be shortened to ```gci -fo```.

>Tip: In PowerShell, when using commands with options, Powershell attempts to match options based on the starting letters you provide. For instance, if there are multiple options for a command that begin with the same letter, such as ```Filter``` and ```Force``` options for the ```Get-ChildItem``` command starting with 'F', you need to specify an additional letter to disambiguate them. Therefore, if you use ```Get-ChildItem -F```, it will not work, but ```Get-ChildItem -fo``` will successfully match the Force option.

- If you only want to show subdirectories under the current directory, in Linux you can use ```ls -d */```, the same command in Powershell becomes ```Get-ChildItem -Attributes Directory``` which can be shortened to ```Get-ChildItem -Directory``` which can be further shortened to ```gci -ad```. 

You can also use multiple Attribute combinations, for example, ```!Directory,Hidden``` will search for regular and hidden files. Here the !(NOT) operator before Directory allows you to list files and the ,(OR) operator with the Hidden attribute also adds hidden files to the list. For detailed usage, please see the Microsoft [documentation](https://learn.microsoft.com/en-us/powershell/module/Microsoft.PowerShell.Management/Get-ChildItem?view=powershell-7.2#example-1-get-child-items-from-a-file-system-directory).

- The ```-R``` option for ```ls``` allows you to list subdirectories recursively. The Powershell equivalent is ```Get-ChildItem -Recurse```, which can be shortened to ```gci -R```.

- What if you wanted to filter only .txt files from the recursive listing. Well, in linux you have to pipe the output of ```ls``` to ```grep```, while Powershell offers a ```-Filter``` option for ```Get-ChildItem``` to do that, resulting in the command ```Get-ChildItem -Recurse -Filter *.txt```, or ```gci -r -fi *.txt```.

>Warning: Using the ```ls``` command in Linux executes the default ```ls``` command provided by the Linux operating system, even inside Powershell. So if you are writing cross-platform Powershell scripts, use ```gci``` instead of ```ls```.



|  Bash  |  Powershell   |
|----------------------|--------------------------------------|
| ```ls``` | ```gci -ad \| fw Name -A``` |
| ```ls -l``` | ```gci```|
| ```ls -al``` | ```gci -fo```|
| ```ls -d */``` | ```gci -ad``` |
| ```ls -alR``` | ```gci -r``` |
| ```ls -alR \| grep .txt``` | ```gci -r -fi *.txt``` |


