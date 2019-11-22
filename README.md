# fshash
Dump and check file hashes.

### Example

Run with -dump to get a list of all sha256 hashes in the directory you choose.

```
nemasu@midgar:~ $  fshash -dump /boot
9f3f2f64c5a0ffa62fbe4f0531115dd65e3fccdf550ace52b09a42afb62b4aa6 *./vmlinuz
51f4df2b3fb8eca253e23df7450e7edc0f5bf76d373dcbd93e85916baa9e4862 *./loader/entries/backup.conf
7b101e1dc436c0e26a204d796b055e16806780b418a44d5958825dd4cab198d7 *./loader/entries/noop.conf
03f2688fd89b6dd5dcdff471f261d6bd5892f5cea31c4ac470fbe58fd0835aba *./noop-initramfs.img
b24a1c40a4fea9d615416cc0405dcd5daa642060645dfae0acf4e50f8696a9ed *./intel-ucode.img
f295083ed62856a2b16f050065eb89422b3deb752264b0f6763844af396a3602 *./loader/loader.conf
87ee5ff061e77442a45e39b05e80e3ffea783109c4ee99798c6b1a02d460632c *./EFI/systemd/systemd-bootx64.efi
87ee5ff061e77442a45e39b05e80e3ffea783109c4ee99798c6b1a02d460632c *./EFI/BOOT/BOOTX64.EFI
9c71dacf579a39e33810a936d06715568271000c8e5d28047d0b6f99a5c7fd49 *./grub/grub.cfg
```

Save this to a file.
```
nemasu@midgar:~ $  fshash -dump /boot > boot.hsh
```

Run with -check to compare filesystem version to saved version.
```
nemasu@midgar:~ $  fshash -check /boot boot.hsh
```

If there are no changes, there will be no output, and will return 0.

### Errors

Currently there are 2 types of errors.

File count:
```
nemasu@midgar:~ $  fshash -dump /boot > boot.hsh
nemasu@midgar:~ $  sudo touch /boot/a
nemasu@midgar:~ $  fshash -check /boot boot.hsh 
Error: Size does not match, exiting.
DB: 9
FS: 10

```

Hash mismatch:
```
Error, missmatched hash for file *./a
FS: 48ce0fff0bd505e9b562a94a3e3dee9a435695f4a009b4877a5960d4fd6926ef
DB: e3b0c44298fc1c149afbf4c8996fb92427ae41e4649b934ca495991b7852b855

```

If a file exists in one place but not another, and there are equal number of files, it will show up as blank:
```
Error, missmatched hash for file *./grub/grub.snek
FS: 
DB: 9c71dacf579a39e33810a936d06715568271000c8e5d28047d0b6f99a5c7fd49
```

Any error will result in 1 being returned.
