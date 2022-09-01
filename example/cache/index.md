# Cache

Example of in memory cache storage

```cs
// Simple storage 
public class Cache<T>
{
	public T Data;
	string FILE_NM = null;
	public Type DataType {get;private set;}
	public bool Compress { get; set;}
	public bool EncodeData { get; set;} 
	public bool InMemory { get; set;}
	
	private readonly string DIR_NM = "Cache";
	
	public Cache(string uniquename)
	{
		if (!Directory.Exists(DIR_NM))               
            Directory.CreateDirectory(DIR_NM);		
			
		FILE_NM = string.Format("{0}/{1}",DIR_NM,uniquename);
		this.Compress = true;
		this.EncodeData = true;
		this.DataType = typeof(T);
		this.InMemory = false;
	}
	
	private string Base64Encode(string plainText) 
	{
	  var plainTextBytes = System.Text.Encoding.UTF8.GetBytes(plainText);
	  return System.Convert.ToBase64String(plainTextBytes);
	}

	private string Base64Decode(string base64EncodedData) 
	{
	  var base64EncodedBytes = System.Convert.FromBase64String(base64EncodedData);
	  return System.Text.Encoding.UTF8.GetString(base64EncodedBytes);
	}
	
	private string JsonSerialize(T data)
	{
		JavaScriptSerializer js = new JavaScriptSerializer();  
		string jsonData = js.Serialize(data);
		return jsonData;
	}
	
	private T JsonDeserialize(string jsonData)
	{
		JavaScriptSerializer js = new JavaScriptSerializer();  
		T TObject = js.Deserialize<T>(jsonData);  
		return TObject;
	}
	
	public void Store(T data) 
	{
		JavaScriptSerializer js = new JavaScriptSerializer(); 
		js.MaxJsonLength = Int32.MaxValue;
		string jsonData = js.Serialize(data);
		
		if(EncodeData == true)
			jsonData = Base64Encode(jsonData);
		
		byte[] encoded = Encoding.UTF8.GetBytes(jsonData);
		
		if(Compress)
		{
			using(FileStream fStream = new FileStream(FILE_NM, FileMode.Create, FileAccess.Write))
		    using(GZipStream cStream = new GZipStream(fStream, CompressionLevel.Optimal))
			{
				cStream.Write(encoded, 0, encoded.Length);
				cStream.Flush();
			}
		}
		else
		{
			using(FileStream fStream = new FileStream(FILE_NM, FileMode.Create, FileAccess.Write))		    
			{
				fStream.Write(encoded, 0, encoded.Length);
				fStream.Flush();
			}
		}
		
	}
	
	
	public object GetObj() 
	{
		JavaScriptSerializer js = new JavaScriptSerializer();  
		js.MaxJsonLength = Int32.MaxValue;
		
	    object Tobject = null;
		
		
		if(Compress)
		{
			
		    using(FileStream fStream = new FileStream(FILE_NM, FileMode.Open, FileAccess.Read))
		    using(GZipStream cStream = new GZipStream(fStream, CompressionMode.Decompress))
			using (var mStream = new MemoryStream())
		    {
				cStream.CopyTo(mStream);
				
				var result = getString(mStream);
				if(EncodeData == true)
					result = Base64Decode(result);
				Tobject = js.Deserialize<object>(result);
		    }
		}
		else
		{
			using (var mStream = new MemoryStream())
		    using(FileStream fStream = new FileStream(FILE_NM, FileMode.Open, FileAccess.Read))		   
		    {
				fStream.CopyTo(mStream);
				var result = getString(mStream);
				if(EncodeData == true)
					result = Base64Decode(result);
				Tobject = js.Deserialize<object>(result);
		    }
		}
		
	    return Tobject;
	}
	
	public T Get() 
	{
		JavaScriptSerializer js = new JavaScriptSerializer();  
		js.MaxJsonLength = Int32.MaxValue;
		
	    T Tobject = default(T);;
		
		
		if(Compress)
		{
			
		    using(FileStream fStream = new FileStream(FILE_NM, FileMode.Open, FileAccess.Read))
		    using(GZipStream cStream = new GZipStream(fStream, CompressionMode.Decompress))
			using (var mStream = new MemoryStream())
		    {
				cStream.CopyTo(mStream);
				
				var result = getString(mStream);
				if(EncodeData == true)
					result = Base64Decode(result);
				Tobject = js.Deserialize<T>(result);
		    }
		}
		else
		{
			using (var mStream = new MemoryStream())
		    using(FileStream fStream = new FileStream(FILE_NM, FileMode.Open, FileAccess.Read))		   
		    {
				fStream.CopyTo(mStream);
				var result = getString(mStream);
				if(EncodeData == true)
					result = Base64Decode(result);
				Tobject = js.Deserialize<T>(result);
		    }
		}
		
	    return Tobject;
	}
	
	private string getString(MemoryStream BASE)
    {
        BASE.Position = 0;
        StreamReader R = new StreamReader(BASE);
        return R.ReadToEnd();
    }
	
	/* Binary serilization discarded as it needs serialized attribute
	public void Set(T data) 
	{
		
	    BinaryFormatter bf = new BinaryFormatter();;
	    using(FileStream fileStream = new FileStream(FILE_NM, FileMode.Create, FileAccess.Write))
	    using(GZipStream compressionStream = new GZipStream(fileStream, CompressionMode.Compress))
	    {
	        bf.Serialize(compressionStream, data);
			//compressionStream.Write(
	    }
	}

	public  T Get() 
	{
	    BinaryFormatter bf = new BinaryFormatter();;
	    T Tobject;
	    using(FileStream fileStream = new FileStream(FILE_NM, FileMode.Open, FileAccess.Read))
	    using(GZipStream compressionStream = new GZipStream(fileStream, CompressionMode.Decompress))
	    {
	        Tobject = (T)bf.Deserialize(compressionStream);
	    }

	    return Tobject;
	}*/
	
	~Cache()
	{
		if(InMemory == true && !string.IsNullOrEmpty(FILE_NM))
			File.Delete(FILE_NM);
		//File.Move(FILE_NM, FILE_NM + " Archive" + DateTime.Now.ToString("hhmmssfff"));
	}

}
  ```
