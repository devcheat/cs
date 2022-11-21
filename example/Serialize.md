Serialize & DeSerialize
===

The following is an extension method that can be used to Serialize & DeSerialize an object. This is not dependent on Newtonsoft.Json but instead uses `using System.Runtime.Serialization.Json`

```cs
using System.Runtime.Serialization.Json;

public static class Ext
{
        public static string SaveToXml<T>(this T t)
        {
            StringWriter stringwriter = new StringWriter();
            XmlSerializer serilizer = new XmlSerializer(typeof(T));
            serilizer.Serialize(stringwriter, t);
            return stringwriter.ToString();
        }

        public static T LoadFromXml<T>(this string xmlString)
        {
            var stringReader = new StringReader(xmlString);
            var serializer = new XmlSerializer(typeof(T));
            return (T)serializer.Deserialize(stringReader);
        }

        public static string SaveToJson<T>(this T t)
        {
            var stream1 = new MemoryStream();
            var ser = new DataContractJsonSerializer(typeof(T));
            ser.WriteObject(stream1, t);
            stream1.Position = 0;
            var sr = new StreamReader(stream1);
            return (sr.ReadToEnd());
        }

        public static T LoadFromJson<T>(this string jsonString)
        {
            T deserializedEntity = default(T);
            var ms = new MemoryStream(Encoding.UTF8.GetBytes(jsonString));
            var ser = new DataContractJsonSerializer(typeof(T));
            deserializedEntity = (T)ser.ReadObject(ms);// as T;
            ms.Close();
            return deserializedEntity;
        }
}
```
