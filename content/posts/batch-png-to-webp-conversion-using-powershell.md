---
title: "Batch Png to Webp Conversion using Powershell"
date: 2023-05-21T23:18:11+05:30
draft: false
subtitle:
tags:
  - Powershell
  - Scripting
  - Webp
  - Png
  - Jpeg
  - Tiff
---

Compressing PNG images to WebP, especially for serving web content, can result in a lot of bandwidth and time saved if your website or app has a lot of images. Google provides PNG to WebP(the tool also encodes JPEG and TIFF to WebP) and WebP to PNG conversion tools at [this page](https://developers.google.com/speed/webp/docs/precompiled). However the tools work from the command line and take only one file as input. Let us try to speed this up with Powershell.

First, download the tools and extract them to a folder libwebp. Be sure to not include dots(.) in the extracted folder path as this seems to cause issues with Windows locating them via Path variables. Next, go to environment varibles by searching for them in the start menu and go to the environment variables page.

{{< figure src="/images/environment-variables.png" alt="Environment Variables Dialog Box" width="400px" >}}

Then, go to the Path environment variable under user environment variables, and add the location of the bin subfolder of the folder you just extracted from the zip file by clicking on New and either typing out the path name or browsing to the folder. For me, it was ```C:\Programs\libwebp\bin```.

{{< figure src="/images/path-environment-variable.png" alt="Path Environment User Variable" width="400px" >}}

Now, lets get on to the scripting part. First of all, search for Powershell ISE in the start menu and open it. A new script should already be opened. If it's not, go to File-->New and open a new script. Type or Paste the following in the script below:

{{< highlight powershell >}}
Function webp-to-png {
    $names = gci -fi *.webp | Select-Object -Property Name, Basename
    if (-not ($names.length -eq 0)) {
        foreach ($name in $names) {
            dwebp.exe $name.Name -o ($name.Basename + ".png")
        }
    }
}

Function png-to-webp {
    $names = gci -fi *.png | Select-Object -Property Name, Basename
    if (-not ($names.length -eq 0)) {
        foreach ($name in $names) {
            cwebp.exe -q 90 $name.Name -o ($name.Basename + ".webp")
        }
    }
}

New-Alias webp2png webp-to-png
New-Alias png2webp png-to-webp
{{< / highlight >}}

Now, save the file as ```Microsoft.PowerShell_profile.ps1```  in the folder ```C:\Users\<<Your username here>>\Documents\PowerShell```. Replace <<Your username here>> with your Windows username. Saving in this location adds the commands to your Powershell profile, which means you can use it everytime you start a new powershell window from now on just by typing ```webp2png``` OR ```png2webp```. A detailed discussion of Powershell profiles can be found [here](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_profiles?view=powershell-7.2).

That's it! Now you can just go to any folder in Powershell and just type ```png2webp``` to convert all PNG files within that folder to WebP. You can also convert WebP files to PNG by typing ```webp2png```.

>Note: If you're already using this Powershell profile to save some of your own commands, be sure to not overwrite the profile, just open the file and add the script at the end.
