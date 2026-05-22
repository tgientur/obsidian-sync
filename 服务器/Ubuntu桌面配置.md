# Ubuntu 桌面配置

> WSL2 通过 XFCE + 远程桌面连接

---

## 连接方式

1. `Win + R` → `mstsc` 打开远程桌面连接
2. 计算机栏输入 `172.29.160.1:3391`
3. 登录界面：
   - **Session**: `Xorg`（或 `Xvnc`）
   - **用户名**: WSL2 用户名
   - **密码**: WSL2 用户密码（`sudo` 密码）

## 启动桌面

```bash
sudo systemctl start graphical.target
```

---

172.21.15.75

1. 按 `Win + R`，输入 `mstsc` 打开远程桌面连接。
    
2. 在“计算机”栏输入：`172.29.160.1:3391`（注意端口是 3391，不要写错）。
    
3. 点击“连接”。
    
4. 登录界面：
    
    - **Session** 选择 `Xorg`（或者 `Xvnc`，一般 Xorg 即可）
        
    - **用户名**：你的 WSL2 用户名（例如 `yourname`）
        
    - **密码**：你的 WSL2 用户密码（就是 `sudo` 用的那个密码）
        
5. 点击 OK，稍等几秒，就能看到 XFCE 桌面了。

启动
 sudo systemctl start graphical.target