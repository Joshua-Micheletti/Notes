Key changes:
- Region is now provided from frontend
- Filesystem structure is now fixed

The fixed structure of the filesystem is:
- Region
	- Area
		- Data
		- Media
		- Geom

Meaning that to check if a file is being uploaded to an area, you just need to check the parent path and make sure that it's in a subfolder of a region (requires region detection). To get the tag of the area, just use the first child of the Region folder of the added file or folder.

While creating a new area, the frontend needs to specify the Region the new area belongs to, since it's impossible to extract that info from every area type.

Apart from that, the frontend doesn't need to do anything else.

The affected endpoints from this change are:
