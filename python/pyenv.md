#pyenv使用教程

###安装

**Mac**

- brew install pyenv
- brew install pyenv-virtualenv

配置

- echo 'eval "$(pyenv init -)"' >> ~/.bash_profile
- echo 'eval "$(pyenv virtualenv-init -)"' >> ~/.bash_profile

生效

- source ~/.base_profile

**Ubuntu**

- git clone git://github.com/yyuu/pyenv.git  ~/.pyenv

或

- curl -L https://raw.github.com/yyuu/pyenv-installer/master/bin/pyenv-installer | bash


配置

- echo 'export PYENV_ROOT="$HOME/.pyenv"' >> /etc/profile.d/pyenv.sh
- echo 'export PATH="$PYENV_ROOT/bin:$PATH"' >> /etc/profile.d/pyenv.sh
- echo 'eval "$(pyenv init -)"' >> /etc/profile.d/pyenv.sh
- echo 'eval "$(pyenv virtualenv-init -)"' >> /etc/profile.d/pyenv.sh

生效
- source /etc/profile.d/pyenv.sh

###常用命令
查看当前使用的python版本
> pyenv version

查看可供pyenv使用的python版本
> pyenv versions

查看可安装的python版本
> pyenv install --list

安装python版本
> pyenv install 3.5.2

设置全局python版本
> pyenv global \<python版本>

设置局部python版本
> pyenv local \<python版本>

创建虚拟环境 
> pyenv virtualenv \<python版本> <环境name>

列出当前虚拟环境 
> pyenv virtualenvs

进入虚拟环境
> pyenv activate \<环境name>

退出虚拟环境
> pyenv deactivate

删除虚拟环境
> pyenv uninstall \<环境name>






