# Week 3 双语操作指南｜从 `week03_starter` 到 `week03_demo`
# Week 3 Bilingual Guide · From `week03_starter` to `week03_demo`

> **适用课程 / Course:** 3280 Backend Web Development — Week 3（PHP 文件读写 / PHP File I/O）
> **起点 / Start:** `../week03_starter/`　**终点 / Finish:** `../week03_demo/`
> **预计用时 / Time:** 60–90 分钟 / minutes
> **环境 / Environment:** PHP 8.0+（本指南所有输出均在 PHP 8.2.4 实测 / all outputs verified on PHP 8.2.4）
> **同内容网页版 / Same content as a webpage:** 打开同目录的 `index.html` / open `index.html` in this folder
> **互动练习 / Interactive practice:** 打开同目录的 `practice.html` —— 空白代码段网页填空 + 答案校对按钮 + 每题「查看原代码」+ 每部分 starter/demo 横向对比（暗夜 IT 主题，进度自动保存在浏览器）/ open `practice.html` for fill-in-the-blank exercises with answer checking, a per-question "view original code" button, side-by-side starter/demo comparison, and a dark IT theme (progress saved in your browser).

## 阅读符号 / Legend

| 标记 Mark | 含义 Meaning |
| --- | --- |
| **目标 / Goal** | 这一步要达到的效果 / what this step achieves |
| **代码 / Code** | 需要你输入或修改的代码 / code you must type or edit |
| **运行 / Run** | 执行方式 / how to execute |
| **预期 / Expected** | 正确的输出结果 / correct output |
| **检查点 / Checkpoint** | 做完后先自查再继续 / verify before moving on |
| 提示 / Tip　警告 / Warning　练习 / Exercise | 补充说明、易错点、加练 / notes, pitfalls, extra practice |

---

## 1. 这次任务要做什么 / What This Task Is About

**中文：** `week03_starter/` 是一个「半成品」：它只给你**工具箱**（`inc/file_utility.php` 里的 5 个函数）和**数据**（`data/` 里的 4 个文本文件），但**没有任何可以运行的页面**。你的任务是从零写出 4 个演示页面，并修好工具箱里的 1 行代码，最终得到 `week03_demo/`。

**English:** `week03_starter/` is a half-finished project. It gives you the **toolbox** (5 functions in `inc/file_utility.php`) and the **data** (`data/`), but there is **no runnable page at all**. Your job is to write 4 demo pages from scratch and fix 1 line in the toolbox, ending up with `week03_demo/`.

**本次要掌握的核心能力 / Core skills covered:**

1. 一次读入整个文件 / read a whole file at once — `file_get_contents()`
2. 读取前先判断文件是否存在 / check existence before reading — `file_exists()`
3. 写入与追加 / write and append — `file_put_contents()`, `FILE_APPEND`
4. 用文件句柄逐行读、逐行写 / line-by-line reading and writing with a file handle — `fopen()`, `fgets()`, `fgetcsv()`, `fwrite()`, `fclose()`
5. 用函数封装并复用文件操作 / wrap file operations in reusable functions — `require_once`, 自定义函数 / user-defined functions
6. 用 `try / catch / finally` 处理错误 / error handling — `try / catch / finally`

---

## 2. 两个文件夹的差别（任务拆解表）/ Diff of the Two Folders (Task Breakdown)

| 路径 Path | Starter | week03_demo | 差别与要做的事 / Difference & what to do |
| --- | --- | --- | --- |
| `data/data.txt` | 有 | 有 | 内容相同（60 字节）/ identical (60 bytes) — 不用动 |
| `data/orders.txt` | 有 | 有 | 内容相同 / identical — 不用动 |
| `data/orders.csv` | 有 | 有 | 内容相同 / identical — 不用动 |
| `data/users.txt` | 有（2 行 / 2 lines） | 有（3 行 / 3 lines） | 差异是**运行** `04` 造成的追加，不用手改 / the extra line is produced by *running* `04` |
| `data/newfile.txt` | **无** | 有（52 字节） | 由 `03_write_file.php` **运行时自动生成**，不要手写 / created at runtime by `03` |
| `inc/file_utility.php` | `fopen($fileName, "r")` | `fopen($fileName, "a")` | **修改 1 行**（第 57 行）/ edit 1 line (line 57) |
| `01_read_file.php` | **无** | 有 | **新写** / create new |
| `02_read_file_exist.php` | **无** | 有 | **新写** / create new |
| `03_write_file.php` | **无** | 有 | **新写** / create new |
| `04_file_read_write.php` | **无** | 有 | **新写** / create new |

**结论 / Conclusion：** 全部工作量 = **新建 4 个 PHP 文件（约 60 行代码）+ 修改 1 行 + 运行 4 次**。不需要数据库、不需要框架、不需要改配置 / Total work = **4 new PHP files (~60 lines) + 1 edited line + 4 runs**. No database, no framework, no config.

---

## 3. 任务清单 / Task Checklist

- [ ] **T1** 新建 `01_read_file.php`：一次读完整个文件并输出字节数 / read whole file + report its size
- [ ] **T2** 新建 `02_read_file_exist.php`：先判断文件是否存在再读取（含一个"错误路径"演示）/ guard reads with `file_exists()`
- [ ] **T3** 新建 `03_write_file.php`：用 heredoc 写文件，再追加内容并保留原有内容 / write, then append without erasing
- [ ] **T4** 修改 `inc/file_utility.php` 第 57 行：`"r"` → `"a"`，让 `fileWrite()` 真正能写入 / make `fileWrite()` actually writable
- [ ] **T5** 新建 `04_file_read_write.php`：`require_once` 引入工具箱，依次调用 `readArray()`、`readData()`、`readCSV()`、`fileWrite()` / glue everything together
- [ ] **T6** 运行验收：终端或浏览器逐个运行 4 个文件，对照第 9 节检查结果 / run all 4 files and compare with Section 9

---

## 4. 准备工作 / Setup

### 4.1 确认 PHP 可用 / Check PHP

```bash
php -v
```

**预期 / Expected:** 显示出 `PHP 8.x` 的版本信息（如 `PHP 8.2.4 (cli)`）。若提示 `command not found`，请安装 XAMPP / MAMP，或使用 XAMPP 自带路径，例如 / If not found, install XAMPP/MAMP or use the bundled binary:

```bash
/Applications/XAMPP/xamppfiles/bin/php -v
```

### 4.2 复制一份工作目录 / Copy the working folder

**中文：** 直接在 `week03_starter/` 里写也可以，但更推荐先复制成 `/tmp` 之外的一个新文件夹（例如 `my_week03/`），这样出错时可以随时回到干净的起点。
**English:** You may work directly inside `week03_starter/`, but it is safer to copy it to a new folder (e.g. `my_week03/`) so you can always restart from a clean state.

```bash
cp -R week03_starter my_week03
cd my_week03
```

### 4.3 两种运行方式 / Two ways to run

**方式 A：命令行（推荐，输出最清楚）/ Way A: CLI (recommended, clearest output)**

```bash
cd my_week03          # 必须在项目根目录运行 / must run from the project root
php 01_read_file.php
```

**方式 B：浏览器（XAMPP/Apache）/ Way B: browser (XAMPP/Apache)**

```bash
cp -R my_week03 /Applications/XAMPP/xamppfiles/htdocs/week03
# 启动 XAMPP 的 Apache，然后访问 / start Apache, then open:
# http://localhost/week03/01_read_file.php
```

> **提示 / Tip：** `01`～`04` 里大量使用 `\n`（换行符）。浏览器会把它当空格，所以网页上可能"挤成一行"。要看真实换行，请用 **查看网页源代码**（macOS `⌘ + U`，Windows `Ctrl + U`）或直接用命令行运行 / The scripts use `\n`, which a browser renders as a space. Use *View Source* or run them in the terminal to see the real line breaks.

> **警告 / Warning：** `04_file_read_write.php` 使用的是**相对路径**（`data/users.txt`），所以命令行运行时必须先 `cd` 到项目根目录；否则会看到 `The file does not exist` / `04` uses **relative paths**, so you must `cd` into the project root first.

---

## 5. 步骤一 · `01_read_file.php` —— 一次读完整个文件 / Step 1 · Read the Whole File

**目标 / Goal：** 用最短的代码把一个文本文件全部读出来并显示，同时报告文件大小 / Read an entire text file in one call and report its size.

### 代码 / Code

```php
<?php

$file = __DIR__ . '/data/data.txt';

$text = file_get_contents($file);

print "File content are \n" . $text;

$numBytes = filesize($file);

print "\nFilesize is " . $numBytes;
```

### 逐行说明 / Line-by-line

