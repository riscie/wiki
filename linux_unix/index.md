![](./0.png)
# linux/unix

<!-- @import "[TOC]" {cmd="toc" depthFrom=1 depthTo=2 orderedList=false} -->

<!-- code_chunk_output -->

* [linux/unix](#linuxunix)
	* [ubuntu](#ubuntu)

<!-- /code_chunk_output -->


## ubuntu
### remove old kernels (to free up space in /boot)
```bash
dpkg -l 'linux-*' | sed '/^ii/!d;/'"$(uname -r | sed "s/\(.*\)-\([^0-9]\+\)/\1/")"'/d;s/^[^ ]* [^ ]* \([^ ]*\).*/\1/;/[0-9]/!d' | xargs sudo apt-get -y purge
``` 
