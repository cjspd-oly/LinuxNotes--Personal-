---

## 🎧 Bluetooth Audio Profiles & Codecs – Notes for Headphone Use

---

### 1. 🔌 **Bluetooth Audio Profiles**

#### 🔹 **A2DP (Advanced Audio Distribution Profile)**

- **Purpose:** High-quality **stereo audio** streaming (music, videos, games).
- **Direction:** One-way (source → sink).
- **Used for:** Playback.
- **Headphone Role:** A2DP Sink (receives audio).
- **Supports codecs:** SBC, AAC, aptX, LDAC, SBC-XQ, etc.

#### 🔹 **HSP (Headset Profile) / HFP (Hands-Free Profile)**

- **Purpose:** **Two-way** low-bitrate audio – for **calls**, not music.
- **Direction:** Bidirectional.
- **Used for:** Voice communication.
- **Headphone Role:** HSP/HFP headset.
- **Supports codecs:** CVSD (basic), mSBC (wideband).

---

### 2. 🎵 **Codecs Explained**

#### 🔸 **SBC (Sub-band Codec)**

- Default codec under A2DP.
- Max bitrate: ~328 kbps.
- Good compatibility.
- **Acceptable** quality for casual listening.

#### 🔸 **SBC-XQ (eXtended Quality SBC)**

- Not an official codec, but a **tuned SBC profile** (enabled via PipeWire/PulseAudio tweaks).
- Bitrate: Up to **512 kbps** (comparable to AAC).
- Offers **noticeable improvement** in clarity, punch, and dynamic range.

#### 🔸 **CVSD (Continuously Variable Slope Delta modulation)**

- Used in **HSP/HFP**.
- Narrowband (8kHz).
- Mono, low quality.
- Optimized for **speech**, not music.

#### 🔸 **mSBC (Modified SBC)**

- Wideband voice codec (16kHz).
- Higher quality **calls**.
- Still **not suitable for music**.
- Preferred over CVSD when using HFP for voice.

---

### 3. ✅ **What Should You Use for Headphones?**

| Purpose            | Profile     | Codec       | Notes                                  |
|--------------------|-------------|-------------|----------------------------------------|
| 🎵 Music/Media     | A2DP Sink   | **SBC-XQ**  | Best quality via SBC on Linux.         |
| 🎧 Basic Music     | A2DP Sink   | SBC         | Compatible, lower quality.             |
| 📞 Voice Calls     | HSP/HFP     | mSBC        | Best option for voice (wideband).      |
| 🔊 Voice-only Apps | HSP/HFP     | CVSD        | Avoid unless mSBC not supported.       |

---

### 4. 🛠️ **Linux (PipeWire / PulseAudio) Configuration**

#### 👉 To enable SBC-XQ in PipeWire:

1. Edit `/etc/pipewire/media-session.d/bluez-monitor.conf`  
2. Find:
   ```ini
   properties = {
       bluez5.enable-sbc-xq = true
   }
   ```
3. Restart PipeWire:
   ```bash
   systemctl --user restart pipewire
   ```

#### 👉 For PulseAudio:

- Use patched version with `SBC-XQ` support or switch to PipeWire for better support.
- Check with:
   ```bash
   pactl list sinks short
   ```

---

### 5. 💡 Additional Insights

- 🔇 **A2DP cannot do microphone input.** Switching to HSP/HFP is **mandatory** if you need to use the headset mic.
- 🎧 **Automatic profile switching** is supported in most modern systems (e.g., using PulseAudio modules or PipeWire).
- 🔊 If sound quality drops during a call → **your system likely auto-switched to HSP/HFP.**
- 🧠 Proprietary codecs like **aptX/LDAC** are not always supported due to licensing, especially on Linux.

---

### 6. 🧪 Testing & Debugging

```bash
# Check current profile and codec (PulseAudio)
pactl list cards

# Set profile to A2DP
pactl set-card-profile bluez_card.<DEVICE_ID> a2dp-sink
```

---