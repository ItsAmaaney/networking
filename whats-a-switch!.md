# Switching in Networking


## 📌 What is Switching?
A **switch** operates at **Layer 2 (Data Link Layer)** of the OSI model.  
switch uses **MAC addresses** to forward **frames**

📌 **Reminder:**  
At the **Data Link Layer (Layer 2)**,**frame** is a data unit
A frame contains:  
- Source MAC address  
- Destination MAC address  
- Data (payload)  

👉 switches use frames to forward traffic.  
(See [OSI Model](../OSI_Model.md) for a full breakdown of layers and data units.)

## Frames vs Packets

### Frame
- Used at **Layer 2 (Data Link Layer)** of the OSI Model
- Used by **Switches**
- Uses **MAC Addresses**
- Used for communication inside a **Local Area Network (LAN)**
- Example protocol: **Ethernet**

**Structure**
- Source MAC Address
- Destination MAC Address
- Data (Payload)

---

### Packet
- Used at **Layer 3 (Network Layer)** of the OSI Model
- Used by **Routers**
- Uses **IP Addresses**
- Used for communication **between different networks**
- Example protocol: **Internet Protocol (IP)**

**Structure**
- Source IP Address
- Destination IP Address
- Data (Payload)

---

### Key Differences

| Feature | Frame | Packet |
|-------|-------|-------|
| Layer | Data Link Layer (Layer 2) | Network Layer (Layer 3) |
| Address Type | MAC Address | IP Address |
| Device | Switch | Router |
| Scope | Local Network | Between Networks |

---

### 📡 When data is sent:

- 🔹 First → it becomes a **Packet** (uses **IP address**)
- 🔹 Then → it gets wrapped into a **Frame** (uses **MAC address**)

---

### 📦 Easy Analogy

- 📄 **Packet = item (data with IP)**
- 📦 **Frame = cover/box (used for delivery)**

## ⚡ How Switching Works
1. **Frame Creation**  
   - A device (e.g., PC1) sends data to PC2.  
   - The frame contains:  
     - **Source MAC** (PC1)  
     - **Destination MAC** (PC2)
       
       ![frame](./Images/PC1-to-PC2-Frame.png)
       
2. **Switch Checks MAC Table (CAM Table)**  
   - If the **destination MAC is known**, the frame is sent **only to that port**.  
   - If the **MAC is unknown**, the switch **floods** the frame to all ports except the incoming one.

     ![Flooding](./Images/Switch_Flooding_Frame.png) 

3. **Learning Process**  
   - The switch learns MAC addresses by recording the **source MAC** of each frame in its **Content Addressable Memory (CAM) table** along with the port.  

4. **ARP in Action**  
  ## 📡 ARP Request & Reply (Real Process)

### 🔹 Step 1: ARP Request

#### 🧠 What your device does

1️⃣ Your PC checks:

> "Do I already know the MAC for this IP?"

- 📌 It checks the **ARP cache (table)**
- ❌ If not found → it creates an **ARP Request**

---

### 🧾 ARP Request Structure (inside frame)

- 📡 **Destination MAC**: FF:FF:FF:FF:FF:FF (Broadcast)
- 🖥️ **Source MAC**: Your PC MAC  
- 🌐 **Sender IP**: 192.168.1.5  
- 🔑 **Sender MAC**: AA:AA:AA  
- 🎯 **Target IP**: 192.168.1.10  
- ❓ **Target MAC**: 00:00:00:00:00:00 (Unknown)

> ⚠️ This is NOT text — it is structured binary data

---

### 🔁 What the Switch Does

- 📥 Switch receives the frame  
- 👀 Sees **Broadcast MAC (FF:FF:FF:FF:FF:FF)**  
- 📤 Forwards it to **ALL ports except the sender**

---

## 📩 Step 2: ARP Reply

### 🧠 What happens

- 🎯 The device with IP **192.168.1.10** recognizes the request  
- 💬 It replies with its MAC address  

---

### 🧾 ARP Reply Structure

- 📡 **Destination MAC**: AA:AA:AA (Your PC)  
- 🖥️ **Source MAC**: BB:BB:BB (Target Device)  
- 🌐 **Sender IP**: 192.168.1.10  
- 🔑 **Sender MAC**: BB:BB:BB  
- 🎯 **Target IP**: 192.168.1.5  
- 📍 **Target MAC**: AA:AA:AA  

> 📌 This is **Unicast** (sent only to your PC)

---


---

## 🔑 Key Features of Switching
- **Flooding** → Sends frames to all ports when destination MAC is unknown.  
- **Learning** → Builds CAM table using source MACs.  
- **Forwarding** → Sends frames directly to the correct port (unicast).  
- **Broadcasting** → Used for ARP, DHCP requests, etc.    (ARP-addrss reso prtcl {to find mac addrs if ip is known}, 
  - (dhcp-dynmic host configrtn prtcl[**automatically assign IP addresses**])

---

## 🖼️ Example Flow
- PC1 wants to ping PC2.  
- PC1 sends ARP Request (`FFF.FFF.FFF.FFF`) → Switch floods it.
- PC2 replies with its MAC → Switch learns PC2’s MAC in CAM table.  
- Now PC1 ↔ PC2 communication goes directly via their ports (no flooding needed).  

---

✅ **Summary:**  
Switching = **Flood → Learn → Forward**  