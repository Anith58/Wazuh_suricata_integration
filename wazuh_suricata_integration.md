# Wazuh Integration with Suricata IDS 🛡️🐍

This guide walks through integrating **Suricata IDS** with **Wazuh** to forward intrusion detection alerts into the Wazuh SIEM platform.

---

## 1️⃣ Install Suricata on Your System
```bash
sudo apt update
sudo apt install suricata -y
```

---

## 2️⃣ Enable and Start Suricata
```bash
sudo systemctl enable suricata
sudo systemctl start suricata
sudo systemctl status suricata
```

---

## 3️⃣ Verify Suricata Logging
Suricata logs are usually located in:
```bash
/var/log/suricata/eve.json
/var/log/suricata/fast.log
```
You can check them with:
```bash
tail -f /var/log/suricata/eve.json
tail -f /var/log/suricata/fast.log
```

---

## 4️⃣ Edit Wazuh Agent Configuration
On the **Wazuh Agent** server (not the manager), open the configuration file:
```bash
sudo nano /var/ossec/etc/ossec.conf
```

Inside `<ossec_config>`, add the following blocks:

```xml
<localfile>
  <log_format>json</log_format>
  <location>/var/log/suricata/eve.json</location>
</localfile>

<localfile>
  <log_format>syslog</log_format>
  <location>/var/log/suricata/fast.log</location>
</localfile>
```

Save and exit.

---

## 5️⃣ Restart Wazuh Manager
```bash
sudo systemctl restart wazuh-manager
sudo systemctl status wazuh-manager
```

---

## 6️⃣ Verify Integration
Run the following command on the **Wazuh Manager** to confirm the logs are being read:
```bash
tail -f /var/ossec/logs/ossec.log | grep suricata
```

If Suricata generates alerts, they should now appear in the Wazuh dashboard.

---

## ✅ Summary
By following these steps, you have:
- Installed and configured Suricata IDS.
- Integrated Suricata logs into Wazuh.
- Enabled real-time IDS event monitoring inside Wazuh.

This setup allows Wazuh to **correlate Suricata's intrusion detection alerts** with other security events for better incident detection.