- `__DIR__` 是**当前 PHP 文件所在的绝对目录**，用它拼路径可以避免"换个目录运行就找不到文件"的问题 / `__DIR__` is the absolute directory of the current file; using it avoids "file not found when run from another folder".
- `__DIR__ . '/data/data.txt'` 拼出绝对路径（注意斜杠 `/` 要自己加）/ builds an absolute path (remember the leading slash).
- `file_get_contents($file)` **一次把整个文件读成一个字符串**，自动打开、自动关闭，不需要 `fopen()` / reads the whole file into one string; it opens and closes the file for you.
- `print "File content are \n" . $text;` 用 `.` 连接字符串；`\n` 是换行符（因为用了双引号才会被解释）/ `.` concatenates strings; `\n` is a newline (parsed because the string uses double quotes).
- `filesize($file)` 返回文件字节数 / returns the file size in bytes.
- 注意：这个文件**没有闭合标签 `?>`**——只含 PHP 的文件建议省略，可以避免多余空白输出 / Note: no closing `?>`; it is recommended to omit it in pure-PHP files to avoid stray output.

### 运行 / Run

```bash
php 01_read_file.php
```

### 预期输出 / Expected（PHP 8.2.4 实测 / verified）

```
File content are
Hello there
This is my secret code
Please do not open it

Filesize is 60
```

**检查点 / Checkpoint：** ① 三行文件内容都出现了；② 文件大小是 **60** 字节。如果只有一行或数字不对，先确认 `data.txt` 没有被改动过 / ① all three lines appear; ② size is **60**. Otherwise check that `data.txt` was not modified.

> **练习 / Exercise：** 把 `filesize()` 换成 `strlen($text)` 对比结果；再把 `filesize()` 换成 `count(file($file))`，猜猜得到什么（提示：行数）/ Compare `strlen($text)` with `filesize()`, then try `count(file($file))` (hint: number of lines).

---

## 6. 步骤二 · `02_read_file_exist.php` —— 存在才读 / Step 2 · Read Only If It Exists

**目标 / Goal：** 读文件前先用 `file_exists()` 判断，避免直接报 Warning；同时**故意演示一个错误路径**，理解绝对路径与相对路径的区别 / Guard reads with `file_exists()` and deliberately demo a wrong path to understand the difference between absolute and relative paths.

### 代码 / Code

```php
<?php

$file1 = __DIR__ . '/data/data.txt';
$file2 = __DIR__ . 'data.txt';

if(file_exists($file1)){
    print 'File 1 exist';
    print file_get_contents($file1);
}

if(file_exists($file2)){
    print 'File 2 exist';
    print file_get_contents($file2);
}
```

### 逐行说明 / Line-by-line

- `$file1` 是**正确路径**：`项目目录/data/data.txt` / a **correct** path.
- `$file2 = __DIR__ . 'data.txt'` 是**故意写错的**：少了 `/data/` 这个子目录，实际变成 `项目目录data.txt`（不存在的文件）/ intentionally wrong: the `/data/` sub-folder is missing, so it points to a non-existent `项目目录data.txt`.
- `file_exists($path)` 返回 `true` / `false`，常用于读取、写入、删除前的安全检查 / returns a boolean; conventionally used as a safety check.
- `if` 语句只有在文件存在时才执行 `file_get_contents()`，因此**不会**出现"Failed to open stream"警告 / the read only happens when the file exists, so no warning is raised.
- 两个 `print` 之间没有加 `\n`，所以 `File 1 exist` 会和文件内容连在一起 / no `\n` between the two prints, which is why the output runs together.

### 运行 / Run

```bash
php 02_read_file_exist.php
```

### 预期输出 / Expected（实测 / verified）

```
File 1 existHello there
This is my secret code
Please do not open it
```

**中文：** 只输出了 File 1 —— 因为 File 2 的路径不存在，`file_exists()` 返回 `false`，第二次读取被"拦住了"。这就是写文件操作前先判断的价值。
**English:** Only File 1 appears. File 2's path does not exist, so `file_exists()` returns `false` and the second read is skipped. That is the value of checking first.

### 修正练习 / Fix it exercise

把 `$file2` 改成正确路径，再运行一次 / fix `$file2` and run again:

```php
$file2 = __DIR__ . '/data/data.txt';
```

**预期 / Expected（实测 / verified）：** 会看到 `File 1 exist...` 和 `File 2 exist...` 两段内容 / both blocks are printed.

**检查点 / Checkpoint：** 你能解释"为什么 File 2 没有输出"以及"改哪里就能输出" / You can explain why File 2 is skipped and how to fix it.

---

## 7. 步骤三 · `03_write_file.php` —— 写入与追加 / Step 3 · Write and Append

**目标 / Goal：** 用 heredoc 语法准备多行字符串，用 `file_put_contents()` 先**覆盖写**、再**追加写** / Prepare multi-line strings with heredoc, then write (overwrite) and append with `file_put_contents()`.

### 代码 / Code

