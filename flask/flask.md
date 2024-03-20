# 准备工作

1. 搭建基础环境遇到的第一个问题如下，需要修改Windows的PowerShell运行策略
``` powershell
PS D:\pycharmProject> py -3 -m venv venv # flask需要一套完整的模拟环境
PS D:\pycharmProject> venv\Scripts\activate # 运行完之后会激活该虚拟环境
venv\Scripts\activate : 无法加载文件 D:\pycharmProject\venv\Scripts\Activate.ps1，因为在此系统上禁止运行脚本。有关详细
信息，请参阅 https:/go.microsoft.com/fwlink/?LinkID=135170 中的 about_Execution_Policies。
所在位置 行:1 字符: 1
+ venv\Scripts\activate
+ ~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo          : SecurityError: (:) []，PSSecurityException
    + FullyQualifiedErrorId : UnauthorizedAccess
PS D:\pycharmProject> get-executionPolicy 
Restricted
PS D:\pycharmProject> set-executionPolicy remotesigned

执行策略更改
执行策略可帮助你防止执行不信任的脚本。更改执行策略可能会产生安全风险，如 https:/go.microsoft.com/fwlink/?LinkID=135170
中的 about_Execution_Policies 帮助主题所述。是否要更改执行策略?
[Y] 是(Y)  [A] 全是(A)  [N] 否(N)  [L] 全否(L)  [S] 暂停(S)  [?] 帮助 (默认值为“N”): Y
PS D:\pycharmProject>
```

2. 激活虚拟环境
``` powershell
PS D:\pycharmProject> venv\Scripts\activate
(venv) PS D:\pycharmProject>
```

