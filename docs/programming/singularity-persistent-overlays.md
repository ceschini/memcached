# Singularity's Persistent Overlays

This is a requirement for applications that must have write privilleges, such as Oracle Database images.

> If you want to use a SIF container as though it were writable, you can create a directory, an ext3 file system image, or embed an ext3 file system image in SIF to use as a persistent overlay.

## Usage

we can use persistent overlay in the following scenarios.

### File system image overlay

You can use tools like `dd` and `mkfs.ext3` to create and format an empty ext3 file system image, which holds all changes made in your container within a single file.

#### Features

- Easy to transport (it is a single file)
- Faster than a directory (for HPC tasks)
- No privillege required
- Empty space information required

### Directory Overlay

Easier to use, but can't be transported as easily, also must be root.

### Overlay embedded in SIF

It is possible to embed an overlay image in the SIF file that holds a container. This allows the read-only container image and your modifications to it to be managed as a single file.

### Final note

If you mount your container without the `--overlay` directory, your changes will be gone.