```php
<?php

$file = __DIR__ . '/data/newfile.txt';

$content1 = <<< CONTENT
the cat
sat
on the mat

CONTENT;

file_put_contents($file,$content1);

$content2 = <<< CONTENT
the rat
spat
on the cat!
CONTENT;

file_put_contents($file,$content2,FILE_APPEND);
```

### 逐行说明 / Line-by-line

- `<<< CONTENT ... CONTENT;` 是 **heredoc**：用来写多行字符串，里面的换行会原样保留 / heredoc keeps line breaks exactly as typed.
  - `<<<` 后面是自定义的标识符（这里是 `CONTENT`）/ the identifier after `<<<` is your choice.
  - **结束标识符必须单独占一行**（PHP 7.3+ 允许缩进，缩进会从字符串中被移除）/ the closing identifier must be on its own line (PHP 7.3+ allows indentation, which is stripped from the string).
  - 结束标识符后面的 `;` 是语句结束符，不能漏 / the `;` after the closing identifier is the statement terminator.
  - **紧挨结束标识符的那一个换行不计入字符串**。`$content1` 在 `CONTENT;` 前多留了一个**空行**，所以它以换行结尾；`$content2` 没有空行，所以它**不以换行结尾** / the newline immediately before the closing identifier is **not** part of the string: `$content1` has an extra blank line so it ends with a newline, while `$content2` does not.
- `file_put_contents($file, $content1)` 默认是**覆盖**：文件原有内容先被清空（文件不存在时会自动创建）/ by default it **overwrites**; it also creates the file if missing.
- 第三次参数 `FILE_APPEND` 表示**追加到末尾，不覆盖**——这是本节课最重要的一个标志位 / `FILE_APPEND` appends instead of overwriting — the key flag of this lesson.
- 这个脚本**没有任何输出**，它的成果在文件里 / the script prints nothing; its result is in the file.
- `file_put_contents()` 的返回值是**写入的字节数** / its return value is the number of bytes written.

### 运行 / Run

```bash
php 03_write_file.php
```

### 预期结果 / Expected

**中文：** 命令行没有输出，这是正常的。请打开 `data/newfile.txt`（它是自动生成的），内容应为 6 行：
**English:** No terminal output — that is expected. Open the generated `data/newfile.txt`; it should contain 6 lines:

```text
the cat
sat
on the mat
the rat
spat
on the cat!
```

**中文：** 实测字节数为 **52**，等于 `$content1`（26 字节）+ `$content2`（26 字节）。注意"`the mat` 后面有换行、最后一行 `on the cat!` 后面没有换行"，这正是上面 heredoc 空行规则造成的效果。
**English:** The verified size is **52 bytes** = `$content1` (26) + `$content2` (26). Note that `the mat` is followed by a newline while the final line `on the cat!` is not — exactly the heredoc blank-line rule explained above.

验证字节数（可选）/ check the size (optional):

```bash
php -r "echo filesize('data/newfile.txt');"      # 52
```

**检查点 / Checkpoint：** ① 文件被自动创建；② 第二次写入**没有**覆盖第一次的内容（共 6 行）；③ 如果去掉 `FILE_APPEND`，重跑后文件只剩 3 行——能解释原因 / ① the file is auto-created; ② the second write did not erase the first (6 lines); ③ removing `FILE_APPEND` leaves only 3 lines — and you can explain why.

> **练习 / Exercise：** 试着从终端追加一行（理解同样的机制）/ Append a line from the terminal:
> ```bash
> php -r "file_put_contents('data/newfile.txt', \"a big fat rat\n\", FILE_APPEND);"
> ```

---

## 8. 步骤四 · 修改 `inc/file_utility.php` —— 让 `fileWrite()` 真正能写 / Step 4 · Make `fileWrite()` Actually Write

**目标 / Goal：** 找到并修改 **1 行**，让写入函数生效。这是整个任务中唯一的"改错"环节 / Locate and fix **one line** so the write function works. This is the only bug-fixing step.

### 定位 / Locate

打开 `inc/file_utility.php`，找到 `fileWrite()` 函数（约第 55 行起），里面有这一行 / open `inc/file_utility.php`, find `fileWrite()` (from about line 55) and look at this line:

```php
$fh = fopen($fileName, "r"); // change to ab next
```

**修改为 / Change it to:**

```php
$fh = fopen($fileName, "a"); // change to ab next
```

### 为什么要改 / Why

