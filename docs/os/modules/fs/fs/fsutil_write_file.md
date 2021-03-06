## <font color="#F2853F" style="font-size:24pt">fsutil\_write\_file</font>

```no-highlight
int fsutil_write_file(const char *path, const void *data, uint32_t len)
```

Calls fs\_open(), fs\_write(), and fs\_close() to open a file at the specified path, write the supplied data to the current offset of the specified file handle, and close the file and invalidate the file handle.  If the specified file already exists, it is truncated and overwritten with the specified data.

#### Arguments

| Arguments | Description |
|-----------|-------------|
| path |  Pointer to the file to write to |
| data |  The data to write |
| len |  The number of bytes to write |

#### Returned values

* 0 on success
* [FS error code](fs_return_codes.md) on failure

#### Header file

```no-highlight
#include "fs/fs.h"
```

#### Example

This example creates a 4-byte file.

```no-highlight
int
write_id(void)
{
    int rc;

    /* Create the parent directory. */
    rc = fs_mkdir("/cfg");
    if (rc != 0 && rc != FS_EALREADY) {
        return -1;
    }

    /* Create a file and write four bytes to it. */
    rc = fsutil_write_file("/cfg/id.txt", "1234", 4);
    if (rc != 0) {
        return -1;
    }

    return 0;
}
```
