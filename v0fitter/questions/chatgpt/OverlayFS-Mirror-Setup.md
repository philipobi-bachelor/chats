---
Source: https://chatgpt.com/c/682adcb8-6d48-8008-b9f0-8bc1b4e4fc4a
Model: o4-mini
Exported with: https://github.com/pionxzh/chatgpt-exporter
---

# OverlayFS Mirror Setup

#### You:
Linux setup directory to mirror another directory, such that the original directory is never modified but files are only truly copied if a process tries to write to it in the mirror directory

#### ChatGPT:
Below is a recipe using Linux’s built‑in OverlayFS to present a “mirror” of a source directory that remains read‑only, while writes are diverted into a separate copy‑on‑write layer. The original is never modified; only when a process writes to the overlay does that file get copied up into the upper layer.

## Summary

OverlayFS lets you stack a read‑only **lower** directory beneath a writable **upper** directory, merging them at a **mount point**. Reads go to the lower layer by default; the first time you write to a file, OverlayFS automatically **copies up** that file into the upper layer (copy‑on‑write), keeping the lower layer intact. A small **workdir** is also required by the kernel to coordinate the copy‑up operations. This approach is supported in modern Linux kernels (4.0+), is lightweight, and doesn’t require FUSE.

## 1. Prerequisites

1. **Kernel support** for OverlayFS (Linux ≥ 4.0).
2. Three empty directories on the **same filesystem** (upper and workdir must be on same FS):
   ```bash
   mkdir -p /path/to/original   # your existing source (lowerdir)
   mkdir -p /path/to/upper      # where modified/copied files will go
   mkdir -p /path/to/workdir    # internal kernel work area
   mkdir -p /path/to/merged     # mount point (your “mirror”)
   ```
   citeturn0search1turn0search2

## 2. Mounting the Overlay

Use the following command to create the merged view:

```bash
sudo mount -t overlay overlay \
  -o lowerdir=/path/to/original,upperdir=/path/to/upper,workdir=/path/to/workdir \
  /path/to/merged
```

- **lowerdir**: original, read‑only directory
- **upperdir**: writable layer for new or modified files
- **workdir**: required empty directory on same FS as upperdir
- **/path/to/merged**: where you’ll interact with the combined tree\
          citeturn0search1turn0search2

### What happens under the hood

- **Reads**: If a file exists in `upperdir`, it’s served from there; otherwise from `lowerdir`. citeturn0search7
- **Writes (first time)**: The kernel copies the file from `lowerdir` to `upperdir` (copy\_up), then applies your write to the new copy. Subsequent writes stay in `upperdir`. citeturn0search8
- **Metadata-only writes**: With the **metacopy** option (`-o metacopy=on`), only metadata is copied at first, and data is copied only when needed. citeturn0search4

## 3. Example Workflow

1. **Inspect merged tree**:
   ```bash
   ls /path/to/merged
   ```
2. **Read an existing file** (pulls from original if untouched):
   ```bash
   cat /path/to/merged/foo.txt
   ```
3. **Modify that file**:
   ```bash
   echo "mod" >> /path/to/merged/foo.txt
   ```
   This triggers a copy‑up: `/path/to/original/foo.txt` → `/path/to/upper/foo.txt`, then your append is applied to the upper copy. citeturn0search0turn0search7
4. **Create a new file**:
   ```bash
   echo "hello" > /path/to/merged/newfile.txt
   ```
   New files go directly into `upperdir`. citeturn0search7

## 4. Unmounting and Persistence

- **Unmount** when done:
  ```bash
  sudo umount /path/to/merged
  ```
- All changes reside in `upperdir` and survive across mounts; the original `lowerdir` remains unmodified. citeturn0search0

## 5. Advanced Options & Alternatives

### Multiple lower layers

You can stack several read-only sources:

```bash
sudo mount -t overlay overlay \
  -o lowerdir=/lower1:/lower2:/lower3,upperdir=/upper,workdir=/work \
  /merged
```

