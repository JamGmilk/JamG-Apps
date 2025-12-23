
**适用于：**
- Obsidian 用户
- Android 设备，且具备 root 环境
- 使用 Git 同步笔记
## 1. 修改所有权

打开 Termux 终端，使用 `su` 切换到 root 用户。首先记录 Termux 和 Obsidian 的所有者名称，使用 ls 命令查询：

```bash
su
ls -ld /storage/emulated/0/Android/data/com.termux
ls -ld /storage/emulated/0/Android/data/md.obsidian
```

返回如下：

```bash
drwxrws---+ 3 u0_a356 ext_data_rw 3452 Jun 11  2025 /storage/emulated/0/Android/data/com.termux

drwxrws---+ 4 u0_a505 ext_data_rw 3452 Dec  9 04:34 /storage/emulated/0/Android/data/md.obsidian
```

注意到：

|          | Owner     | Group         |
| -------- | --------- | ------------- |
| Termux   | `u0_a356` | `ext_data_rw` |
| Obsidian | `u0_a505` | `ext_data_rw` |

修改所有者：

```bash
# 这里按实际填写
chown -R u0_a356:ext_data_rw /storage/emulated/0/Android/data/md.obsidian
```

---
## 2. 克隆仓库到 Obsidian 沙盒

退出 root，检查 Termux 能否访问 Obsidian 沙盒。

```bash
exit
cd /storage/emulated/0/Android/data/md.obsidian
ls -l
# 这里不会报错 "No such file or directory" 
```

克隆仓库喵~

```bash
cd /storage/emulated/0/Android/data/md.obsidian/files
git clone https://github.com/用户名/仓库名.git MyNotes
```

成功后改回所有权：

```bash
chown -R u0_a505:ext_data_rw /storage/emulated/0/Android/data/md.obsidian
```

---

当需要对仓库进行命令行时，可按上面方法修改沙盒的所有权，然后就可以正常使用终端喵。

```bash
git pull
git add -A
git commit -m "Update via Termux"
git push
```
