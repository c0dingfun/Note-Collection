Showing Network Interfaces
====

```csharp
NetworkInterface
    .GetAllNetworkInterfaces()
    .Where(ni => ni.NetworkInterfaceType == NetworkInterfaceType.Wireless80211 ||
                 ni.NetworkInterfaceType == NetworkInterfaceType.Ethernet)
    .ToList()
    .ForEach(ni =>
    {
        var name = ni.Name;
        var ips = ni
                  .GetIPProperties()
                  .UnicastAddresses
                  .Where(ua => ua.Address.AddressFamily == AddressFamily.InterNetwork)
                  .Select(ua => ua.Address.ToString());

        Console.WriteLine($"{name}-> {string.Join(",", ips)}");
    });
```
