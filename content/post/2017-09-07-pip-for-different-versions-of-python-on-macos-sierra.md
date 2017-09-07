---
title: pip for Different Versions of Python on macOS Sierra
author: ~
date: '2017-09-07'
slug: pip-for-different-versions-of-python-on-macos-sierra
categories: ['Python']
tags: ['pip', 'macOS']
---

Three versions of Python exist on my macOS Sierra. The first one is the original Python come with macOS. The other two are Python 2 and Python 3 installed by `brew`. 

To load the raw Python 2 interpreter inside the terminal, type

```bash
$ python
Python 2.7.10 (default, Feb  7 2017, 00:08:15)
[GCC 4.2.1 Compatible Apple LLVM 8.0.0 (clang-800.0.34)] on darwin
Type "help", "copyright", "credits" or "license" for more information.
>>>
```

To load the brew-installed Python 2 interpreter inside the terminal, type

```bash
$ python2
Python 2.7.13 (default, Jul 20 2017, 21:05:06)
[GCC 4.2.1 Compatible Apple LLVM 8.1.0 (clang-802.0.42)] on darwin
Type "help", "copyright", "credits" or "license" for more information.
>>>
```

To load the brew-installed Python 3 interpreter inside the terminal, type

```bash
$ python3
Python 3.6.2 (default, Jul 18 2017, 13:03:54)
[GCC 4.2.1 Compatible Apple LLVM 8.1.0 (clang-802.0.42)] on darwin
Type "help", "copyright", "credits" or "license" for more information.
>>>
```

<br>
[pip](https://pypi.python.org/pypi/pip) is the PyPA recommended tool for installing Python packages. Therefore, to install pip for different versions of Python, we need to use the corresponding Python interpreter to run the [get-pip.py](https://bootstrap.pypa.io/get-pip.py).

For the raw Python 2

```bash
$ python get-pip.py
```

For the brew-installed Python 2

```bash
$ python2 get-pip.py
```

For the brew-installed Python 3

```bash
$ python3 get-pip.py
```

<br>
After successfully installed pip package, we can type following command to check the directory of `site-packages` where `pip` self is installed and later `pip` will install other packages to

For the raw Python 2
```bash
$ pip --version
pip 9.0.1 from /Library/Python/2.7/site-packages (python 2.7)
```

For the brew-installed Python 2
```bash
$ pip --version
pip 9.0.1 from /usr/local/lib/python2.7/site-packages (python 2.7)
```

For the brew-installed Python 3
```bash
$ pip --version
pip 9.0.1 from /usr/local/lib/python3.6/site-packages (python 3.6)
```

<br>
For the two versions of Python 2, I recommand to use the brew-installed Python 2 as often as possible because using `pip` to install packages for the raw Python 2 needs `sudo` permission. 