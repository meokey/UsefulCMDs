
## 开局第一行总是使用 set -euo pipefail
| 命令 | 含义 | 作用 |
|--|--|--|
| set -e | Exit immediately if a command exits with non-zero. | 防止错误被忽略 |
| set -u | Treat unset variables as an error. | 防止未定义变量导致隐蔽bug |
| set -o |  pipefailPipeline returns failure if any command fails. | 防止管道隐藏中间错误 |
```Bash
#!/bin/bash
set -euo pipefail
```

## 永远安全读取变量不要直接写$VAR， 而是优雅地写：
| 方式 | 含义 |
|--|--|
| ${VAR:-} | 如果未定义，返回空字符串 |
| ${VAR:=默认} | 如果未定义，赋值为默认值 |
| ${VAR:+替代} | 如果已定义，返回替代值 |
| ${VAR:?错误信息} | 如果未定义，抛出指定错误 |

## 使用绝对路径，避免 crontab 等环境问题
 Bash 中默认工作目录 (PWD) 不总是你预期的地方，尤其在 crontab 或系统任务中。
 标准做法是，确定并切换到脚本自身目录：
```Bash
SCRIPT_DIR="$(cd "$(dirname "${BASH_SOURCE[0]}")" && pwd)"
cd "$SCRIPT_DIR"
```

## 检查命令执行结果，而不是盲目假设成功
 例如，下载文件后不要想当然地认为成功了：
```Bash
wget http://example.com/file.zip

if [ ! -f file.zip ]; then
  echo "下载失败"
  exit 1
fi
```
 或者用更优雅的写法：
```Bash
wget -O file.zip http://example.com/file.zip || { echo "下载失败"; exit 1; }
```

## 保持脚本输出清晰简洁，尤其是日志信息
 好的脚本输出应该：打印正在做什么成功/失败都提示错误信息清晰比如：
```Bash
echo "📦 正在备份..."
tar -czf backup.tar.gz data/
echo "✅ 备份完成: backup.tar.gz"
```
 配合 Emoji 表情（如 📦 ✅ ❌ ⚠️），在自动化日志里也能清晰一眼看懂状态。


## 附录：最佳Bash模板#!/bin/bash
```Bash
set -euo pipefail

SCRIPT_DIR="$(cd "$(dirname "${BASH_SOURCE[0]}")" && pwd)"
cd "$SCRIPT_DIR"

# 加载.env
if [ -f ".env" ]; then
    export $(grep -v '^#' .env | xargs)
else
    echo "❌ .env文件不存在"
    exit 1
fi

# 示例逻辑
echo "🚀 开始任务..."

# Your logic here
```