| 模式 Mode | 含义 Meaning | 能读 Read | 能写 Write | 文件不存在时 If file missing | 指针位置 Pointer |
| --- | --- | --- | --- | --- | --- |
| `r` | 只读 / read only | ✔ | ✘ | 失败 / fails | 开头 / start |
| `r+` | 读写 / read+write | ✔ | ✔ | 失败 / fails | 开头 / start |
| `w` | 只写，**清空** / write, truncates | ✘ | ✔ | 自动创建 / creates | 开头 / start |
| `w+` | 读写，**清空** / read+write, truncates | ✔ | ✔ | 自动创建 / creates | 开头 / start |
| `a` | **追加** / append | ✘ | ✔ | 自动创建 / creates | 末尾 / end |
| `a+` | 读 + 追加 / read+append | ✔ | ✔ | 自动创建 / creates | 末尾 / end |
| `x` / `x+` | 独占创建（已存在则失败）/ exclusive create | ✘ / ✔ | ✔ | 自动创建，已存在则失败 / creates only if absent | 开头 / start |

- 在 `"r"`（只读）句柄上调用 `fwrite()` 会失败，PHP 提示 / writing to a read-only handle fails with:
  `PHP Notice: fwrite(): Write of 18 bytes failed with errno=9 Bad file descriptor`
- 因为 `fwrite()` 返回 `false`，`if (! fwrite(...))` 成立，于是你的代码**主动抛出异常** / `fwrite()` returns `false`, so the `if` is true and your own exception is thrown:

  ```text
  Error (File: .../inc/file_utility.php, line 60): Could not open the file for writing!
  ```

- 源码里那句注释 `// why the exception was raised? bcause the file was opened for reading!` 已经告诉你答案 / the in-code comment already gives the answer.
- 改成 `"a"`（追加）后，`fwrite()` 成功，原有内容不会被覆盖——这正好符合 `04` 里"给 users.txt 增加一条记录"的需求 / with `"a"`, `fwrite()` succeeds and existing data is preserved, which is exactly what `04` needs.

> **提示 / Tip：** `"ab"` 中的 `b` 表示二进制模式（`b` = binary）。在 Windows 上处理换行更安全；在 macOS/Linux 上 `"a"` 与 `"ab"` 效果相同 / the `b` in `"ab"` = binary mode; it matters on Windows and is equivalent on macOS/Linux.

### 改前 / 改后对比 / Before vs After（实测 / verified）

**改前 / Before（`"r"`）：** `04` 运行后 `users.txt` **没有任何变化**，并输出上面的 Notice 与 Error 行。
**改后 / After（`"a"`）：** `users.txt` 末尾新增一行 `john	john@doe.com`（Tab 分隔），文件从 **44 字节** 变成 **62 字节**。

**检查点 / Checkpoint：** `grep` 一下确认改对了 / confirm with:

```bash
grep -n 'fopen($fileName' inc/file_utility.php
```

**预期 / Expected：** 只出现一次 `a`，例如 / one `a` only:

```text
57:        $fh = fopen($fileName, "a"); // change to ab next
```

---

## 9. 步骤五 · `04_file_read_write.php` —— 把工具箱拼装起来 / Step 5 · Glue It All Together

**目标 / Goal：** 引入工具箱，把 4 个函数按"读数组 → 逐行读 → 读 CSV → 追加写入"的顺序调用一次 / Include the toolbox and call all four functions in order.

### 代码 / Code

```php
<?php

require_once 'inc/file_utility.php';

// read the file as an array
$file = 'data/users.txt';
readArray($file);

// read using file_exist() and fgets()
$file = 'data/orders.txt';
readData($file);

// read using file_exist() and fgetcsv()
$file = 'data/orders.csv';
readCSV($file);

// write a new entry to users.txt
$file = 'data/users.txt';
$data = "\njohn\tjohn@doe.com";

fileWrite($file,$data);
```

### 逐行说明 / Line-by-line

- `require_once 'inc/file_utility.php';` 引入工具箱。`require_once` 保证**只引入一次**，重复引入不会导致"函数重复定义"的致命错误 / `require_once` includes the file exactly once, so repeated includes cannot cause a "cannot redeclare function" fatal error.
- 这里的路径是**相对路径**（相对于"运行时的工作目录 CWD"），所以必须在项目根目录执行 / these are **relative** paths (relative to the current working directory), so run from the project root.
- 四次调用分别对应工具箱里的 4 个函数 / the four calls map to the four toolbox functions:

