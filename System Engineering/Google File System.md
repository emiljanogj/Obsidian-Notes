#GFS
***
- Files divided into fixed-size chunks identified by a 64-bit handle and stored in many chunkserver for reliability
- All fs metadata is stored in the master
- Chunks are stored as local files => Linux's buffer cache already keeps frequently accessed data in the memory

![[Pasted image 20230910134401.png]]

#### Namespace Management and Locking

- When we need to create a file, 2 locks are obtained:
	- A read lock for the directory to prevent the read/modify/snapshot of the directory
	- A write lock for the file


#### Garbage Collection

- When a file is deleted by the application, the master logs the deletion immediately. However, the file is not deleted immediately but rather renamed to a hidden name that includes the deletion timestamp
- The master then cleans up all these files during a regular scan. However, these files can still be restored and read under the new name until the clean is performed
- The master then identifies orphaned chunks and sends a *HeartBeat* message to the chunkservers to also delete those files