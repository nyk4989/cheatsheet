## ## Reverse shellが完了した後
ptyを使用したshell
```sh
方法1
python -c "import pty;pty.spawn('/bin/bash')"

方法2
script /
```
https://hidepatidar.medium.com/spawning-interactive-reverse-shell-7732686ea775