| 调用 Call | 函数 Function | 内部机制 Internals | 输出 Output |
| --- | --- | --- | --- |
| `readArray('data/users.txt')` | `readArray()` | `file()` 一次读成**数组**（每个元素一行）→ `foreach` → `explode("\t")` → `list($name,$email)` → `trim()` 去掉行尾换行 / `file()` returns an array of lines, then `explode`, `list`, `trim` | 每行一个 `mailto:` 链接 / one `mailto:` link per line |
| `readData('data/orders.txt')` | `readData()` → `openFile()` | `file_exists()` 判断 → `fopen($fileName,'r')` → `while(!feof($fh)) echo fgets($fh);` → `fclose()` / existence check, then `fopen`/`fgets`/`feof`/`fclose` | 原样文本 / raw text |
| `readCSV('data/orders.csv')` | `readCSV()` | `fgetcsv($fh, 1024, ',')` 把每行按逗号切成数组 → 双层循环输出 HTML 表格 / `fgetcsv` splits each row by comma, then nested loops build an HTML table | `<table>` + `<tr>/<td>` |
| `fileWrite('data/users.txt', $data)` | `fileWrite()` | 第 57 行 `fopen(..., "a")` → `fwrite()` → `try/catch/finally` → `fclose()` | **无输出**，但文件被追加一行 / no output; the file gains a line |

- `$data = "\njohn\tjohn@doe.com";` 是以**换行开头**的（因为 `users.txt` 最后一行没有换行符，先补一个换行才不会接在同一行后面）；`\t` 是 Tab，因为 `readArray()` 用 `explode("\t")` 切分，**必须用 Tab 而不是空格** / the string starts with `\n` to start a new line, and uses `\t` because `readArray()` splits on tab — **a space will not work**.

### 运行 / Run

```bash
cd my_week03          # 项目根目录 / project root
php 04_file_read_write.php
```

### 预期输出 / Expected（实测 / verified，已修改 `"a"` 且 `users.txt` 用 Tab 分隔）

```text
<a href="mailto:joe@douglas.com">Joe</a> <br>
<a href="mailto:douglas@joe.com">Douglas</a> <br>
Order Type, Customer ID, Amount
regular, C123A, 4
regular, C223B, 1
special, C112A, 1<table>
<tr>
	<td>Order Type</td>
	<td>Name</td>
	<td>Amount</td>
</tr>
<tr>
	<td>special</td>
	<td>Jason Gilmore</td>
	<td>4</td>
</tr>
...（共 5 行数据，5 data rows）
```

**中文：** 顺序是：2 个邮件链接（`users.txt`）→ `orders.txt` 原文 → `orders.csv` 的 HTML 表格。用浏览器打开时表格会真正渲染成表格（命令行里只能看到源码）。
**English:** Order of output: the 2 mail links (`users.txt`) → raw `orders.txt` → the HTML table from `orders.csv`. Open it in a browser to see the table actually rendered.

**检查点 / Checkpoint：**

1. `users.txt` 末尾出现了新的一行 `john	john@doe.com` / the new line `john	john@doe.com` appears at the end of `users.txt`.
2. 页面上没有 `Warning` / `Notice` / `Fatal error` / no PHP diagnostics appear.
3. 再运行一次，`users.txt` 会**再追加**一行 —— 说明写入是"追加"而不是"覆盖"（这一点和 `03` 的幂等行为不同）/ running it again appends a second line: appending is not idempotent, unlike `03`.

> **警告 / Warning：** 本仓库 `week03_demo/data/users.txt` 的**第 1 行用的是空格而不是 Tab**（`Joe joe@douglas.com`），所以运行时会出现
> `PHP Warning: Undefined array key 1 ... on line 9`，并且输出的链接是 `<a href="mailto:">Joe joe@douglas.com</a>`。
> **解决办法 / Fix：** 把第 1 行的空格改回 **Tab**（或在编辑器里对齐成两列）即可，这也是第 11 节排查表里的第 6 条 / replace the space with a tab.

---

## 10. 验收清单 / Acceptance Checklist

| # | 检查项 Check | 期望结果 Expected |
| --- | --- | --- |
| 1 | `php 01_read_file.php` | 输出 3 行文本 + `Filesize is 60` |
| 2 | `php 02_read_file_exist.php` | 只输出 `File 1 exist...`（File 2 被跳过） |
| 3 | `php 03_write_file.php` 后查看 `data/newfile.txt` | 6 行、52 字节，且**已存在**的文件（可重复运行结果不变） |
| 4 | `inc/file_utility.php` 第 57 行 | 模式是 `"a"` |
| 5 | `php 04_file_read_write.php` | 2 个链接 + `orders.txt` 原文 + 5 行表格数据，无报错 |
| 6 | `data/users.txt` | 比初始多一行 `john	john@doe.com`（44 → 62 字节） |
| 7 | 浏览器访问 `http://localhost/week03/04_file_read_write.php` | 邮箱是可点击链接，CSV 渲染成表格 |