Entries from `/lower1` take precedence over `/lower2`, etc. citeturn0search1

### AUFS or UnionFS (FUSE‑based)

- **AUFS**: older unionfs in kernel patches, supports more features but not in mainline.
- **UnionFS-FUSE**: userspace alternative if kernel doesn’t support OverlayFS.

## 6. Cleanup

To revert completely, simply unmount and remove the extra directories:

```bash
sudo umount /path/to/merged
rm -rf /path/to/upper /path/to/workdir /path/to/merged
```

---

With this setup, your “mirror” directory at `/path/to/merged` behaves exactly like a writable copy of your original, but under the hood only touched files are truly copied, and your source stays pristine.


#### You:
is this also possible as non-root

#### ChatGPT:
Below is an overview of how you can achieve a copy‑on‑write “overlay” of a read‑only directory entirely as an unprivileged user. In short, you cannot use the in‑kernel OverlayFS mount syscall without CAP\_SYS\_ADMIN (i.e. root), but you can get the same behavior via FUSE‑based union filesystems—chiefly **fuse‑overlayfs** or **unionfs‑fuse**—or by leveraging unprivileged user namespaces on sufficiently new kernels.

## 1. Why you need FUSE (or user namespaces)

The standard `mount -t overlay` operation always requires CAP\_SYS\_ADMIN on the host. Unprivileged users cannot directly invoke it. However, the **FUSE** subsystem provides a kernel driver (`/dev/fuse`) that lets non‑root processes implement filesystems in user‑space—and FUSE itself enforces that unprivileged users can only mount filesystems under directories they own and for files they own citeturn0search1.

Alternatively, on kernels **≥ 5.11**, OverlayFS gained unprivileged mounting support *within* user namespaces—so if you can create a user namespace, you can mount an in‑kernel overlay without being root in the parent namespace citeturn0search2turn0search5.

## 2. Rootless Overlay via fuse‑overlayfs

