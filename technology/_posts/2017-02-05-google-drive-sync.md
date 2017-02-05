---
layout: post
title: Syncing Google Drive folders with local ones
category: technology
date: 2017-02-05
---

I'm not a huge fan of staying signed in to Google Chrome/Chromium. However, as far as I can tell, you need to stay signed in to use the 'Save to Google Drive' addon for Chrome. Which is rather disappointing. My main use case was browsing for papers on my laptop and saving them to Google Drive directly. Downloading and reupladung was too much effort.

## Yay for command line utilities

There are, thankfully, a bunch of command line utilities that let you sync to Google Drive. I decided to go with grive (https://github.com/Grive/grive) - it seemed to fit my use case perfectly.

grive provides a command to sync with your entire Google Drive, or a bunch of subfolders. I prefer only syncing certain subfolders (too much data on Drive, and I don't need all of it on my local machine)

## Manually sync? Ugh

Yeah, it's just as much effort to run a command line program every time I want to save a newly downloaded file to Google Drive. Enter 'inotifywait'. It lets you monitor certain directories for changes, and execute a command if something changes (for instance, a file modification or write). So I wrote a tiny script that sets up inotifywait to run the grive sync operation every time there's a write somewhere in the folder (https://github.com/mayankbh/useful-scripts/tree/master/grive\_syncer)

I'll broadly explain the various flags and seemingly (and truly) random arguments I pass in -
* -r : Recursively monitor subdirectories
* -e create : Wait for create operation
* --format "%w\t%f" : inotifywait prints out the name of the newly created file/modified folder in this specified format (man pages probably explain this better)
* --exclude : I used this one to ignore certain types of files, for instance, swap files (this is probably not perfect, but this works for my use case - saving PDFs
. : The folder to monitor (everything in the current directory, I should really switch this into a command line argument instead, but I'm lazy)

I pipe the output of itnotifywait into the read INPUT command, this process runs in a loop. Each loop iteration, I extract the filename and folder (this was mainly for testing/logging purposes, but I'm sure you could do some neat things with those variables) and then run the grive sync operation on the modified folder.

## Backgrounding it

It's kinda boring to keep a terminal dedicated to this command. I guess you could use nohup, redirect stdout/stderr to /dev/null etc, but what works for me is to navigate to the folder I want to keep synced, run the setsid ./syncher.sh command, which basically makes it its own process group (as best as I understand it) and means it doesn't die even if you kill your terminal emulator.

## Syncing folders

So I normally create specific folders with the same name as those in my Google Drive - this ensures I don't accidentally pull down everything in my Google Drive storage onto my local machine. I'm not sure if this pushes newly created folders into Google Drive as well, though. Never needed to test it, even though it's incredibly easy to test.

So there you have it. The hipster way of saving files to Google Drive without staying logged in on Chrome (There is most definitely an easier way, but it was nice to work with a few new tools and ideas while setting this up)
