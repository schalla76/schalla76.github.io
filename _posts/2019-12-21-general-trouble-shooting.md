---
layout: post
title: "Troubleshooting"
date: 2019-12-21
categories: general
---

# Troubleshooting

## Virtualbox audio doesn't work after resume from sleep

### Solution

I have a virtual box with windows as guest and host OS. The audio works fine when the guess OS boots for first time. There is no audio on the guest OS when the host OS resumes from sleep. I couldn't fine a proper solution to fix this problem other than restarting the guest OS.

The only other work around I found was to follow below step.

- On the guest OS, right click on the speaker icon and select **Playback devices**
- From the sound dialog, select the speakers and click on the configure button
- From the Speaker Setup dialog, change the audio channels and test. This resets the audio on the guest OS
