# syncdir - an expect script to keep a remote directory updated with changes made in a local directory

## Setup

Make sure the expect package is installed and the expect command is in your PATH.

Ensure you have a directory:

```
$HOME/bin
```

in your PATH.

Run:

```
./Install.sh
```

The syncdir expect script can now be run.

## First use

On a terminal window on the local Linux system run:

```
cd /path/to/local/directory
syncdir --host remotehost --user username --pass password --rdir /path/to/remote/directory
```

Output should be similar to:

```
spawn sftp -P 22 username@remotehost
username@remotehost's password:
Connected to remotehost.
sftp> cd /path/to/remote/directory
sftp> pwd
Remote working directory: /path/to/remote/directory
sftp>
```

On a terminal window on the remote Linux system run:

```
cd /path/to/remote/directory
```

On a second terminal window on the local Linux system run:

```
date > testfile.txt
```

On the first terminal window on the local Linux system there should be output similar to:

```
sftp> put testfile.txt
Uploading testfile.txt to /var/tmp/syncdir.tmp/testfile.txt
testfile.txt                                                                                                           100%   29     3.0KB/s   00:00
sftp>
```

On the terminal window on the remote Linux system run:

```
cat testfile.txt
```

The content of this file on the remote Linux system should be the same
as file on the local Linux system.

Each time a file is created or modified on the local Linux system that
file will be copied using a sftp put command onto the remote Linux system.

## Stopping the syncdir expect script

Just type Control^C to stop the syncdir expect script. A more graceful
way to stop the script may be added at a future date such as looking
for a file called STOP with a file size of zero.

## Why not use rsync or similar?

Good question. If you need to keep remote files in sync with a local
source then the rsync command is an excellent option.

The syncdir expect script was written for a specific usage case of editing
(and possibly compiling) source code on one system and having the update
source and recompiled code ready for execution on a remote system within
seconds. This is handy when the remote system, for example, does not
have a compiler installed and installing the compiler on the remote
system is not possible (e.g. no root/sudo like access is available on
the remote system).


----------------
End of README.md