**中文：** 7 项全部通过，说明你已经完成了从 `week03_starter` 到 `week03_demo` 的全部编程工作。
**English:** All 7 passing means you have reproduced every change from `week03_starter` to `week03_demo`.

---

## 11. 常见错误与排查 / Troubleshooting

| 现象 Symptom | 原因 Cause | 解决 Fix |
| --- | --- | --- |
| `The file does not exist` | 路径写错，或命令行工作目录不对（`04` 用相对路径）/ wrong path or wrong working directory | `cd` 到项目根目录；路径写成 `data/xxx.txt` 或 `__DIR__ . '/data/xxx.txt'` |
| `PHP Warning: file_get_contents(...): Failed to open stream: No such file or directory` + `stat failed` | 直接读了不存在的文件（没做 `file_exists()` 判断）/ reading a missing file without a guard | 先用 `file_exists()` 判断，再读（见步骤二） |
| `Notice: fwrite(): Write of 18 bytes failed with errno=9 Bad file descriptor` | `fopen(..., "r")` 只读句柄上调用 `fwrite()` / writing to a read-only handle | 把模式改成 `"a"`（见步骤四） |
| `Error (File: .../file_utility.php, line 60): Could not open the file for writing!` | 上面那条失败触发了 `fileWrite()` **里自己写的** `throw`，是设计好的行为 / your own exception fired | 先修 `"r"` → `"a"`；这条信息本身不是 PHP 内部错误 |
| `PHP Fatal error: Uncaught TypeError: fgetcsv(): Argument #1 ($stream) must be of type resource, bool given ... :45` | 文件不存在时 `openFile()` 返回 `false`，而 `readCSV()` **没有检查**就把它传给了 `fgetcsv()` / `readCSV()` ignores the `false` returned by `openFile()` | 在 `readCSV()` 里加保护：`$fh = openFile($fileName,'r'); if(!$fh) return;` |
| `PHP Warning: Undefined array key 1 ... on line 9`，链接变成 `mailto:` | `users.txt` 某行用**空格**代替了 **Tab**，`explode("\t")` 分不出两列 / a space was used instead of a tab | 把该行改成 Tab 分隔（本仓库 `week03_demo/data/users.txt` 第 1 行就是这种情况） |
| 网页上所有内容挤成一行 / 什么都没有 | 脚本用 `\n`，浏览器不认；`02`/`03` 本来就几乎没有输出 / browser ignores `\n`; `02`/`03` produce little output on purpose | 用「查看网页源代码」，或在命令行运行 |
| `failed to open stream: Permission denied` | 文件权限不足、被同步软件锁定（如 OneDrive）、或 XAMPP 用户无写权限 / permissions or file locks | macOS/Linux：`chmod 664 data/*.txt`；Windows：确认文件未设为"只读"，不使用时可暂停云同步 |
| 中文/乱码或多余的空白输出 | 文件编码不是 UTF-8，或 `?>` 之后有空白 / encoding or whitespace after `?>` | 保存为 UTF-8 无 BOM；纯 PHP 文件省略结尾 `?>` |
| 若你的 PHP 版本提示 `fgetcsv()` 相关 deprecated（8.3+/8.4+） | 新版本对 `$length`、`$escape` 参数有废弃提醒 / newer versions warn about those parameters | 改写成 `fgetcsv($fh, null, ',')`（明确传参，语义不变）/ pass the parameters explicitly |

---

## 12. 关键函数速查表 / Quick Reference

| 函数 Function | 作用 Purpose | 返回值 Returns | 用在本项目 Where |
| --- | --- | --- | --- |
| `file_get_contents($f)` | 一次读入整个文件 / read a whole file | `string` 或 `false` | `01`、`02` |
| `file_put_contents($f,$s[,FILE_APPEND])` | 写 / 追加 / write or append | 写入字节数或 `false` / bytes written or `false` | `03` |
| `filesize($f)` | 文件字节数 / file size in bytes | `int` 或 `false` | `01` |
| `file_exists($f)` | 文件或目录是否存在 / does it exist | `bool` | `02`、`openFile()` |
| `file($f)` | 读成数组（每元素一行）/ read into an array of lines | `array` 或 `false` | `readArray()` |
| `fopen($f,$mode)` | 打开文件，取得句柄 / open and get a handle | `resource` 或 `false` | `openFile()`、`fileWrite()` |
| `fgets($fh)` | 读一行 / read one line | `string` 或 `false` | `readData()` |
| `fgetcsv($fh,$len,$sep)` | 读一行并按分隔符切分 / read a row and split it | `array` 或 `false` | `readCSV()` |
| `fwrite($fh,$s)` | 通过句柄写 / write through a handle | 写入字节数或 `false` | `fileWrite()` |
| `feof($fh)` | 是否到达文件末尾 / end of file reached? | `bool` | `readData()` |
| `fclose($fh)` | 关闭句柄（释放资源）/ close and release | `bool` | `readData()`、`readCSV()`、`fileWrite()` |
| `trim($s)` / `explode($sep,$s)` / `list($a,$b)` | 去空白 / 分割 / 解构 / trim, split, destructure | — | `readArray()`、`readCSV()` |

