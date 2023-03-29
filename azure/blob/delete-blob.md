**Delete Blob**
===


deleteBlob.cs
```cs
string BlobConnectionString = "conn string";
string BlobContainerName = "containerName";
string blobName = "blobfilename";
 // create object of your storage account  
var storageAccount = CloudStorageAccount.Parse(BlobConnectionString);

// create the client of your storage account  
var client = storageAccount.CreateCloudBlobClient();

// create reference of container  
var container = client.GetContainerReference(BlobContainerName);

// get a particular blob within that container  
var blockBlob = container.GetBlockBlobReference(blobName);

// delete the blob and return bool  
blockBlob.DeleteIfExistsAsync();
```
