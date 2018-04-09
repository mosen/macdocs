DFS Support
===========

`How DFS Works <https://technet.microsoft.com/en-us/library/cc782417(v=ws.10).aspx>`_.

Apple previously provided the source to the DFS client, the last of which seems to be Yosemite 10.10.5, which contains
`smb-759.40.1 <https://opensource.apple.com/source/smb/smb-759.40.1/>`_.

The **msdfs.c** file calls ``smb_log_info()`` which, in turn calls ASL.