---

## 13. 加练题目 / Extension Exercises

1. **数行数 / Count lines.** 写 `05_count_lines.php`，输出 `data/orders.txt` 的总行数（提示：`file()` + `count()`）/ output the number of lines.
2. **安全的表格 / Safer table.** 给 `readCSV()` 加 `if(!$fh) return;`，并补上漏掉的 `</table>` 结束标签，再用 `htmlspecialchars()` 处理单元格内容，防止 HTML 注入 / guard the handle, close the table tag, escape cell content.
3. **更好的 CSV 表格 / Better CSV table.** 第一行改成表头 `<th>`，其余行用 `<td>`（提示：加一个计数器变量）/ render the first row as a header.
4. **创建新文件的函数 / A create-new-file function.** 给 `inc/file_utility.php` 追加 `writeNewFile($fileName, $str)`：文件不存在时用 `"w"` 模式创建并写入，返回写入字节数；文件已存在时用 `file_exists()` 阻止覆盖并提示 / implement exclusive creation.
5. **加锁写入 / Locked append.** 用 `file_put_contents($f, $s, FILE_APPEND | LOCK_EX)` 追加一行，并解释多用户环境下 `LOCK_EX` 的作用 / explain why locking matters.
6. **把输出变成数据 / Return, don't echo.** 复制 `readCSV()` 为 `getCSV($fileName)`，把 `echo` 改成 `$html .= ...`，最后 `return $html;`，体会"函数返回数据、页面负责显示"的分离设计 / refactor echo-based code into a function that returns a string.
7. **错误处理练习 / Error handling.** 故意把 `04` 里的 `data/users.txt` 改成 `data/users2.txt`，观察输出的报错链路（哪一行触发、谁抛异常），再把它改回来 / break it on purpose and trace the error path.

---

## 14. 术语表（中英对照）/ Glossary

| 中文 Chinese | English | 说明 Note |
| --- | --- | --- |
| 文件句柄 | file handle | `fopen()` 返回的资源，后续读写都靠它 / resource returned by `fopen()` |
| 文件指针 | file pointer | 当前读/写位置；`"a"` 模式打开时在末尾 / current read/write position |
| 追加模式 | append mode | `"a"`，写在末尾且不清空 / writes at the end without truncating |
| 覆盖写 | overwrite | `"w"` 或默认的 `file_put_contents()` / `"w"` or the default of `file_put_contents()` |
| 只读模式 | read-only mode | `"r"`，能读不能写 / can read, cannot write |
| 二进制模式 | binary mode | 模式字符串里的 `b` / the `b` in the mode string |
| 定界符（heredoc） | heredoc / nowdoc | 多行字符串语法 / multi-line string syntax |
| 解析 CSV | parse CSV | `fgetcsv()` 按分隔符切成数组 / split a row by a delimiter |
| 制表符 | tab (`\t`) | `users.txt` 的列分隔符 / column separator in `users.txt` |
| 换行符 | newline (`\n`) | 与 `\r\n`（Windows CRLF）区别 / vs. CRLF |
| 文件末尾 | end of file (EOF) | `feof()` 判断 / tested by `feof()` |
| 异常 | exception | `throw` / `try` / `catch` / `finally` |
| 相对路径 | relative path | 相对当前工作目录，如 `data/users.txt` / relative to the CWD |
| 绝对路径 | absolute path | 如 `__DIR__ . '/data/data.txt'` |
| 幂等 | idempotent | 重复执行结果不变；`03` 幂等，`04` 追加**不**幂等 / same result when repeated |

---

### 文件对照 / Where to look

- 我写的成品代码 / finished code: `../week03_demo/`（`01`–`04` + `inc/file_utility.php`）
- 全新起点 / clean starting point: `../week03_starter/`
- 本指南网页版 / web version of this guide: 同目录 `index.html`

**祝调试顺利！/ Happy debugging!**
