# 代码质量静态检查

常用命令

```shell
metrics **   # or **/*

pycodestyle . -qq # --statistics 
yapf -ri .

pylint src
# pyflakes . #配置麻烦，所以我不用
bandit -c .bandit -r . # 如果.bandit是ini格式文件可以直接用`bandit -r .`
prospector  # or prospector .

pytest --cov=.
coverage report
```

# 命令简介

1. metrics: 代码复杂度（codacy.com 支持）
    + Files：不同格式的文件数量
    + SLOC: single line of code，统计代码行数
    + Comments：代码、注释行数
    + McCabe：代码复杂度

2. pycodestyle：检查代码格式是否符合pep8
3. yapf：修改代码格式。（类似于autopep8,black）
4. pylint：源代码静态检查（codacy.com 支持）
5. bandit：代码安全性检查（codacy.com 支持）
6. prospector：（codacy.com 支持）
    + profile-validator：检查prospector配置文件格式
    + pep8格式检查
    + mccabe检查
    + dodgy检查，检查不应再公开项目中存在的'dodgy'东西。比如
        - secret keys
        - passwords
        - AWS tokens
        - source control diffs.
    + pyflaks检查
    + pydocstyle：pep257 docstring规则检查
    + #pylint：由于会出错，所以禁止了
    + #pyroma：检查setup.py是否遵循python打包系统原则
7. pytest：代码测试。（Travis-CI 支持）
    + `pytest --cov=.`运行pytest并调用coverage插件，并显示corerage report结果
8. coverage report：测试代码覆盖率

> pyflakes：代码静态检查，速度比pylint快。但是如果要自定义配置，就需要使用flake8.


# 安装及配置说明

安装:

```
pip install metrics
pip install pycodestyle
pip install yapf
pip install pylint
# pip install pyflakes
pip install bandit
pip install prospector
pip install pytest
pip install pytest-qt
pip install coverage
```

配置

1. metrics
    + <https://pypi.org/project/metrics/>
    + 配置文件：metrics目前插件很少，因为我不用插件所以不需要配置
    + 结果文件：`.metrics`
2. pycodestyle
    + <https://pypi.org/project/pycodestyle/>
    + 帮助文件：<https://pycodestyle.readthedocs.io/en/latest/>
    + 配置文件：`setup.cfg`或`tox.ini`中的`[pycodestyle]`节
3. yapf
    + <https://pypi.org/project/yapf/>
    + 配置文件：`.style.yapf`中的`[style]`节，`setup.cfg`中的`[yapf]`节
        + .yapfignore文件中配置忽略文件列表
4. pylint
    + <https://pypi.org/project/pylint/>
    + 帮助文档：<http://pylint.pycqa.org/en/latest/>
    + 配置文件：`.pylintrc`。pylint2.5以后版本会支持setup.cfg配置
        + 目前pylint2.4.3 还不支持2.5的很多功能
5. bandit
    + <https://pypi.org/project/bandit/>
    + 帮助文档：<https://github.com/PyCQA/bandit>
    + 配置文件：`.bandit`中`[bandit]`节。ini格式
        + 由于在codacy上默认吧.bandit作为ymal格式配置文件，所以我个人把.bandit文件写作yaml格式，在自己电脑上用bandit -c .bandit -r .调用
6. prospector
    + <https://github.com/PyCQA/prospector>
    + 帮助文档：<https://prospector.readthedocs.io/en/latest/index.html>
    + 配置文件：`.prospector.yml`
    + 
    

# 本项目配置

忽略的文件

+ *metrics*: no cofing
    - None
+ pycodestyle: `setup.cfg`
    - `.git, *old.py, *_bak.py, dist, toml`
+ *yapf*: `setup.cfg`
    - None
+ pylint: `.pylintrc`
    - `.git, dist, build, bak, toml, splash.py`
    - `.*(_[vV][0-9.\-_]+[.]py$$|[.]old[.]py$$|_bak[.]py$|_rc[.]py$)`
    
    
> 貌似在codacy.com上，这些配置文件里的ignore列表都不起作用，只有.codacy.yml配置里的ignore列表才起作用。
