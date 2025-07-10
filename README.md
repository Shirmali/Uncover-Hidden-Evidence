# Uncover-Hidden-Evidence

# 🕵️ Root-Me Forensics Challenge: Job Interview

## 🎯 Challenge Goal

Analyze a provided forensic image (`image_forensic.e01`) and uncover hidden evidence — specifically a flag — using forensic tools and techniques.

---

## 🧰 Tools Used

| Tool          | Purpose                               |
| ------------- | ------------------------------------- |
| `ewfexport`   | Convert `.E01` image to `.raw` format |
| `file`, `tar` | Identify and extract archive contents |
| `bmc-tools`   | Decode `.bmc` (RDP bitmap cache)      |
| `eog`         | View `.bmp` screenshots               |

---

## ✅ Step 1: Convert the Forensic Image (.E01) to RAW

The image was provided in **EnCase format** (`.E01`), commonly used in forensic investigations. To analyze it, we converted it to a raw format using `ewfexport`:

```bash
ewfexport image_forensic
```

> ⚠️ Do **not** include `.e01` in the filename when using `ewfexport`. It automatically detects the file.

**Prompts & Responses:**

* Export format: `raw`
* Target path and filename: `image`
* Segment size: Press Enter to accept default (0)

This created:

```bash
image.raw
```

---

## ✅ Step 2: Identify and Extract TAR Archive

To determine the file type:

```bash
file image.raw
```

**Output:**

```
image.raw: POSIX tar archive (GNU)
```

> 💡 Even though it had a `.raw` extension, the file was actually a `.tar` archive.

To extract the contents:

```bash
tar -xvf image.raw
```

This yielded a file:

```bash
bcache24.bmc
```

---

## ✅ Step 3: Analyze the `.bmc` File (RDP Bitmap Cache)

`.bmc` files are RDP bitmap caches. When someone connects via Remote Desktop Protocol (RDP), screenshots from the session are cached for performance — and these can contain sensitive visual data.

We used `bmc-tools` by ANSSI to extract those cached images:

### 🔧 Clone and Set Up:

```bash
git clone https://github.com/ANSSI-FR/bmc-tools.git
cd bmc-tools
mkdir ../bcache24bmc
```

### 🔍 Run the Tool:

```bash
./bmc-tools.py -s ../bcache24.bmc -d ../bcache24bmc/ -v
```

This extracted `.bmp` screenshots from the cache file into the `bcache24bmc/` directory.

---

## ✅ Step 4: View the Extracted Screenshots

Open the `.bmp` images using an image viewer:

```bash
eog ../bcache24bmc/*.bmp
```

You can also manually open the folder and browse the screenshots.

Three screenshots revealed the flag: 

- Yeah (RdP_)
- this is the (l3av3s_Tra)
- flag (c3s)

```
RdP_l3av3s_Trac3S
```

---

## 🏁 Final Flag

```bash
RdP_l3av3s_Trac3S
```

✅ Successfully submitted to Root-Me.

---

## 🔎 Forensic Insight

* RDP sessions store screenshot fragments in `.bmc` files
* Even if a session is closed, cached visual data can linger
* Always inspect file types using `file` — don’t trust extensions

---

## 📁 Summary

This challenge was a great demonstration of:

* Handling forensic disk image formats
* Working with archives hidden inside `.raw` files
* Investigating OS-level artifacts like RDP cache
* Using open-source tools to recover evidence

---

### 🚀 Follow for more forensic write-ups!

🛡️ Cybersecurity | Digital Forensics | CTFs
