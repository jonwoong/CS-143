Name: Jonathan Woong
UID: 804205763
TEAM: None

1. DiskHashedRelation.scala
	
	getIterator(): simply return iterator of input paramater "partitions"

	closeAllPartitions(): call closePartition() on each DiskPartition in the array

	insert(row: Row): call add() on data and check size of partition with measurePartitionSize()
		call spillPartitionToDisk() and clear() on data if partition size exceeds blockSize

	next(): simply call next() on the current iterator

	hasNext(): call built-in hasNext and check if it exists, if so, fetchNextChunk()

	fetchNextChunk(): check if chunkSizeIterator has a next chunk, if so, get the next chunk size
		by calling next(). Get the bytes in the next chunk with getNextChunkBytes() and 
		set the current iterator with getListFromBytes().iterator

	closeInput(): check if data is empty with isEmpty(), if not, spillPartitionToDisk() and 
		clear() the data

	apply(): generate a list of partitions with genDiskPartition()
		for each row in input, insert into the parition list with hash function hashCode()
		create a new array of DiskPartitions and generate a hashed relation with GeneralDiskHashedRelation()

2. CS143Utils.scala

	getUdfFromExpressions(): loop through each expression, use isInstanceOf to check whether 
		an expression is a ScalaUdf, if it is, return it

	hasNext(): use builtin hasNext on input, return it

	next(): create a new JavaArrayList, calculate preUdfProjection(), udfProject(), and 
		postUdfProjection() for each row and add to the new JavaArrayList
		Add the keys and values to the cache with put()

3. basicOperators.scala

	generateIterator(): create a DiskHashedRelation with DiskHashedRelation(), an iterator for it
	 	with getIterator(), a DiskPartition, and an iterator for the cache 

	 hasNext(): set the builtin hasNext to fetchNextPartition if it isn't already set

	 next(): call next() on the cache iterator

	 fetchNextPartition(): check if there's a next partition, if so, generate a caching iterator 
	 	with its data using generateCachingIterator() and set hasNext

	 

