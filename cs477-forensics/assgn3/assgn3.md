# CS 477 Assignment 2 - Registry Exploration

## Robert Detjens

---

### Findings

The Windows registry contains a wealth of information recording what has been done on a computer. Investigating the
registry on Yeat's HDD image will reveal many aspects of what this laptop was used for.

We can tell that there was a no-name / generic flash drive entitled "Ubuntu srv" [Fig. 1] mounted as `D:\` on the system
[Fig. 2]. There was an additional SanDisk flash drive "Easter 1916" on the system as well, `E:\`.

![Records of USB devices from `HKLM\SOFTWARE\Microsoft\Windows Portable Devices`](images/winportabledevices.png)

![Mount points of removable drives from `HKLM\SYSTEM\MountedDevices`](images/mounteddevices.png)

"Ubuntu srv" was first used on November 23, 2020 and last seen on /March 8 2021 [Fig. 3].

![Time of first install from `HKLM\SYSTEM\ControlSet001\Enum\USBSTOR`](images/usbstor.png)

\pagebreak

Examining the list of installed programs, the `yeatsw` user has installed the following programs:

- Mozilla Thunderbird, an email client
- Tor Browser, an anonymizing internet browser

These installers were recorded in the Compatibility Assistant [Fig. 4], the program uninstallation registry [Fig 5.],
and Firefox registry entries [Fig. 6].

![`NTCU\SOFTWARE\Microsoft\Windows NT\CurrentVersion\AppCompatFlags\Compatibility Assistant`](images/compatassistant.png){ height=150px }

![`NTLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Uninstall`](images/uninstall.png){ height=150px }

![`HKCU\SOFTWARE\Mozilla\Firefox\Launcher`](images/tordesktop.png){ height=150px }

---

There are also several registry keys missing from the disk snapshot that would be of interest to an investigator:

- `NTCU\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\RunMRU` for user `yeatsw`
- `NTCU\SOFTWARE\Microsoft\Windows\CurrentVersion\RunOnce` for user `yeatsw`
- `NTCU\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\ComDlg32` for user `WillyB`
