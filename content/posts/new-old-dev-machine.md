---
title: "Setting up my new old dev machine"
date: 2023-03-06T11:11:11+01:00
draft: true
categories:
- Personal
tags: 
- dev
- linux
- tinkering
---

# Setting up my new old dev machine

## Getting the Fingerprint Reader to Work

The X270 uses a fingerprint sensor from Validity.
To get the fingerprint sensor installed and working, it was simply just following the instruction see [here](https://github.com/uunicorn/python-validity).

## Configuring Pop!\_OS

Adding the Trash icon to the Dock

Open Terminal and enter the following command

gsettings set org.gnome.shell.extensions.dash-to-dock show-trash true

### Configure the Trackpoint sensitivity

The GUI in Pop!\_OS Settings does not allow to change the TrackPoint sensitivity. Fear not, this can infact be [changed in the terminal](https://github.com/jsborjesson/dotfiles/issues/94) using `xinput --set-prop`.
