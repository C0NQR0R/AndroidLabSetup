# Android Lab Build

This repository provides a step-by-step guide for building your own **Android Pentesting Lab** using **Nox Emulator**, **ADB**, **Burp Suite**, **Frida**, and **Objection**.  
It is based on practical challenges and solutions during setup.

---

## Challenges
1. Choosing a good emulator and Android version.  
2. Connecting **adb (Windows)** with the emulator.  
3. Connecting **adb (Kali VM)** with the emulator.  
4. Connecting **Burp Suite (Windows)** with the emulator.  
5. Installing **Objection** (Windows), **Frida (Windows)** and its server on the emulator.  
6. Fixing network proxy bug.  

---

## Solutions & Steps

### 1. Choosing Emulator & Android Version
- Problem: Not all emulators/versions work well with pentesting tools.  
- ✅ Solution: Use **Nox Emulator** with **Android 12**.  

---

### 2. Connect ADB (Windows) with Emulator
1. Find Nox port. Example ports: 
```
adb disconnect 127.0.0.1:57001
adb disconnect 127.0.0.1:64001
adb disconnect emulator-5554
```

2. Or directly specify the emulator:  
```
adb -s 127.0.0.1:62026 shell
```

### 3. Connect ADB (Kali) with Emulator

Keep your VM network setting to NAT.
By default, Nox listens only on `127.0.0.1:62026` → forward it to 0.0.0.0`
Make sure Windows Firewall allows incoming TCP connections.
Then connect from Kali:
```
 adb connect <windows-ip>:62026
```

### 4. Connect Burp (Windows) with Emulator

1. Open Burp and get your Windows IP using:
```
ipconfig
```
2. Connect emulator with adb.
3. Reverse port so Burp catches traffic:
```
adb reverse tcp:8888 tcp:8888
```
- Any connection to `127.0.0.1:8888` inside the emulator is tunneled to Burp on Windows.

  
### 5. Frida & Objection Setup
- Use Frida 16.5.9 and Objection 1.11.0 (compatible).

⚠️ Important: Installing Objection might update Frida version → downgrade it again.

Install like this:

```
pip install frida-tools
pip install objection==1.11.0
pip install frida==16.5.9
```
Frida server download:
👉 (Frida 16.5.9 Release)[https://github.com/frida/frida/releases/tag/16.5.9] 


### 6. Fix Proxy Bug

Nox sometimes resets proxy settings.

Workaround: Drag the network proxy slider to the left inside settings 😅
---
- Tested on: Windows 11 + Kali Linux + Nox Android 12.
- For educational and security research purposes only.

```



