#### Hard Links

- Hard links that reference the same file share the same inode struct, including the link count, access permissions, user + group ownership and file content. Changing the content for one link will also change it for all the other links. 
- **Note**: A hard link cannot point to a directory. 
- To create a hard link, we use the following command:
```
ln file1 file2
```

#### Symbolic Links

- They have 2 main advantages over hard links:
	- They can link 2 files on different file systems
	- They can point to directories or special files, not just to a regular file
- To create a symbolic link, we use the following command:
```
ln -s file1 file2
```
