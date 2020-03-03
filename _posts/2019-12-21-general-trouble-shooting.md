---
layout: post
title: "Troubleshooting"
date: 2019-12-21
categories: general
---

# Troubleshooting

## VirtualBox audio doesn't work after resume from sleep

### Solution

I have a virtual box with windows as guest and host OS. The audio works fine when the guess OS boots for first time. There is no audio on the guest OS when the host OS resumes from sleep. I couldn't fine a proper solution to fix this problem other than restarting the guest OS.

The only other work around I found was to follow below step.

- On the guest OS, right click on the speaker icon and select **Playback devices**
- From the sound dialog, select the speakers and click on the configure button
- From the Speaker Setup dialog, change the audio channels and test. This resets the audio on the guest OS

## VirtualBox access denied error

### Solution

Try opening as administrator

## Mouse clicks doesn't work on second monitor

When using the dual monitors and mouse is set to Integration mode. It won't work for the second monitor. The has no problem.

This got fix after upgrading the VirtualBox to latest version and reinstalling the guest additions.

## Aspx page doesn't display the error details

### Solution

When there are exceptions or errors while opening the aspx pages the complete error details are not displayed. Example 500 error.

Update the web.config file as below

[Source link](https://stackify.com/web-config-customerrors-asp-net/)

```xml
<configuration>
  <system.web>
    <customErrors mode="Off"/>
  </system.web>
</configuration>
```

If its an desktop application (not a web page) which is invoking the aspx page then the error details could be retrieved using below code.

[Source link](https://stackoverflow.com/questions/7261986/how-to-get-error-information-when-httpwebrequest-getresponse-fails)

```c#
catch(WebException exp)
{
  HttpWebResponse resp = exp.Response as HttpWebResponse;
  if (resp != null)
  {
      Stream s = exp.Response.GetResponseStream();
      StreamReader sr = new StreamReader(s);
      Trace(sr.ReadToEnd());
  }
}
```

## SDx fails to login on firefox

### Solution

The SDx fails on the firefox on certain machine because of IP6 and you get **Error: Internal server error has occurred. Please check server logs**. When you check the authentication log file (<Site Folder>\SPFConfigService\SPFAuthentication\STSAuth.log), you can see that it fails in the method IPConfigurationUtilities. This could be because the firefox uses IP6 by default. To disable the IP6 on firefox, go to about:config and set true for the config flag network.dns.disableIPv6.
