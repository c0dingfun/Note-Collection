Object Serialization and De-serialization
=====

```csharp

// Convert an object to a byte array
public static byte[] ObjectToByteArray(Object obj)
{
    BinaryFormatter bf = new BinaryFormatter();
    using (var ms = new MemoryStream())
    {
        bf.Serialize(ms, obj);
        return ms.ToArray();
    }
}
```

- You just need copy this function to your code and send to it the object that you need convert to byte array. If you need convert the byte array to object again you can use the function below:

```csharp

// Convert a byte array to an Object
public static Object ByteArrayToObject(byte[] arrBytes)
{
    using (var memStream = new MemoryStream())
    {
        var binForm = new BinaryFormatter();
        memStream.Write(arrBytes, 0, arrBytes.Length);
        memStream.Seek(0, SeekOrigin.Begin);
        var obj = binForm.Deserialize(memStream);
        return obj;
    }
}
```

- You can use this function with custom classes. You just need add [Serializable] attribute in your class to enable serialization

[Ref] (http://stackoverflow.com/questions/1446547/how-to-convert-an-object-to-a-byte-array-in-c-sharp)