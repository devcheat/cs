# **Proxy**
Proxy is a structural design pattern that provides an object that acts as a substitute for a real service object used by a client. A proxy receives client requests, does some work (access control, caching, etc.) and then passes the request to a service object.

The proxy object has the same interface as a service, which makes it interchangeable with a real object when passed to a client.

## Usage examples:
While the Proxy pattern isn’t a frequent guest in most C# applications, it’s still very handy in some special cases. It’s irreplaceable when you want to add some additional behaviors to an object of some existing class without changing the client code.

## Identification:
Proxies delegate all of the real work to some other object. Each proxy method should, in the end, refer to a service object unless the proxy is a subclass of a service.

> This pattern provides a surrogate or placeholder for another object to control access to it, often used for purposes like lazy initialization, access control, logging, or caching.

## Scenario
We'll create a scenario where we have an Image class that loads and displays an image. We'll implement a ProxyImage that controls access to the actual Image object.

```cs
namespace Proxy
{
    public interface IImage
    {
        void Display();
    }

    public class RealImage : IImage
    {
        private readonly string _fileName;

        public RealImage(string fileName)
        {
            _fileName = fileName;
            LoadImageFromDisk(); // Simulate loading image
        }

        private void LoadImageFromDisk()
        {
            Console.WriteLine($"Loading {_fileName} from disk.");
        }

        public void Display()
        {
            Console.WriteLine($"Displaying {_fileName}.");
        }
    }

    public class ProxyImage : IImage
    {
        private readonly string _fileName;
        private RealImage _realImage;

        public ProxyImage(string fileName)
        {
            _fileName = fileName;
        }

        public void Display()
        {
            if (_realImage == null)
            {
                _realImage = new RealImage(_fileName); // Lazy initialization
            }
            _realImage.Display();
        }
    }

    public class Program
    {
        public static void Main(string[] args)
        {
            IImage image1 = new ProxyImage("photo1.jpg");
            IImage image2 = new ProxyImage("photo2.jpg");

            // The image is loaded only when it is displayed
            image1.Display(); // Loading image
            image1.Display(); // Just displaying

            image2.Display(); // Loading image
        }
    }
}
```

## Summary
- **Subject Interface**: IImage defines the methods for image operations.
- **Real Subject**: RealImage implements the actual functionality for loading and displaying the image.
- **Proxy**: ProxyImage controls access to the real image, implementing lazy loading.
- **Client Code**: Demonstrates how the proxy delays loading the image until it is actually needed.