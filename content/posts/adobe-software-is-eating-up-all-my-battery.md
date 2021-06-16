---
title: Adobe software is eating up all my battery
date: 2021-06-16T10:37:41+01:00
lastmod: 2021-06-16T10:37:41+01:00
author:
  - C-A de Salaberry
# authorlink: https://author.site
cover: /img/cover.jpg
categories:
  - debug
tags:
  - macOS
  - adobe
  - battery
draft: false
---

Ho to prevent adobe software from eating up all your battery by disabling their sleeper agents.

<!--more-->

## Disable the CoreSync Extension

![](./assets/disable-core-sync-extension-screenshot.png)

1. First start by disabling all your Adobe app to avoid conflicts
2. The CoreSync features of Adobe allows to sync files in the background with their cloud.
Have a look at the extension panel in your system preferences. Untick `Finder Extensions` to disable it.

## Disable LaunchAgents and LaunchDaemons.

We will now make sure that macOS will not relaunch adobe software when we kill them. They live in the following folders:
- `/Library/LaunchAgents/`
- `~/Library/LaunchAgents/` (might not exist)
- `/Library/LaunchDaemons/`

- Go to those folders
- Create a 'Disabled' folder to backup the agents you want to disable in each of these folders
- move all the `com.adobe.**.plist` files in the newly created folder

## Kill the Adobe software

Start your `Activity Monitor`. Make sure to show all running processes in `View > All Processes`.
search `adobe` with the search bar.
select all adobe processes and use the "cross" button in the top bar to kill them.

And voila !

## Notes

Be careful when you install an adobe update, or a new adobe software, the agents might come back.


