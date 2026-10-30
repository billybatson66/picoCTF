# CanYouSee

### **📝 Task:** 
> This file seems broken... or is it? Maybe a couple of bytes could make all the difference. Can you figure out how to bring it back to life?


### **📁 File provided:**
* `file - an unknown file type


### **⌨️ Solution:**

First step always will be to check the file info:

```bash 
file file
```

<p align ="center">
    <img src= "../Corrupted file/assets/step1.png" />
</p>

It's an unknown file so we'll check the bytes of its header:

```bash
xxd file
```

<p align ="center">
    <img src= "../Corrupted file/assets/step2.png" />
</p>

We see that it's a JPEG image in JFIF format, but the hex signature of the header is broken, it's supposed to be `FF D8 FF E0` instead, so we'll open up vim to edit the broken hex signature:

```bash
vim -b file
:%!xxd 
```

Shift + R to enable replace mode, go to the header and replace the first 2 bytes with JPEG (JFIF) hex signature. After doing that we'll revert back and save the file.

<p align ="center">
    <img src= "../Corrupted file/assets/step3.png" />
</p>

```bash
:%!xxd -r
:wq 
```

Hit enter and close vim, after that the file should be a jpeg image:

```bash
eog file
```

<p align ="center">
    <img src= "../Corrupted file/assets/step4.png" />
</p>


We see the flag in the image, we'll simply type it out and put it in the flag.txt.

```bash
echo "academy{r3st0r1ng_th3_by73s_a6f66bd7} > flag.txt
```

<details>
  <summary>Click to reveal spoiler</summary>
  <br>
  <div align="center">

  ### `academy{r3st0r1ng_th3_by73s_a6f66bd7}`

  </div>
</details>

---

### **💡 Key takeaways:**

Linux: Operating systems like Linux ignore file extensions and determine a file's true format by reading its "magic number" (file signature) at the very beginning of its raw bytes. Always run the file command first to check how the system interprets the data.

If an image viewer fails to open a file or the terminal reads it as raw "data", the magic number might be altered. You can look up the correct hex signature for the intended format (e.g., FF D8 FF E0 for JPEG) and patch the file to restore it.

You do not need GUI-based tools to fix binary corruption. By converting a binary buffer to a hex dump in Vim (:%!xxd) and exclusively using Replace mode (R), you can safely overwrite exact bytes without breaking the strict alignment needed to reverse the dump back into a working file (:%!xxd -r).