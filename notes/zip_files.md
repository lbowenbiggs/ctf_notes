ZIP Files
---------
This document aims to explain what I know about .zip files. Things to add: 

- [ ] What happens if no Central Directory File header for a file in the .zip? No Local Header for a file listed in the Central Directory?
- [ ] How zip bombs work

# ZIP File Structure
You know you have a .zip file if the file ends with a central directory. There is no .zip magic header information, but the first few bytes of the file should be a local file header.

In the following Diagram, H1...Hn represent Local File Headers, and DATA1...DATAn is the compressed file data. E1...En are the Central Directory File Headers, and EOCD is the End Of Central Directory Record. 

```
+----+-------+----+-------+-----+----+-------+----+----+-----+----+------+
| H1 | DATA1 | H2 | DATA2 | ... | Hn | DATAn | E1 | E2 | ... | En | EOCD |
+----+-------+----+-------+-----+----+-------+----+----+-----+----+------+
```

In the .zip file, the Local headers duplicate some of the information in the corresponding Central Directory File Header. This is useful if some of the data has been corrupted/deleted. 

All multi-byte values in the header are stored little-endian (eg, 0x04034b50 reads as 50 4b 03 04).

For more complete detail on the layout, see [Wikipedia](https://en.wikipedia.org/wiki/Zip_(file_format)#Structure)

## Comparison of File Headers
Because most of the data is duplicated, The following is a comparision of the two headers (ommitting the file signatures). Blank entries in the Local File Header indicate data that is not duplicated. 

```
+---------------------------------------------+--------+
| Central Directory                           | Local  |
+--------+---+--------------------------------+--------+
| Offset | B | Description                    | Offset |
+--------+---+--------------------------------+--------+
| 4      | 2 | Version Made By                |        |
| 6      | 2 | Min. Version needed to Extract | 4      | 
| 8      | 2 | Gen. Purpose Bit Flag          | 6      |
| 10     | 2 | Compression Method             | 8      |
| 12     | 2 | Last Modification Time         | 10     |
| 14     | 2 | Last Modification Date         | 12     |
| 16     | 4 | CRC-32                         | 14     |
| 20     | 4 | Compressed Size                | 18     |
| 24     | 4 | Uncompressed Size              | 22     |
| 28     | 2 | File Name Len (n)              | 26     |
| 30     | 2 | Extra Field Len (m)            | 28     |
| 32     | 2 | File Comment Len (k)           |        |
| 34     | 2 | Disk Number where File Starts  |        |
| 36     | 2 | Internal File Attributes       |        |
| 38     | 2 | External File Attributes       |        |
| 42     | 4 | Rel. Offset of Local Header    |        |
| 46     | n | File Name                      | 30     |
| 46+n   | m | Extra Field                    | 30+n   |
| 46+n+m | k | File Comment                   |        |
+--------+---+--------------------------------+--------+
```

The Directory Header allows a program (or you, if you are determined enough) to find out what is in the .zip file before actually extracting. It also allows you to restore a broken or missing Local File Header (or vice-versa).
