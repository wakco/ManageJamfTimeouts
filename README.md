# ManageJamfTimeouts
A little signed package for ADE/DEP Jamf PreStage only deployment that attempts to monitor the Jamf enrolment process.

The package contains three files, where two are deployed, and the postinstall script is executed at the end of the deployment, to start the managejamftimeout script with the LaunchDaemons plist file so the installer can finish while monitoring jamf gets started.

- postinstall
  - executes /bin/launchctl bootstrap system /Library/LaunchDaemons/nz.ac.waikato.managejamftimeout.plist
- nz.ac.waikato.managejamftimeouts.plist
  - run at load configured launchd plist to start /usr/local/bin/managejamftimeout
- managejamftimeout
  - zsh script that does all the work, it will monitor the jamf processes until it sees a jamf process for **postMdmEnrollment**, at which point it will end, delete itself, delete the plist, and remove the LoginWindowText it has been using to notify of its status (it will periodically reload the loginwindow process for this).
