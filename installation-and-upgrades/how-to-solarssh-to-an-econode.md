# How to SolarSSH to an Econode

![](../.gitbook/assets/0.png)

HowTo: SolarSSH to an Econode

v.2025.08.15a

Overview:

This document aims to provide the steps on how to SolarSSH into a remote Econode and administer the plugins attached. There are 3 methods to connect remotely to an Econode:

* SolarNetwork Web app
* SolarSSH desktop command line tools
* Hologram Spacebridge

This document deals with the first two methods, whereas the Hologram method is described here:

[Using Hologram Spacebridge to access an Econode](https://docs.google.com/document/d/15lvWFZ_eNKdUezsihYsFUvqDcm4T_talaQMYp6cEuT4/edit?usp=sharing)

Tools required:

* Linux Laptop with Chrome or Firefox browser, and a bash command-line SSH client
* SolarNetwork SSH certificate installed
* Stopwatch on a smartphone

Information required:

* Token and Secret for SolarNetwork (found in the password support Google Doc)
* Credentials for your Econode ID (there are two of them, the OS and the Admin GUI)
* Windows Subsystem for Linux: [https://docs.microsoft.com/en-us/windows/wsl/install-win10](https://docs.microsoft.com/en-us/windows/wsl/install-win10)
* SolarSSH Web App: [https://go.solarnetwork.net/solarssh/index.html?nodeId=550](https://go.solarnetwork.net/solarssh/index.html?nodeId=550)

SolarNetwork Web app Step-by-Step process:

The following steps allow you to connect to an Econode that is connected to the internet. Note that you are accessing a computer over an encrypted connection and possibly over a slower mobile 3G/4G connection. GO SLOW with everything you do - take care with the passwords and the keyboard entries you make, it will be faster in the end.

Step &#x31;**:** Make sure your browser software has the SolarNetwork certificate installed.

This step is covered here: [How to Maintain an Econode Remotely](https://docs.google.com/document/d/1bWTDfqlbR4rFWfawf-yT2ACnsQZ5sGrP_Oc7TUCMOac/edit)

Step 2: Go to the SolarSSH client endpoint for your Econode:

https://go.solarnetwork.net/solarssh/?nodeId=359

Note that the target SolarNode ID is at the end of this URL

Step 3: Fill in the Token and Secret fields in the upper right to authenticate with SolarNetwork

![](<../.gitbook/assets/1 (5).png>)

You need to look in the Dev Passwords Google Sheet. Be slow and letter-perfect in copying and pasting these values one at a time into the fields. The Secret will not show the value you paste, but a line of dots.

Step 4: Now, click the yellow Connect button

Step 5: Fill in the credential for the OS password for that Econode by selecting the value from the Dev Password list using Copy (Control-C or Right Click and Copy - do not Cut or Control-X! If you do, try Control-Z to reverse what you just did) and Paste using the right-click button on your mouse and choosing Paste

![](<../.gitbook/assets/2 (4).png>)

![](<../.gitbook/assets/3 (3).png>)

Remember, there are two kinds of user/password credentials. One is to access the SolarNode OS (operating system) via a command-line terminal. The user name is always (lowercase):

solar

And the password is specific to the SolarNode on the Google Sheet. You will not see the password value - just dots.

The other set of credentials is for the web GUI Admin interface. The username for this tool is always:

[itsupport@ecogysolar.com](mailto:itsupport@ecogysolar.com)

And the password is specific to the SolarNode and specified on the Google Sheet.

Step 6: Wait - it could take 5 minutes before you connect to the Econode, so worth setting a timer

![](<../.gitbook/assets/4 (1).png>)

Step 7: If successful, you will see the SolarSSH client connect to the Econode with this screen, giving you command line access to the Econode in what is called a Terminal.

![](<../.gitbook/assets/5 (2).png>)

Step 8: Try watching the log file

Working with the terminal can be awkward, so click into the black part to make sure the cursor is actually active in that command line terminal. You know the mouse cursor is in the Terminal when the white lined box (see above) becomes a solid white rectangle:

![](<../.gitbook/assets/6 (2).png>)

Type the following command all lowercase:

**sn-log-tail -f**

And click Enter on your keyboard

![](<../.gitbook/assets/7 (3).png>)

You should see a changing list of log events scroll by as they happen on the node. You are looking for the log entry that shows that the SolarNode has connected on the /dev/tty/USB0 device (the RS-485 adapter) to the meter. Such a reading should look like this:

![](<../.gitbook/assets/8 (1).png>)

You should see common properties that the meter or inverter is exposing - this means that you have connected to the Econode and are watching it grab data from the kWh meter. What you are seeing in terms of values is what an electrician should see on the meter’s LCD screen itself.

Step 9: exit watching the log file by pressing Control-C (Control key and the letter C key at the same time). This should leave you back at the command Terminal. Note that if you leave this Command Terminal for a few minutes, like a screensaver, it will _log you out of your SolarSSH session automatically_ and request that you connect again, where you must supply the OS password again for the user:

solar

Click the yellow Connect button again to supply this password

![](<../.gitbook/assets/9 (2).png>)

You will remember that the OS password for user solar is in the Dev Password Google sheet for that NodeID. This automatic logout is for security reasons - it is likely that you will have to do this during a SolarSSH sesson.

Step 10: click the yellow Setup button to access the Admin GUI

This will launch a new browser tab where it will show you the SolarNode web Admin GUI (like a webpage)

Step 11: Click on the Settings menu item

![](<../.gitbook/assets/10 (1).png>)

Step 12: Fill in the credentials for this Econode - the user will always be

[itsupport@ecogysolar.com](mailto:itsupport@ecogysolar.com)

The password will be in the Dev Passwords Google Sheet for that SolarNode ID

And click the Login button

![](<../.gitbook/assets/11 (1).png>)

Step 13: Click on the Manage button for each of the two plugins:

Modbus Serial Connection

![](../.gitbook/assets/12.png)

SunSpec Power Meter

![](../.gitbook/assets/13.png)

To check or set their properties. Remember to make a change, use the TAB key after making a value change, and THEN click the Save button.

Step 13: Click on the red End button

This ends your SolarSSH session, but you can reconnect by leaving the Token and Secret values as is and clicking Connect again

**SolarSSH Desktop Tools Process**

,For an SSH-enabled desktop this is the layout of how you access different host computers and devices remotely via the solarssh infrastructure:

![](<../.gitbook/assets/14 (1).jpeg>)

The syntax an example command line to dial into node 350 for example is:

ecosuite-ssh 350 -L8888:localhost:8080 -L8889:192.168.6.1:80 -L8890:10.10.17.15:80 -L8891:10.10.17.16:80 -L8892:10.10.17.17:80

Step 1: add ecosuite-ssh and ecosuite-sftp functions to your shell profile

The method of configuring your shell profile depends on the shell your workstation is configured to use. Linux workstations typically default to bash and can be configured in a \~/.bashrc file, while macOS defaults to zsh and can be configured in a \~/.zshrc file. Open a shell terminal (on macOS this is the Terminal app) and edit the appropriate configuration file using a command line editor like nano, like this for bash (Linux):

nano \~/.bashrc

or this for zsh (macOS):

nano \~/.zshrc

This will open the file with many environmental variables and other settings and default functions for your user. Scroll to the very bottom and copy and paste the following two functions into the bottom of the file. Note that the data token you use needs to replace the \<token> values shown:

Shell functions

function ecosuite-ssh () {

local node\_id="$1"

if \[ -z "$node\_id" ]; then

echo 'Must provide node ID , e.g. 123'

else

shift

echo "Enter SN token secret when first prompted for password. Enter node $node\_id password second."

ssh -o ServerAliveInterval=180 -o ServerAliveCountMax=2 -o NumberOfPasswordPrompts=1 -o StrictHostKeyChecking=no -o UserKnownHostsFile=/dev/null -o LogLevel=ERROR \\

-J "$node\_id"':**\<token>**@ssh.solarnetwork.net:9022' $@ solar@solarnode-$node\_id

fi

}

function ecosuite-sftp () {

local node\_id="$1"

if \[ -z "$node\_id" ]; then

echo 'Must provide node ID , e.g. 123'

else

shift

echo "Enter SN token secret when first prompted for password. Enter node $node\_id password second."

sftp -o ServerAliveInterval=180 -o ServerAliveCountMax=2 -o NumberOfPasswordPrompts=1 -o StrictHostKeyChecking=no -o UserKnownHostsFile=/dev/null -o LogLevel=ERROR \\

-o "ProxyJump=$node\_id"':**\<token>**@ssh.solarnetwork.net:9022' $@ solar@solarnode-$node\_id

fi

}

**Note from Matt:**

The StrictHostKeyChecking and UserKnownHostsFile ones stop SSH from verifying the server identity against a cached local copy… but I use that because I’m logging in/out of many nodes all the time and don’t mind skipping the verification. The LogLevel one removes the warning about connecting to an unverified host. The NumberOfPasswordPrompts one means it will stop after 1 failed password attempt, so the SolarSSH firewall doesn’t end up blocking my IP from too many bad password attempts.

Step 2: Logout of your terminal session and log back in

Step 3: Open a terminal session
