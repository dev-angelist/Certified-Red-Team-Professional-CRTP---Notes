---
description: Lab Instructions
icon: wrench
---

# 0 - Lab Instructions

## Lab Instructions

• You can use a web browser or OpenVPN client to access the lab. See the 'Connecting to lab'\
document for more details.\
• All the tools used in the course are available in C:\AD\Tools.zip on your student machine.\
However, please feel free to use tools of your choice.\
• Unless specified otherwise, all the PowerShell based tools (especially those used for\
enumeration) are executed using InviShell to avoid verbose logging. Binaries like Rubeus.exe\
may be inconsistent when used from InviShell, run them from the normal command prompt.\
• The lab is reverted daily to maintain a known good state. The student VMs are not reverted but\
still, please save your notes offline!\
• The lab manual uses a terminology for user specific resources. For example, if you see studentx\
and your user ID is student41, read studentx as student41, supportxuser as support41user and\
so on.\
• Your student VM hostname could be dcorp-studentx or dcorp-stdx.\
• Please remember to turn-off or add an exception to your student VMs firewall when your run\
listener for a reverse shell.\
• The C:\AD directory is exempted from Windows Defender but AMSI may detect some tools\
when you load them. The lab manual uses the following AMSI bypass:

If you want to turn off AV on the student VM after getting local admin privileges, please use the\
GUI as Tamper Protection incapacitates the 'Set-MpPreference' command.\
• Note that we are using obfuscated versions of publicly available tools. Even if the name of the\
executable remains the same, the tool is obfuscated. For example, Rubeus.exe in the lab is an\
obfuscated version of publicly available Rubeus.\
• Note that if you get an error like 'This app can't run on your PC' for any executable (Loader.exe,\
SafetyKatz.exe or Rubeus.exe), re-extract it from C:\AD\Tools.zip
