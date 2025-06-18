## ⚠️ BitsAndBytes CUDA Setup Nightmare (Resolved) 😮‍💨

### 🧵 What I Was Doing

I was trying to use `bitsandbytes` with Hugging Face Transformers to load a quantized LLM in 8-bit using my GPU (Tesla T4). My machine has **CUDA 12.4**, which seemed fine — until **everything broke**. 😂

---

### ❌ The Error

I ran this:

```python
from transformers import AutoModelForCausalLM, BitsAndBytesConfig
```

And got slapped with this:

```
CUDA Setup failed despite GPU being available.
libbitsandbytes_cuda124.so not found.
Defaulting to libbitsandbytes_cpu.so...
```

Then later:

```
ImportError: cannot import name 'pack_dict_to_tensor' from 'bitsandbytes.utils'
```

At this point, I realized this wasn’t a "just pip install it" kind of situation.

---

### 🔍 What Went Wrong

* The version of `bitsandbytes` from `pip install` didn’t support CUDA 12.4.
* It was missing the compiled binary: `libbitsandbytes_cuda124.so`
* Even after building from source, old leftover Python files caused more import errors.

---

### 🧼 What I Did to Fix It

#### 1. **Uninstalled existing `bitsandbytes`**

```bash
pip uninstall bitsandbytes -y
```

#### 2. **Deleted any leftover files**

```bash
rm -rf /usr/local/lib/python3.11/dist-packages/bitsandbytes*
```

> ⚠️ I had to delete the `dist-packages` manually because it still had broken `.py` files from the old version.

#### 3. **Cloned and built from source**

```bash
git clone https://github.com/TimDettmers/bitsandbytes.git
cd bitsandbytes
CUDA_VERSION=124 make cuda12x
python setup.py install
```

#### 4. **Confirmed it works**

```python
import bitsandbytes as bnb
print("BitsAndBytes is working!")  # finally 🙏
```

---

### 💡 What I Learned

* `bitsandbytes` doesn’t (yet) provide prebuilt binaries for CUDA 12.x, so **source build is mandatory**.
* After building from source, if Python is still failing imports — it’s probably reading old junk in `site-packages`.
* Always clean up fully before reinstalling a custom package.
* Just because `pip install` runs cleanly doesn’t mean it’ll actually work. Always test with an import.

---

### ✅ Next Time Checklist

* [ ] Check if CUDA version is supported by pip package
* [ ] If building from source, clean `site-packages` first
* [ ] Run `make cuda12x` before `setup.py install`
* [ ] Confirm with `import bitsandbytes as bnb` and test model load
