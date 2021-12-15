How to fix: Centos warning: setlocale: LC_CTYPE: cannot change locale (UTF-8): No such file or directory

`vi /etc/environment` 

And add line
```
LANG=en_US.utf-8
LC_ALL=en_US.utf-8
``` 
Alternatively,

Create locale file manually: `localedef -i en_US -f UTF-8 en_US.UTF-8`
