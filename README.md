# initpi
## Overview
This is a shell script called `initpi` that is designed to automate the secure setup of the Raspberry Pi through a user interface (UI) or and/or an optional configuration file `initpi.conf`.  The user modifies the contents of `initpi.conf` for `initpi`, forcing a UI on select fields if desired.  If no `initpi.conf` is found, or if `initpi.conf` is unchanged, UI is implemented for all settings.  `initpi` deletes `initpi.conf` automatically after each use.  `rc.local` is used to invoke `finish` to finalize configuration after a forced reboot at the end of `initpi` run.  Saving a configuration file for future use by copying it prior to the run (see **Security** section) allows the user to repeat the same setup over and over or across different systems, but comes with obvious risk.  Encrypting copies of `initpi.conf` is highly recommended.

### Short instructions:
1. Download all files to a single directory
1. Optional: Edit `initpi.conf` *(unedited or absent `initpi.conf` file forces user interface)*
1. Optional: Save a copy of edited `initpi.conf`
1. Run `initpi` as root
1. Login via SSH or otherwise *(if a revert timer is set, the system will revert to original settings after the timer expires)*

### The following can be configured:
* timezone
* new username
* new password
* WiFi SSID
* WiFi WPA key
* SSH public key
* SSH port number

Additionally, a firewall is set up with the proper rules to allow the SSH port through, and considerations are made for SSH configuration via keys only (no password access).  A system update and full upgrade are also performed.

## Security
Obviously saving passwords in plain text is not wise, so `initpi` deletes `initpi.conf` after execution.  I want to automatically encrypt the configuration file to allow it to be safely stored for future use and avoid the need to delete it, but for now copying the configuration file and securing it are at the user's discretion and must assume the risk.

### Mitigating security risks (most effective first):
* Don't save `initpi.conf` at all, re-enter information for each run in UI
* Force user interaction on certain fields only using `SETTING=?` (i.e.: passwords)
* Use temporary passwords for new users, or omit to disable and change manually afterward
* Encrypt copies of `initpi.conf`
* Keep copies of `initpi.conf` off the internet, even if they are encrypted
* Keep copies of `initpi.conf` only visible to you or root if you wish for it to remain on the machine

Any security considerations are welcome, while keeping in mind that this program is not meant to be used on sensitive systems, just hobbyists who brick their system a lot or are always resetting it to try something new.

### Use restrictions:
The program prevents certain combinations of options that would otherwise lock you out, such as a new user with a disabled password but no SSH key provided.  UI requires password confirmations.

## To-do list:
1. Add any necessary error handling
1. Analyze flow for password variables to reduce exposure (i.e.: turn off logging during UI)
1. Possibly divide each action into it's own file for modularity (i.e.: a separate file for SSH setup)
1. Improve overall syntax, clean up code and appearance

## Future vison
* Expand this tool to help automate deployment of multiple systems (TBD).
* Combine with custom NOOBS tool which I created but lost and have to recreate.  That way the NOOBS file you use to flash your SD card will be configured automatically (note: using this method, the NOOBS zip file could be password protected, which would further secure the credentials)
