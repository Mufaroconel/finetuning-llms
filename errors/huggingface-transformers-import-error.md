````markdown
# ⚠️ BitsAndBytes Import & Cleanup Error (Resolved)

## 🧵 What I Was Doing

After building `bitsandbytes` from source to fix CUDA compatibility, I tried importing it in Python:

```python
import bitsandbytes as bnb
````

But it failed with this error:

```
ImportError: cannot import name 'pack_dict_to_tensor' from 'bitsandbytes.utils'
```

## ❌ The Problem

* Python was still loading old leftover bitsandbytes files from the previous pip installation.
* These stale files caused import failures even after a successful build and install.
* The problem was due to the presence of old `bitsandbytes` directories inside the Python `site-packages`.

## 🔍 How I Diagnosed It

* Checked the installed packages and paths.
* Found multiple `bitsandbytes` folders in `/usr/local/lib/python3.11/dist-packages/`.
* The old files conflicted with the new source-built package.

## 🧼 How I Fixed It

1. Uninstalled bitsandbytes cleanly:

```bash
pip uninstall bitsandbytes -y
```

2. Manually deleted all leftover bitsandbytes directories:

```bash
rm -rf /usr/local/lib/python3.11/dist-packages/bitsandbytes*
```

3. Reinstalled the package from source (already done before).

4. Verified the import now works:

```python
import bitsandbytes as bnb
print("BitsAndBytes is working!")
```

## 💡 What I Learned

* After building from source, Python can still pick up old package files if not manually cleaned.
* `pip uninstall` may not always remove all files, especially for packages built from source.
* Manual cleanup of `site-packages` is sometimes necessary to avoid import conflicts.

## ✅ Next Time Checklist

* [ ] Always check for leftover package files in `site-packages` after uninstall.
* [ ] Manually delete old folders if import errors persist after reinstall.
* [ ] Test imports immediately after installation.

```
