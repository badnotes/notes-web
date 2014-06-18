python pip pypi pypy

这是几P呀，搞的真混乱...

pypi 是Python的官方软件包仓库，可以使用 easy_install 或者 pip 在本地下载仓库的安装包到本地安装，非常方便。

pip 是Python的包管理工具。like ruby gems,nodejs npm

pypy 是Python的一种实现。简单说就是pypy程序的解释器和通用的Python解释器不一样，引入了JIT机制。使得Python程序的运行效率显著提升。

python 3.4以后的版本自动pip,

add python/script to system path

download ez_setup.py to python/script

http://peak.telecommunity.com/dist/ez_setup.py


D:\>ez_setup.py
Setuptools version 0.6c11 or greater has been installed.
(Run "ez_setup.py -U setuptools" to reinstall or upgrade.)


D:\>ez_setup.py pip


D:\>pip -V
pip 1.5.6 from D:\tools\Python27\lib\site-packages\pip-1.5.6-py2.7.egg (python 2
.7)

https://raw.githubusercontent.com/pypa/pip/master/contrib/get-pip.py

python get-pip.py



admin access

用 pip 安装Python包：

D:\>pip install requests


http://stackoverflow.com/questions/4750806/how-to-install-pip-on-windows