3. 安装Flask
``` powershell
(venv) PS D:\pycharmProject> pip install Flask
Collecting Flask
  Obtaining dependency information for Flask from https://files.pythonhosted.org/packages/93/a6/aa98bfe0eb9b8b15d36cdfd03c8ca86a03968a87f27ce224fb4f766acb23/flask-3.0.2-py3-none-any.whl.metadata
  Downloading flask-3.0.2-py3-none-any.whl.metadata (3.6 kB)
Collecting Werkzeug>=3.0.0 (from Flask)
  Obtaining dependency information for Werkzeug>=3.0.0 from https://files.pythonhosted.org/packages/c3/fc/254c3e9b5feb89ff5b9076a23218dafbc99c96ac5941e900b71206e6313b/werkzeug-3.0.1-py3-none-any.whl.metadata
  Downloading werkzeug-3.0.1-py3-none-any.whl.metadata (4.1 kB)
Collecting Jinja2>=3.1.2 (from Flask)
  Obtaining dependency information for Jinja2>=3.1.2 from https://files.pythonhosted.org/packages/30/6d/6de6be2d02603ab56e72997708809e8a5b0fbfee080735109b40a3564843/Jinja2-3.1.3-py3-none-any.whl.metadata
  Downloading Jinja2-3.1.3-py3-none-any.whl.metadata (3.3 kB)
Collecting itsdangerous>=2.1.2 (from Flask)
  Obtaining dependency information for itsdangerous>=2.1.2 from https://files.pythonhosted.org/packages/68/5f/447e04e828f47465eeab35b5d408b7ebaaaee207f48b7136c5a7267a30ae/itsdangerous-2.1.2-py3-none-any.whl.metadata
  Downloading itsdangerous-2.1.2-py3-none-any.whl.metadata (2.9 kB)
Collecting click>=8.1.3 (from Flask)
  Obtaining dependency information for click>=8.1.3 from https://files.pythonhosted.org/packages/00/2e/d53fa4befbf2cfa713304affc7ca780ce4fc1fd8710527771b58311a3229/click-8.1.7-py3-none-any.whl.metadata
  Downloading click-8.1.7-py3-none-any.whl.metadata (3.0 kB)
Collecting blinker>=1.6.2 (from Flask)
  Obtaining dependency information for blinker>=1.6.2 from https://files.pythonhosted.org/packages/fa/2a/7f3714cbc6356a0efec525ce7a0613d581072ed6eb53eb7b9754f33db807/blinker-1.7.0-py3-none-any.whl.metadata
  Downloading blinker-1.7.0-py3-none-any.whl.metadata (1.9 kB)
Collecting colorama (from click>=8.1.3->Flask)
  Obtaining dependency information for colorama from https://files.pythonhosted.org/packages/d1/d6/3965ed04c63042e047cb6a3e6ed1a63a35087b6a609aa3a15ed8ac56c221/colorama-0.4.6-py2.py3-none-any.whl.metadata
  Downloading colorama-0.4.6-py2.py3-none-any.whl.metadata (17 kB)
Collecting MarkupSafe>=2.0 (from Jinja2>=3.1.2->Flask)
  Obtaining dependency information for MarkupSafe>=2.0 from https://files.pythonhosted.org/packages/b7/a2/c78a06a9ec6d04b3445a949615c4c7ed86a0b2eb68e44e7541b9d57067cc/MarkupSafe-2.1.5-cp311-cp311-win_amd64.whl.metadata
  Downloading MarkupSafe-2.1.5-cp311-cp311-win_amd64.whl.metadata (3.1 kB)
Downloading flask-3.0.2-py3-none-any.whl (101 kB)
   ---------------------------------------- 101.3/101.3 kB 25.8 kB/s eta 0:00:00
Downloading blinker-1.7.0-py3-none-any.whl (13 kB)
Downloading click-8.1.7-py3-none-any.whl (97 kB)
   ---------------------------------------- 97.9/97.9 kB 22.8 kB/s eta 0:00:00
Downloading itsdangerous-2.1.2-py3-none-any.whl (15 kB)
Downloading Jinja2-3.1.3-py3-none-any.whl (133 kB)
   ---------------------------------------- 133.2/133.2 kB 15.0 kB/s eta 0:00:00
Downloading werkzeug-3.0.1-py3-none-any.whl (226 kB)
   ---------------------------------------- 226.7/226.7 kB 12.4 kB/s eta 0:00:00
Downloading MarkupSafe-2.1.5-cp311-cp311-win_amd64.whl (17 kB)
Downloading colorama-0.4.6-py2.py3-none-any.whl (25 kB)
Installing collected packages: MarkupSafe, itsdangerous, colorama, blinker, Werkzeug, Jinja2, click, Flask
Successfully installed Flask-3.0.2 Jinja2-3.1.3 MarkupSafe-2.1.5 Werkzeug-3.0.1 blinker-1.7.0 click-8.1.7 colorama-0.4.6 itsdangerous-2.1.2

[notice] A new release of pip is available: 23.2.1 -> 24.0
[notice] To update, run: python.exe -m pip install --upgrade pip
(venv) PS D:\pycharmProject>
```

# 快速开始
1. 第一个Flask应用

``` python
from flask import Flask

app = Flask(__name__)

  

@app.route('/')

def hello_world():

    return 'Hello world!'
```

2. PowerShell命令行运行对应命令
``` powershell
(venv) PS D:\pycharmProject> python -m flask run
 * Serving Flask app 'hello.py'
 * Debug mode: off
WARNING: This is a development server. Do not use it in a production deployment. Use a production WSGI server instead.
 * Running on http://127.0.0.1:5000
Press CTRL+C to quit
127.0.0.1 - - [20/Mar/2024 17:03:39] "GET / HTTP/1.1" 200 -
127.0.0.1 - - [20/Mar/2024 17:03:40] "GET / HTTP/1.1" 200 -
127.0.0.1 - - [20/Mar/2024 17:03:43] "GET / HTTP/1.1" 200 -
```

3. 如下截图展示访问成功
![](Pasted%20image%2020240320173503.png)
