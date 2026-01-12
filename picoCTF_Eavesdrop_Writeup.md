# 🕵️‍♂️ **CTF Challenge Write-up: Network Interception Challenge**
## 🛰️ **Challenge Overview**
- **Category:** `Network Forensics`
- **Challenge:** `Secure Chat Analysis`
- **Description:** _"Analyze this network capture to uncover hidden communication."_

---

## 🧑‍💻 **Initial Analysis**
Received: `network_capture.pcap` (chat between two parties)

---

## 🛠️ **Step-by-Step Solution**

### 1️⃣ Initial Reconnaissance
```sh
# Check basic file info
file network_capture.pcap

# Count packets and protocols
tshark -r network_capture.pcap -q -z io,phs
```

### 2️⃣ Conversation Analysis
```sh
strings network_capture.pcap | head -50
```
_Revealed a conversation about file decryption._

### 3️⃣ Identifying Key Information
- Decryption command with parameters
- Password in chat
- Port `9002` for file transfer
- File send/receive references

### 4️⃣ Locating the Encrypted Data
```sh
# Examine TCP stream 2 (port 9002)
tshark -r network_capture.pcap -z follow,tcp,hex,2
```
_Encrypted data with recognizable header found._

### 5️⃣ Extracting the Encrypted Payload
```sh
# Extract payload from packet 57
tshark -r network_capture.pcap -Y "frame.number == 57" -T fields -e tcp.payload
```

---

### 6️⃣ Decryption Process
```python
# Sample decryption (conceptual)
import subprocess

encrypted_hex = "[REDACTED_HEX_DATA]"
# ... decryption code ...
```

---

### 7️⃣ Obtaining the Flag
After extraction and decryption, the flag was revealed.

---

## 🏁 **Flag Format**
`picoCTF{...}`

---

## 🧰 **Key Techniques Demonstrated**
- Network Traffic Analysis (`tshark`)
- Protocol Stream Reconstruction
- Data Extraction
- Cryptographic Analysis
- Forensic Investigation

---

## 🛡️ **Tools Used**
- `tshark` / `wireshark`
- Command-line utilities
- Python scripts
- Crypto tools

---

## 📚 **Lessons Learned**
- Network comms can hide data
- Chat context gives clues
- Encrypted data has markers
- Multiple approaches help
- Protocol analysis is key

---

## 💡 **Recommendations**
1. Examine chat in captures first
2. Look for passwords/keys
3. Check non-standard ports
4. Use multiple tools
5. Document every step
---