[fuse‑overlayfs](https://github.com/containers/fuse-overlayfs) is a user‑space reimplementation of OverlayFS that works entirely as FUSE.

- **Non‑root by design**: works on almost all modern distributions without root citeturn0search0.

- **Usage** (after installing, e.g. `apt install fuse-overlayfs` or `dnf install fuse-overlayfs`):

  ```bash
  mkdir ~/lower ~/upper ~/work ~/merged
  fuse-overlayfs \
    -o lowerdir=$PWD/lower,upperdir=$PWD/upper,workdir=$PWD/work \
    $PWD/merged
  ```

  - `lowerdir`: your original, read‑only tree
  - `upperdir`: writes & copied‑up files
  - `workdir`: FUSE’s internal state area
  - `merged`: mountpoint under your home directory

- **Behavior**: first write to a file triggers a copy‑up into `upperdir`, then your write is applied—just like in‑kernel OverlayFS citeturn0search0.

- **Podman & rootless containers**: Podman historically used fuse‑overlayfs for its rootless storage driver and continues to do so on older kernels citeturn0search6turn0search15.

## 3. Alternative: unionfs‑fuse

[unionfs‑fuse](https://linux.die.net/man/8/unionfs-fuse) is another FUSE‑based union filesystem offering similar semantics:

```bash
mkdir ~/ro ~/rw ~/union
unionfs-fuse -o cow,default_permissions \
  ~/rw=RW:~/ro=RO ~/union
```

- `cow`: copy‑on‑write
- `default_permissions`: let FUSE check file permissions
- New or modified files go to `~/rw`, reads fall back to `~/ro` citeturn0search16.

## 4. Unprivileged In‑Kernel Overlay (via User Namespaces)

If your kernel ≥ 5.11 **and** your distro allows unprivileged user namespaces, you can do an in‑kernel overlay inside a new namespace:

```bash
unshare --mount --user --map-root-user bash
mount -t overlay overlay \
  -o lowerdir=./lower,upperdir=./upper,workdir=./work ./merged
```

- You become “root” *inside* the namespace and can mount without CAP\_SYS\_ADMIN in the parent citeturn0search2turn0search8.
- Some distros disable unprivileged user namespaces by default for security; check `/proc/sys/kernel/unprivileged_userns_clone`.

## 5. Prerequisites & Caveats

- **FUSE installed** and `fusermount` in your `$PATH`.
- **Permissions**: you must own the mountpoint (`~/merged`) and the underlying `upperdir`/`workdir` when using FUSE citeturn0search1.
- **Kernel support** for user namespaces (≥ 3.8; best ≥ 5.11 for overlay) citeturn0search8.
- **SELinux/AppArmor**: may block FUSE mounts—ensure your policies permit it or disable labels (e.g. `--security-opt label=disable` in Podman) citeturn0search6.

## 6. Summary

- **No**, you cannot use the in‑kernel `mount -t overlay` as a non‑root user *unless* you’re inside a privileged user namespace on kernel ≥ 5.11.
- **Yes**, you can get full copy‑on‑write semantics as non‑root via:
  1. **fuse‑overlayfs** (recommended, easy install)
  2. **unionfs‑fuse** (more generic)
  3. **unprivileged in‑kernel overlay** inside a user namespace (advanced)

Choose the approach that best fits your environment’s security policy and kernel capabilities.


#### You:
how to unmount / stop the fuse-overlayfs again?

#### ChatGPT:
Below is how you can cleanly stop and unmount a FUSE‑based overlay (such as one created with `fuse-overlayfs`):

## Summary

To unmount a FUSE overlay you simply issue the unmount command against your mount point. In most cases:

1. Use the FUSE helper:
   ```bash
   fusermount -u /path/to/merged
   ```
2. Or the generic unmount:
   ```bash
   umount /path/to/merged
   ```

Either will detach the overlay and allow you to remove the backing directories.

---

## 1. Using the FUSE helper (`fusermount`)

FUSE provides its own unmount utility, which does not require root privileges (so long as you own the mount point):

```bash
fusermount -u /path/to/merged
```

- `-u` stands for “unmount.”
- No `sudo` needed if you’re the owner of `/path/to/merged`. citeturn0search4

This is the *preferred* way for any FUSE filesystem—including `fuse-overlayfs`—because it talks directly to the FUSE kernel module.

---

## 2. Using the standard unmount (`umount`)

You can also use the traditional `umount` command, which *may* require root depending on your system’s policy:

```bash
sudo umount /path/to/merged
```

Or without sudo if your user has appropriate permissions:

```bash
umount /path/to/merged
```

This works for FUSE mounts just as it does for any other mounted filesystem. citeturn0search2

---

## 3. Troubleshooting “resource busy” errors

If you see:

```
umount: /path/to/merged: target is busy.
```

then:

1. **Identify processes** using the mount:
   ```bash
   lsof +D /path/to/merged
   ```
2. **Terminate or close** those processes/files:
   ```bash
   kill <PID>
   ```
3. **Retry** the unmount:
   ```bash
   fusermount -u /path/to/merged
   ```

---

## 4. Cleanup

Once unmounted, you can safely remove or repurpose your directories:

```bash
rm -rf /path/to/upper /path/to/work /path/to/merged
```

---

With these commands you can start and stop your rootless FUSE overlay environments at will, keeping your original data pristine and your workspaces tidy.


#### You:
I get the error: cannot mount overlay read-only

#### ChatGPT:
Below is a brief summary of why you’re seeing the `cannot mount overlay read-only` error and three ways to work around it as a non‑root user—or, if you must, as a privileged container—in Kubernetes or other container runtimes.

OverlayFS in‑kernel mounts always require **CAP\_SYS\_ADMIN** (i.e. root) in the parent namespace, so trying to `mount -t overlay …` inside an unprivileged pod fails with “read‑only” even if your lower layer is read‑only and your upper layer is tmpfs. The two main alternatives are:

1. **Run the container as privileged** (granting CAP\_SYS\_ADMIN),
2. **Use a FUSE‑based overlay** (e.g. `fuse‑overlayfs` or `unionfs‑fuse`) which works unprivileged,
3. **Use unprivileged in‑kernel mounts inside a user namespace** on kernels ≥ 5.11.

---

## Why the error occurs

When you mount an in‑kernel OverlayFS, the kernel insists on having administrative privileges to manage the upper and work dirs. Without CAP\_SYS\_ADMIN the syscall is effectively disallowed, and you get:

```
mount: /opt/overlay/target: cannot mount overlay read-only.
```

because the kernel treats the attempted mount request as read‑only (i.e. refuses to write metadata to its workdir) citeturn1view0.

---

## 1. Grant CAP\_SYS\_ADMIN via a privileged container

The simplest fix in Kubernetes is to allow the container to run in **privileged** mode, giving it the necessary capabilities to mount OverlayFS:

```yaml
spec:
  containers:
    - name: my-app
      image: my-image
      securityContext:
        privileged: true
      volumeMounts:
        - mountPath: /opt/overlay
          name: tempfs-volume
```

With `privileged: true`, the in‑kernel mount works as expected citeturn1view0. You can then unprivileged sidecars or the main app can share the merged view.

---

## 2. Use a FUSE‑based overlay (no root needed)

### fuse‑overlayfs

[fuse‑overlayfs](https://github.com/containers/fuse-overlayfs) is a user‑space reimplementation of OverlayFS that never requires root on modern distros (kernel ≥ 4.18) citeturn2search0. After installing it (`apt install fuse-overlayfs` or `dnf install fuse-overlayfs`), run:

```bash
mkdir ~/lower ~/upper ~/work ~/merged
fuse-overlayfs \
  -o lowerdir=$PWD/lower,upperdir=$PWD/upper,workdir=$PWD/work \
  $PWD/merged
```

This presents a merged view at `~/merged`, copying files into `upperdir` only on first write citeturn2search0.

### unionfs‑fuse

Alternatively, [unionfs‑fuse](https://manpages.ubuntu.com/manpages/trusty/man8/unionfs-fuse.8.html) provides a union mount with copy‑on‑write support:

```bash
mkdir ~/ro ~/rw ~/union
unionfs-fuse -o cow,default_permissions \
  ~/rw=RW:~/ro=RO ~/union
```

Writes go into `~/rw`, reads from `~/ro`, and everything “just works” without root citeturn4search0.

---

## 3. In‑kernel OverlayFS in an unprivileged user namespace

If your **kernel ≥ 5.11** and your distribution allows unprivileged user namespaces, you can become “root” inside a new namespace and mount natively:

```bash
unshare --mount --user --map-root-user bash
mount -t overlay overlay \
  -o lowerdir=./lower,upperdir=./upper,workdir=./work \
  ./merged
```

This works because support for overlay mounts in unprivileged user namespaces was added in kernel 5.11 citeturn3search0. (Check `/proc/sys/kernel/unprivileged_userns_clone` if it’s disabled.)

---

## Additional considerations

- **Performance**: FUSE-based overlays (`fuse-overlayfs`) incur extra user‑space overhead; native in‑kernel is faster once you can use it citeturn3search1.
- **SELinux/AppArmor**: Security policies may still block FUSE mounts—ensure `/dev/fuse` is permitted or disable labels for that mount.
- **Cleanup**: Unmount with `fusermount -u /merged` (no sudo) for FUSE, or `umount /merged` for in‑kernel; then `rm -rf upper work merged`.

---

### Summary

- **Error “cannot mount overlay read-only”** happens because in‑kernel OverlayFS needs CAP\_SYS\_ADMIN in the parent namespace.
- **Privileged containers** solve it but may be undesirable for security.
- **FUSE‑based solutions** (`fuse‑overlayfs`, `unionfs‑fuse`) work entirely unprivileged.
- **User‑namespace mounts** require kernel ≥ 5.11 and distro support, giving you native performance without root.
