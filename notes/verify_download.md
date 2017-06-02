Verify A Download with Checksums
================================
When you download a file, especially an executable file, you should check that it is what it claims to be. Most of the time, a program will provide a checksum on the downloads page. Once you have downloaded the file, run a checksum locally to verify they are the same. Note this implicitly implies trust in the provider of the checksum, which is often from the webpage of the original download.

Right now you'll have to manually compare the resulting checksum and the supplied value on the website.


# Windows
certUtil is a built-in tool available on the command line. The syntax is:

```bash
$ certUtil -hashfile pathToFile [HashAlgorithm]
```

Where HashAlgorithm is one of: MD2, MD4, MD5, SHA1, SHA256, SHA384, SHA512.

# Linux
Use different utilities for different checksum algorithms, for example:

```bash
$ shasum -a 256 fileName # Calculate SHA256 sum of fileName
$ md5sum fileName # Calculate MD5 sum of fileName
```

# Sources
Cert Util: https://superuser.com/questions/245775/is-there-a-built-in-checksum-utility-on-windows-7
Kali Linux Guide to Verification: http://docs.kali.org/introduction/download-official-kali-linux-images
