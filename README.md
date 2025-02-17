# PhotonBBS
A simple chat server for Unix / Linux
Copyright (C) 2002-2022, Fewtarius

This program is free software; you can redistribute it and/or modify
it under the terms of the GNU General Public License as published by
the Free Software Foundation; either version 2 of the License, or
(at your option) any later version.

This program is distributed in the hope that it will be useful,
but WITHOUT ANY WARRANTY; without even the implied warranty of
MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
GNU General Public License for more details.

You should have received a copy of the GNU General Public License
along with this program; if not, write to the Free Software
Foundation, Inc., 59 Temple Place, Suite 330, Boston, MA  02111-1307  USA

The latest version of this software can be downloaded from:

https://github.com/fewtarius/photonbbs

## About

PhotonBBS is a simple UNIX / Linux multi-node chat server written in Perl
with support for BBS door games.  It was designed to be modular and provides
a built in set of functions that allow the BBS to be extended very simply.

### Obligatory Screenshot

![alt tag](https://imgur.com/8gGLgnC.png)

Users using PhotonBBS land in a primary "channel" upon logging in and can
begin immediately communicating with other users or they are free to create
channels of their own.  The following describes system commands available
to all users:

### User Functions

    /ACTION <ACTIONS>         Perform <action>                    ...  /A
    /WHISPER <WHO> <MESSAGE>  Send private message                ...  /W
    /BROADCAST <MESSAGE>      Send message to all users           ...  /R
    /INV                      Become Invisible                    ...   !
    /QUIT                     Log off the system                  ...   x

### System Commands

    /USERS                    Who is logged into the system       ...   #
    /SCAN                     Locate chat users                   ...  /S
    /CHANNELS                 Show available channels             ...  /C
    /ONELINERS                Write on the wall                   ...   %
    /BULLETINS                System Bulletins                    ...   @

### Room Commands

    /JOIN <ROOM>              Joins room <room>                   ...  /J
    /BAN <USER>               (UN)Bans <user> from room           ...  /B
    /HIDE                     (UN)Hides room from channel scan    ...  /H
    /OP <USER>                (UN)Assign a user as a ChanOp       ...  /O
    /SSL                      (UN)Enforce SSH on room             ...  /E
    /PRIVATE                  (UN)Sets a room PRIVATE             ...  /V
    /ALLOW <USER>             (UN)Allow <user(s)> in Private room ...  /L
    /TOPIC                    Set the channel topic               ...  /T
    /STATUS                   Show room status                    ...  /$

### User Settings

    /INFO                     Display your user settings
    /SET <COMMAND>            Set user preferences (SEE BELOW)    ...  /U
      Commands:
        NAME                    Set your full name
        PASSWORD                Reset your password
        EMAIL                   Change your email address
        LOCATION                Set your call location
        PHONE                   Set your phone number
        DOB                     Change your DOB
        ANSI                    Change your ANSI preference
        DEFAULT                 Set your default channel
        UNIXPWD                 Change your unix password if applicable

In addition to system commands, a set of @CODES are available to users
which add color to text, or provide system information.  A description of @CODES
appears below.

## Installation

### Installation

PhotonBBS can be deployed from Docker Hub with a single command.

    docker container run -dti --restart unless-stopped --net host --device=/dev/tty0 -v appdata:/appdata:rw -v /dev:/dev -v /lib/modules:/lib/modules -v /sys/fs/cgroup:/sys/fs/cgroup --privileged fewtarius/photonbbs

The BBS will be started and listening on port 23 within a minute or two.  

### Shared Storage

PhotonBBS is capable of multinode support across multiple hosts or containers.  If deploying PhotonBBS using NFS for shared storage, caching must be disabled on the NFS client using mount options lookupcache=none and noac.  If this is a shared mountpoint, these options could degrade performance of other applications.

### Admin Account

Once your BBS is configured, connect via telnet and create your sysop account.  After
logging in and successfully creating the account, use the user editor to grant
yourself administrative rights to the BBS.

    $ docker ps
    $ docker -it exec {CONTAINER ID} /bin/bash
    # /appdata/sbin/useredit.pl

Change the security level to 500 or higher.  Be sure to not add any additional whitespace to the file.

## Customizing the BBS

### Text Files

  * banned_ip - List of IP addresses not allowed on the system
  * ip_list - List of IP addresses allowed to log into a private system
  * welcome.txt - This is the screen presented to the user at the initial connection
  * bulletins.txt - This is a customized index screen for your bulletins
  * lastcalltop.txt, lastcallbot.txt - Last caller customized header and footer
  * oneltop.txt, onelbot.txt - Oneliners customized header and footer
  * login.txt - This screen is presented just after login.
  * telehelp.txt - This is the help file presented in teleconference when the user presses the ? key.
  * account.txt - This is the user information file presented in teleconference with the INFO command.

An informational message can be left in the main channel for users by editing the main channel message.

    $ vi /opt/photonbbs/data/messages/teleconf/MAIN/message

The BBS software will detect ANSI (.ans) and ASCII (.asc) files of the same name as any .txt used by the system, and use the ANSI or ASCII variant first if available.  ANSI, ASCII, and TEXT files as well as any message sent by users of the system may contain @CODES which are converted by the BBS.  A description of available @CODES is as follows:

### Action Colors

    @CLR    - Clear screen (For ANSI files only)  
    @RST    - Reset terminal color
    @BLK    - Black
    @RED    - Red
    @GRN    - Green
    @YLW    - Yellow
    @BLU    - Blue
    @MAG    - Magenta
    @WHT    - White (Light Grey)
    @CYN    - Cyan
    @BBK    - Bright Black (Grey)
    @BRD    - Bright Red
    @BGN    - Bright Green
    @BYL    - Bright Yellow
    @BBL    - Bright Blue
    @BMG    - Bright Magenta
    @BCN    - Bright Cyan
    @BWH    - Bright White

### System Variables

    @SYSTEMCLR   - System output color
    @USERCLR     - User metadata color
    @INPUTCLR    - Input color
    @ERRORCLR    - Error color
    @THEMECLR    - General theme color
    @PROMPTCLR   - Prompt color (:?)
    @DATACLR     - System generated data color
    @LINECLR     - Line color (Oneliner top/bottom)
    @SYSNM       - BBS name
    @SVRNM       - BBS software name
    @MENU        - Currently selected menu
    @NODE        - Your node number
    @USERS       - Number of users online now
    @CONNECT     - IP address you are connecting from
    @HOST        - BBS hostname
    @IP          - BBS IP Address
    @TIME        - The current time
    @DATE        - The current date
    @TOTALCALLS  - Total number of calls to the BBS
    @USER        - Your handle
    @RNAME       - Your real name
    @DOB         - Your date of birth
    @PHONE       - Your phone number
    @LOCAL       - User location
    @ID          - Users system ID index number
    @PRONOUN     - Your pronoun
    @EMAIL       - Your email address
    @DND         - Do not disturb flag
    @BANNED      - Account ban flag
    ~AT          - Provides @ Symbol

## System Bulletins

System Bulletins are a simple way to communicate news and information to
your users.  PhotonBBS ships with a bulletin editor (.BULLEDIT) to help
create and manage system bulletins.  Bulletins even support @CODES in the
title, and in the bulletin itself.

## BBS Door Support

PhotonBBS v1.5 and later provide support for BBS doors. The following drop file formats
are supported:

  * DOOR.SYS
  * DORINFO1.DEF
  * DORINFOx.DEF

Multiple example configuration files are provided to help you get started.  This
includes batch files and scripts for:

  * Tradewars 2002
  * Legend of the Red Dragon
  * Lunatix
  * Barren Realms Elite
  * Operation Overkill II
  * Simpsons
  * Darkness
  * Dopewars

To configure doors, simply create a shell script in PhotonBBS opt/photonbbs/sbin,
and add a line to PhotonBBS home/data/external.mnu. The format of the external.mnu
file is as follows:

    Menu Name|Channel|Description|Executable|Security Level|Hidden|Special|Maximum Users

  * Menu Name - This is what a user would type to execute the command
  * Channel - This is the channel that a user is changed to when the command executes
  * Description - This is what is shown to other users in the room when the command is executed
  * Executable - This is the command to execute
  * Security Level - This is the minimum security level to execute command
  * Hidden - Is this item hidden?
  * Special - Internal or External command? (Internal commands are subroutines, add-ons to photonbbs)
  * Maximum Users - Maximum number of users executing the command concurrently

Example:

    SYSLOG|HIDEOUT|heads to his favorite hideout|tailsys.sh|500|1|external|1

In addition to external BBS doors support, this feature also allows for external utilities to be
available to anyone with proper security.  PhotonBBS ships with a user editor, and configuration
for access to FreeDOS to troubleshoot and configure DOOR games.  The following examples are available
by default:

    .USEREDIT  - BBS User editor
    .BULLEDIT  - BBS Bulletin editor
    .DOS       - FreeDOS Shell
    .SHELL     - BASH Shell
