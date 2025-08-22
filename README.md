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
<img width="578" height="345" alt="image" src="https://github.com/user-attachments/assets/655077f3-679f-490d-8ab4-935e58344948" />

  
- ✅ Solution: Use **Nox Emulator** with **Android 12** using Nox Asst.  
<img width="980" height="592" alt="image" src="https://github.com/user-attachments/assets/5d62f16b-2df1-4867-8b8d-7615d3709979" />

---

### 2. Connect ADB (Windows) with Emulator
- Problem: you can't find the Nox emulator
<img width="1263" height="441" alt="image" src="https://github.com/user-attachments/assets/54a9f0f6-9bf2-4b62-9bc1-4c860f12b29e" />

- ✅ Solution:
  use the following commands to discover Nox ports
<img width="786" height="29" alt="image" src="https://github.com/user-attachments/assets/0bdaf4df-87be-4f1a-80dd-283c877532c4" />
<img width="951" height="185" alt="image" src="https://github.com/user-attachments/assets/97fcddb9-e992-4705-bf4b-4b58b8e009a7" />
<img width="947" height="52" alt="image" src="https://github.com/user-attachments/assets/4a391e4f-93b4-420a-8dcf-28ef2abc0ab5" />

- Try them all, till you catch port for your emulator
<img width="812" height="246" alt="image" src="https://github.com/user-attachments/assets/ae3611db-74fe-491f-9b5f-08be14a6593c" />

In this case to connect to the Emulator you can 
1. Disconnect from other devices then make adb connect 
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

Keep your VM network setting to NAT, and check if you can ping you windows from Kali VM
<img width="817" height="234" alt="image" src="https://github.com/user-attachments/assets/5c28f96f-1020-4f00-8206-39585562dd26" />

By default, Nox/Android Emulator listens only on 127.0.0.1:62026 (local only). We need to forward it to 0.0.0.0 so Kali can connect.
By default, Windows Firewall blocks incoming TCP connections from other devices on the network unless there’s an explicit allow rule.

<img width="1598" height="377" alt="image" src="https://github.com/user-attachments/assets/02006844-3a82-4567-9851-6eafe722b858" />

Now connect from the kali `adb connect <win-ip>:<nox-port>` 
<img width="852" height="434" alt="image" src="https://github.com/user-attachments/assets/7f035f4c-ad7a-461f-ab34-ea909b31d694" />


### 4. Connect Burp (Windows) with Emulator

1. Open Burp and get your Windows IP using:
```
ipconfig
```

<img width="958" height="390" alt="image" src="https://github.com/user-attachments/assets/8a42e8ff-abf0-4938-9b20-6050b1f5029a" />
<img width="1192" height="789" alt="image" src="https://github.com/user-attachments/assets/a925f0ff-e007-4e7e-abb0-dc530c0428a4" />

2. Connect emulator with adb.
3. Reverse port so Burp catches traffic:

<img width="1003" height="121" alt="image" src="https://github.com/user-attachments/assets/d7b47e8d-e276-4a7f-b83e-dd880d687b90" />

Now any connection to 127.0.0.1:8888 inside the emulator is tunneled to the same port on your Windows host, so burp will catch it


  
### 5. Frida & Objection Setup

Reinstall python and check add PATH to environment variable
Then use Frida version to use is 16.5.9 with It’s server 
<img width="409" height="37" alt="image" src="https://github.com/user-attachments/assets/fa416a84-de54-440d-81b8-caf67ea844dd" />
<img width="620" height="169" alt="image" src="https://github.com/user-attachments/assets/498058f2-c606-47bc-a05f-742b3a60ea9a" />
<img width="582" height="47" alt="image" src="https://github.com/user-attachments/assets/8de702f9-cf7e-4465-aef6-128597a55aef" />
Frida server download:
👉 (Frida 16.5.9 Release)[https://github.com/frida/frida/releases/tag/16.5.9] 

Then use Objection 1.11.0
<img width="553" height="60" alt="image" src="https://github.com/user-attachments/assets/7a657e2e-d47e-47df-828c-e85d38f56ff1" />

- IMPORTANT NOTE:
Installing Objection sometimes updates Frida Version, So make sure after installing it, to downgrade Frida Version

Install like this :
```
pip install frida-tools
pip install objection==1.11.0
pip install frida==16.5.9
```

### 6. Fix Proxy Bug

<img width="623" height="875" alt="image" src="https://github.com/user-attachments/assets/b662039d-a6f3-415d-bbfd-166ba34a5382" />

Workaround: Drag the network proxy slider to the left inside settings 😅
- There is a bug on the Nox network proxy , To edit the Proxy -> hold and drag to left xD



- Tested on: Windows 11 + Kali Linux + Nox Android 12.
- For educational and security research purposes only.




