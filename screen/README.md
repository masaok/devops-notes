- [Screen: start a new screen session](#screen-start-a-new-screen-session)
- [Start screen session with name](#start-screen-session-with-name)
- [Rename screen session](#rename-screen-session)
- [Reattach screen session](#reattach-screen-session)
- [Reattached an attached session after SSH crash](#reattached-an-attached-session-after-ssh-crash)
- [Show the current session name](#show-the-current-session-name)
- [References](#references)

### Screen: start a new screen session

```
screen: start a new screen session
CTRL-a, d: detach screen session
```

### Start screen session with name

```
screen -S <new_session_name>
```

### Rename screen session

```
screen -S <old_session_name> -X sessionname <new_session_name>
screen -S 8890.foo -X sessionname bar
```

https://superuser.com/a/521112

### Reattach screen session

```
screen -r <process from screen -ls>
screen -r 2317025.students-api
screen -r students-api
```

### Reattached an attached session after SSH crash

```
screen -d -r <process from screen -ls>
```

### Show the current session name

```
CTRL-a, : <type “sessionname”>
CTRL-a, n: next screen
CTRL-a, p: prev screen
```

https://stackoverflow.com/a/10025891/1302174

### References

Basics
https://uisapp2.iu.edu/confluence-prd/pages/viewpage.action?pageId=115540034

Command Reference
https://www.tecmint.com/screen-command-examples-to-manage-linux-terminals/

Tabs
https://unix.stackexchange.com/questions/26248/tabs-when-using-screen

Attach and Detach
https://www.interserver.net/tips/kb/using-screen-to-attach-and-detach-console-sessions/
