# gild-root

A pre-init which mounts an overlayfs over a (potentially) read-only rootfs.

To use, 2 filesystems are needed:

	- "rom": an unchanging filesystem
	- "rw" : a filesystem to store edits to "rom" in

"rom" does not need to be writable at any time.

"rom" is always `/` at the time `gild-root` is executed.

Directories which must be present in "rom" (used in the process of overlaying):

	- `/proc`: will be used for a normal proc mount
	- `/mnt`: RW filesystem temporary location
	- `/tmp`: overlay filesystem temporary location

No direct managment of `/dev` is done. Device files referenced in arguments are
the responsibility of the user.

After running, filesystem layout is:

	- `/` : overlay filesystem
	- `/rom` : original root filesystem
	- `/overlay` : RW filesystem

# Build

```
	meson . _b
	ninja -C _b
```

# Configuration

On the kernel cmdline, the following options can be provided:

	- `gr.rw_device`: Required. The device used to mount the "rw" filesystem.
	- `gr.fs_type`: Required. The fstype of the "rw" filesystem.
	- `gr.fs_opts`: Optional, defaults to empty. Filesystem options for the "rw" filesystem.


# License

AGPL-3.0 or later
