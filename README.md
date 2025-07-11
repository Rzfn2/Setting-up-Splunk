
# Splunk Enterprise Setup and Windows Event Log Integration

This guide explains how to install Splunk Enterprise on Ubuntu, configure it to receive Windows Event Logs, and set up a Windows Universal Forwarder.

---
## 1️⃣ Install Splunk Enterprise on Ubuntu
Run the following commands:
```bash
sudo apt update
sudo apt upgrade -y

wget -O splunk-9.4.3-237ebbd22314-linux-amd64.deb "https://download.splunk.com/products/splunk/releases/9.4.3/linux/splunk-9.4.3-237ebbd22314-linux-amd64.deb"

sudo dpkg -i splunk-9.4.3-237ebbd22314-linux-amd64.deb

sudo /opt/splunk/bin/splunk enable boot-start -user splunk

/opt/splunk/bin/splunk start --accept-license

sudo ufw allow 8000/tcp
sudo ufw reload
```

Access Splunk Web:
```
http://<YOUR_SERVER_IP>:8000
```

<img width="1918" height="1021" alt="image" src="https://github.com/user-attachments/assets/44a22f14-e0c5-4618-87d9-b561917887ee" />



---
## 2️⃣ Install the Windows Add-on in Splunk

- In the Splunk web interface, go to **Apps > Find More Apps**
- Search for `windows`
- Install **Splunk Add-on for Microsoft Windows**

<img width="1107" height="639" alt="image" src="https://github.com/user-attachments/assets/6faedb26-ed28-4c5d-bba4-b19d35209a1c" />


---

## 3️⃣ Create a New Index

- Navigate to **Settings > Indexes**
- Click **New Index**
- Enter your desired index name (e.g., `abdullah`)

---

## 4️⃣ Configure Receiving

- Go to **Settings > Forwarding and receiving**
- Select **Configure receiving**
- Click **New Port** and enter your port (default: `9997`)

<img width="1891" height="480" alt="image" src="https://github.com/user-attachments/assets/e8d7dd64-3a38-4f75-af08-a9fdff55ced4" />


---

## 5️⃣ Install the Splunk Universal Forwarder on Windows

Download and install the Splunk Universal Forwarder on each Windows machine you want to monitor.

During installation:

|Step|Action|
|---|---|
|Accept the license|Check the box|
|Install type|Universal Forwarder (on-premises)|
|Username and Password|Create as desired|
|Deployment Server|Leave empty|
|Receiver Server|Your Splunk Ubuntu server IP|
|Port|The port you configured (e.g., `9997`)|

---

## 6️⃣ Configure Windows Event Log Inputs

On the Windows machine:
1. Navigate to:
    ```
    C:\Program Files\SplunkUniversalForwarder\etc\system\default
    ```
2. Copy the file `inputs.conf`
3. Paste it in:
    ```
    C:\Program Files\SplunkUniversalForwarder\etc\system\local
    ```
4. Open `inputs.conf` with Notepad.
5. Append the following lines to collect Security logs to your Splunk index:
```
[WinEventLog://Security]
disabled = 0
index = abdullah

[WinEventLog://System]
disabled = 0
index = abdullah

[WinEventLog://Application]
disabled = 0
index = abdullah

```


---

## 7️⃣ Verify Data in Splunk

After completing the setup:
- Login to Splunk Web
- Go to **Search & Reporting**
- Run a search:
    ```
    index=Abdullah
    ```

You should see incoming Windows Security Event logs.

<img width="1914" height="904" alt="image" src="https://github.com/user-attachments/assets/dbd8282e-4463-4551-9f82-8df62c98135f" />


---

✅ **Repeat these steps for any additional Windows machines you want to monitor.**

---
## 👤 Author

Made by [Abdullah](https://github.com/Rzfn2)

Feel free to copy this Markdown into your GitHub repository. If you’d like, I can help you adapt or format further!
