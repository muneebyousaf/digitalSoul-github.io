---
title:  EncFS - Encrypted File System
---
# EncFS - Encrypted File System
EncFS, short for Encrypted File System, is an open-source cryptographic filesystem that provides an encrypted view of an existing directory structure. It allows you to securely store sensitive data on your computer while maintaining the convenience of working with regular files and directories.

## How EncFS Works

EncFS works by encrypting the content of your files and directories on a per-file basis. When you access an encrypted file, EncFS transparently decrypts it on-the-fly, allowing you to work with the unencrypted data as if it were a regular file. Similarly, when you save changes to an encrypted file, EncFS automatically encrypts the new data before storing it on the disk.

## Key Features

- **Ease of Use**: EncFS is user-friendly and easy to set up. It doesn't require complex configurations or changes to your existing filesystem structure.

- **Cross-Platform**: EncFS is compatible with various operating systems, including Linux, macOS, and Windows. This cross-platform support makes it a versatile choice for securing your data.

- **Strong Encryption**: EncFS uses strong encryption algorithms to protect your data, ensuring that even if an unauthorized user gains access to your files, they won't be able to decipher their content without the encryption key.

- **Mounting and Unmounting**: You can mount and unmount EncFS directories as needed. When a directory is mounted, its contents are accessible in plaintext; when unmounted, they are securely encrypted.

## Use Cases

EncFS is suitable for various use cases, including:

- **Securing Personal Files**: You can use EncFS to encrypt and protect your personal documents, photos, and other sensitive files.

- **Cloud Storage**: Encrypt files before uploading them to cloud storage services to ensure privacy and security.

- **Secure Data Transfer**: Encrypt files for secure sharing with others over untrusted networks.

- **Data Backup**: Keep encrypted backups of important data, making it safe from unauthorized access.

## Installation on Ubuntu

To install EncFS on Ubuntu, you can use the following commands:

```bash
sudo apt update
sudo apt install encfs
```

## Mounting and Unmounting EncFS Directories

### Mounting an EncFS Directory

To mount an EncFS directory, use the ==encfs== command followed by the source directory (the encrypted directory) and the destination directory (where you want to access the unencrypted content):

```bash
encfs /path/to/source_directory /path/to/destination_directory
```

You'll be prompted to enter your EncFS password. Once entered, the contents of the source directory will be accessible in the destination directory.

### Unmounting an EncFS Directory

To unmount an EncFS directory, use the ==fusermount== command followed by the -u option and the destination directory:
```bash
fusermount -u /path/to/destination_directory
```

This will unmount the EncFS directory, securing its contents with encryption.




