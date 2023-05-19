---
title: "Annoyances of Microsoft Edge"
date: 2023-05-19T14:05:09+05:30
draft: false
subtitle:
tags:
  - Annoying
  - Microsoft
  - Edge
  - Microsoft Edge
---

My default browser, almost continuously since I started using it, has been Google Chrome. I will, however, sometimes open my Outlook account in Edge or open it to test some webpage I am working on. Edge will occasionally annoy me with a popup asking to be the default browser, even though I have closed the notification before. Just one of the many reasons why I will never use Microsoft Edge as my default browser (as long as Google Chrome exists).

With the new update(111), Microsoft Edge has decide to take things one step further into annoying the user, by installing a desktop search bar that nobody agreed to install. 

So far, it is quite easy to get rid of these annoyances by adding a few registry entries, until Microsoft decides it doesn't want to respect the user's settings again and ignores these entries. But for now, here are the commands you need to enter in an *elevated* Powershell. The first entry might return a notification saying the key already exists, if it does, just move on to command 2.

{{< highlight powershell >}}
New-Item -Path HKCU:\SOFTWARE\Policies\Microsoft -Name Edge
New-ItemProperty -Path HKCU:\SOFTWARE\Policies\Microsoft\Edge -Name DefaultBrowserSettingEnabled -PropertyType DWord -Value 0
New-ItemProperty -Path HKCU:\SOFTWARE\Policies\Microsoft\Edge -Name WebWidgetIsEnabledOnStartup -PropertyType DWord -Value 0
{{< / highlight >}}

