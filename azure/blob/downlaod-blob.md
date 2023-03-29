**Download Blob**
===


downlaodBloab.cs
```cs
string BlobConnectionString = "connection string";
string BlobContainerName = "blobcontainerName";
// create object of your storage account  
var storageAccount = CloudStorageAccount.Parse(BlobConnectionString);

// create the client of your storage account  
var client = storageAccount.CreateCloudBlobClient();

// create reference of container  
var container = client.GetContainerReference(BlobContainerName);

// get a particular blob within that container  
var blockBlob = container.GetBlockBlobReference(blobName);

// get list of all blobs in the container  
//var allBlobs = container.ListBlobsSegmentedAsync();

// convert the blob to memorystream  
var memStream = new MemoryStream();
blockBlob.DownloadToStreamAsync(memStream).Wait(10000); //wait for 10 sec

//var rt = blockBlob.DownloadTextAsync().ConfigureAwait(false);
return memStream;
```
