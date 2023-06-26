
Colaboratory与Jupyter共享魔术命令的概念。魔术命令是一种简写的注解，可以改变单元格文本的执行方式。要了解更多信息，请参阅Jupyter的魔术命令页面。
```python
%%html
<marquee style='width: 30%; color: blue;'><b>Whee!</b></marquee>
```
```python
%%html
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 450 400" width="200" height="200">
  <rect x="80" y="60" width="250" height="250" rx="20" style="fill:red; stroke:black; fill-opacity:0.7" />
  <rect x="180" y="110" width="250" height="250" rx="40" style="fill:blue; stroke:black; fill-opacity:0.5;" />
</svg>
```
<div id="output-area"><span id="output-header"> </span><div id="output-body"><div class="display_data"><div class="output_subarea output_html rendered_html"><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 450 400" width="200" height="200">
  <rect x="80" y="60" width="250" height="250" rx="20" style="fill:red; stroke:black; fill-opacity:0.7"></rect>
  <rect x="180" y="110" width="250" height="250" rx="40" style="fill:blue; stroke:black; fill-opacity:0.5;"></rect>
</svg></div></div></div><span id="output-footer"></span></div>

## 可用的行魔术命令（Line Magics）
```python
%alias: 定义命令别名。
%alias_magic: 创建魔术命令的别名。
%autoawait: 控制自动等待异步代码的行为。
%autocall: 控制函数调用时是否自动应用括号。
%automagic: 开启/关闭自动魔术功能。
%autosave: 控制Notebook的自动保存行为。
%bookmark: 为目录设置书签以便快速导航。
%cat: 显示文件的内容。
%cd: 更改当前工作目录。
%clear: 清除屏幕上的文本。
%colors: 控制控制台颜色设置。
%conda: 在conda环境中执行操作。
%config: 配置Jupyter的选项。
%connect_info: 显示与Notebook服务器的连接信息。
%cp: 复制文件或目录。
%debug: 启用交互式调试器。
%dhist: 显示历史目录列表。
%dirs: 显示当前目录堆栈列表。
%doctest_mode: 控制doctest模式的行为。
%ed: 启动外部编辑器编辑代码。
%edit: 编辑代码并将其重新执行。
%env: 设置环境变量。
%gui: 启用GUI事件循环。
%hist: 显示命令历史记录。
%history: 显示输入/输出历史记录。
%killbgscripts: 杀死后台运行的脚本。
%ldir: 列出本地目录的内容。
%less: 以分页方式显示文件内容。
%lf: 列出当前目录的文件。
%lk: 列出当前目录的文件和链接。
%ll: 列出当前目录的文件详细信息。
%load: 加载外部脚本到单元格中。
%load_ext: 加载Jupyter扩展。
%loadpy: 加载Python文件到单元格中。
%logoff: 关闭日志记录。
%logon: 打开日志记录。
%logstart: 开始记录会话日志。
%logstate: 显示当前的日志记录状态。
%logstop: 停止记录日志。
%ls: 列出当前目录的内容。
%lsmagic: 列出所有可用的魔术命令。
%lx: 列出当前目录的可执行文件。
%macro: 定义一个用于重复执行的宏。
%magic: 显示魔术命令的帮助信息。
%man: 显示命令的man页面。
%matplotlib: 配置Matplotlib绘图设置。
%mkdir: 创建新的目录。
%more: 以分页方式显示文本。
%mv:
```