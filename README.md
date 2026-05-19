# GOSFS-docs

the docs regarding the implementation of GOSFS

the header(first sector) is at hex addr 0x0 to 0x200
and in the header we have

1. magic
2. the version
3. no of super blocks(i hv not yet implemented)
4. no of suoer blocks used(derived from the fomula [%of superblocks used/total amt of superblocks] where %of superblocks used is no of blocks in a superblock used over 512)
5. reserved(this is to ensure that the header is exactly 512 bytes long[1 block])

the magic is GOSFSG*s<br/>
the version is currently at one<br/>
no of super blocks is how you can determine the partation size or set partition size as each superblock is responsible for 512(bytes[the size of a block])\*512(blocks) = 262144bytes so partation size can only be in mutiples of 262144 \_may change in the future with metadata*(discussed later)<br/>
no of super blocks used is to show how much disk is used and left with left being derived from(no of super blocks used - no of super blocks)

the super_block is how we tell is a block is used<br/>
the data will be stored in this format<br/>

in the sectors that store the files data we have

1. filename up to 255char long(ONLY ASCII)
2. the extension up to 15char long (ONLY ASCII oso)
3. flags used for metadata
   we will use the 4 bytes like this 0babcdefgh
   h will indicate if it is file or dir
   g is dependent on h if h is a dir(1) then g is the symlink flag 1 is true 0 is false
   if h is a file(0) then g is executable flag 1 is true 0 is false
   f is the flag that controls if the file/dir is hidden
   e is the flag for compressed flags
4. encoding (could be stored in the w bits in the superblocks)
5. the size of the file (size in bytes e.g. for 10kb -> 10000b)
6. start sector(might be shrunk and changed to relative to the superblock)
7. length (in blocks)
8. data(squeeze out some space to store data)

next steps(as in i nvr implement yet)
splitting files into many chunks NOT bef and aft one another(prob using meta(like if d bit 1 is 1 then look for first sector) using extent tbls)
stitching files
dirs plan to implement that the dirs will store the addr of the files inside and the parent dir and files will store addr of their parent dir so that will oso work for sys links
