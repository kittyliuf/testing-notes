# Python 登录自动化测试入门

## 运行步骤

在当前目录打开终端，首次执行以下命令安装依赖和浏览器：

```powershell
python -m pip install -r requirements.txt
python -m playwright install chromium
```

运行 UI 自动化测试：

```powershell
python -m pytest -q
```

项目默认会让 UI 操作间隔约 2 秒。想看浏览器慢慢执行每一步，使用：

```powershell
python -m pytest test_shalu_ui.py --headed
```

还可以临时调得更慢，例如每步 3 秒：

```powershell
python -m pytest test_shalu_ui.py --headed --slowmo=3000
```

看到 `5 passed` 就表示基础登录测试和 UI 登录测试全部通过。

## 文件说明

- `login.py`：模拟被测的登录功能。
- `test_login.py`：自动化测试用例，文件名以 `test_` 开头，pytest 会自动发现它。
- `ui_login.html`：带输入框和登录按钮的练习页面。
- `test_login_ui.py`：使用 Playwright 点击和操作页面的 UI 自动化测试。
- `test_shalu_ui.py`：对沙鹿 Popeyes 对账中台执行登录页和登录后首页测试。

## 测试真实网站

只运行页面检查（不需要账号）：

```powershell
python -m pytest test_shalu_ui.py -q
```

运行真实登录和首页检查前，请使用有权限的测试账号设置环境变量。账号不会写入代码：

```powershell
$env:TEST_USERNAME = "你的测试账号"
$env:TEST_PASSWORD = "你的测试密码"
python -m pytest test_shalu_ui.py -q
```

登录测试会点击“立即登录”，成功后要求页面跳转到 `Views/Home`。没有设置账号时，该条测试会显示为跳过，不会误提交空账号。
- `requirements.txt`：测试依赖。

## 测试实践记录

项目中的测试思路和复盘记录见 [`docs/`](docs/README.md)，包括用例设计、接口与页面分层、对账场景，以及 AI 辅助测试时的边界。

## 测试结构

每条测试都遵循：准备数据 -> 调用功能 -> 断言结果。

例如：

```python
assert login("test_user", "123456") is True
```

这句话的意思是：输入正确账号密码后，登录结果必须是成功。
