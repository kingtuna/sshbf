THERE IS A VERSION 2 IN THE WORKS.  EXPECT INTERFACE CHANGES
============================================================

sshbf
=====

Simple SSH brute-forcer written in Go

Usage is fairly self-explanatory.  sshbf -h gives the available options.

sshbf is written and tested under OpenBSD, though it should probably
(cross-)compile and run on any posixy platform.  There may be EOF character
issues in Windows, but it may even run under Windows.

Typical Usage:
`sshbf -hfile=targetlist -ntask=64 -pfile=passdict -sfile=wins -sshvs=SSH-2.0-mycompany -ufile=unamelist`

This is very much a work in progress.  False negatives are the focus of development at the moment.  If you'd not mind being used for testing (i.e. run an SSH server with a really good password I can brute-force safely), please let me know.

Please file bug reports if something is output asking for a bug report to be filed.  Likewise, please file reports for unhandled errors.

Usage
-----

```
Usage of sshbf:
  -errdb=false: Print debugging information relevant to errors made during SSH attempts.
  -hfile="": File with a list of hosts to attack, one per line.  Either this or -host must be specified.  This list will be read into memory, so it should be kept short on low-memory systems.  CIDR notation may be used to specify multiple addresses at once.
  -host="": Host to attack.  This is, of course, required.  Examples: foo.bar.com, foo.baaz.com:2222.  Either this or -hfile must be specified.  CIDR notation may be used to specify multiple addresses at once.
  -ntask=4: Number of attempts (tasks) to run in parallel.  Don't set this too high.
  -onepw=false: Only find one username/password pair per host.
  -pass="": Single SSH password.  If neither this nor -pfile is specified, a small internal list of passwords will be attempted.  See -plist.
  -pause=0: Pause this long between attempts.  Really only makes sense if -ntask=1 is used.
  -pback=false: Attempt the username backwards as the password as well as other specified passwords.  E.g. root -> toor.
  -pdefl=false: Attempt the internal default password list.
  -pfile="": File with a list of passwords, one per line.  If neither this nor -pass is specified, a small internal list of passwords will be attempted.  See -plist.
  -plist=false: Print the internal default password list and exit.
  -pnull=false: Attempt the null (empty) password as well as other specified passwords.
  -puser=false: Attempt the username as the password as well as other specified passwords.
  -sfile="": Append successful authentications to this file, which whill be flock()'d to allow for multiple processes to write to it in parallel.
  -sshvs="SSH-2.0-sshbf_0.0.1": SSH version string to present to the server.
  -stdin=false: Ignore most of the above and read tab-separated username<tab>password<tab>host lines from stdin.  NOT CURRENTLY IMPLEMENTED.
  -tries=false: Print every guess.
  -ufile="": File with a list of usernames, one per line.  If neither this nor -user is specified, the single username root will be used.  This list will be read into memory, so it should be kept short on low-memory systems.
  -user="": Single SSH username.  If neither this nor -ufile is specified, root will be used.
  -wait=2m0s: Wait this long for authentication to succeed or fail.
```

Gotchas
-------
- The upper limit for `-wait` is set by the ssh library.
- The passwords generated by `-puser` and `-pback` are used for all users.
- Go's `flag` library is used, so the syntax is a bit goofy.
- Setting `-ntask` too high or `-wait` too low can cause false negatives.
- There's no real way at the moment to know how long it'll take to run.

Script Kiddies
--------------
Please don't use this software for illegal purposes. It's very easy to use
zmap/masscan to find hosts listening on 22 in say, China, and then this to see
just how many hosts respond to `oracle`/`oracle`.  Don't do it.  In the future,
I may put in something nasty to keep script kiddies at bay.
