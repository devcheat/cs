**Azure Service Bus: FIFO using Sessions**
===

> The primary use of Sessions (Groups) is to maintain FIFO when there are multiple receivers listening to the same Queue or Subscription.

Sessions are available only in the Standard and Premium tiers of Service Bus. And once sessions are enabled for an entity (Queue/Subscription), that specific entity can only receive messages that have the SessionId set to a valid value.

Sender.cs
```cs
public record ApplicationMessage(string Message);
 
class Program
{
    static async Task Main(string[] args)
    {
        await using var client = new ServiceBusClient(Shared.Configuration.CONNECTION_STRING);
 
        ServiceBusSender sender = client.CreateSender(Shared.Configuration.QUEUE_NAME);
 
        List<string> sessionIds = new() { "S1", "S2" };
 
        for (var i = 1; i <= 5; i++)
        {
            foreach (var sessionId in sessionIds)
            {
                var applicationMessage = new ApplicationMessage($"M{i}");
                await SendMessageAsync(sessionId, applicationMessage, sender);
            }
        }
    }
 
    static async Task SendMessageAsync(string sessionId, ApplicationMessage applicationMessage, ServiceBusSender serviceBusSender)
    {
        var message = new ServiceBusMessage(Encoding.UTF8.GetBytes(JsonConvert.SerializeObject(applicationMessage)))
        {
            // Note: Since I am sending to a Session enabled Queue, SessionId should not be empty
            SessionId = sessionId,
            ContentType = "application/json"
        };
 
        await serviceBusSender.SendMessageAsync(message);
 
        Console.WriteLine($"Message sent: Session '{message.SessionId}', Message = '{applicationMessage.Message}'");
    }
}
```


When Sessions are enabled for a queue, behind the scene it will create virtual queues. And the receivers can either choose a particular session they are interested in and Peek Lock that entire Session or pick whatever the next session which isn't already locked by any other receivers. So that particular receiver will only be processing messages that belongs to the session they locked in. And no other receivers would see those locked in session messages.

Receiver.cs
```cs
class Program
{
    static async Task Main(string[] args)
    {
        await using var client = new ServiceBusClient(Shared.Configuration.CONNECTION_STRING);
 
        var cts = new CancellationTokenSource();
        Console.CancelKeyPress += (a, o) => 
        {
             Console.WriteLine("---I am Dead!---");
             cts.Cancel(); 
        };
 
        do
        {
            // Here we are accepting the next Session which isn't locked by any other receiver
            ServiceBusSessionReceiver receiver = await client.AcceptNextSessionAsync(Shared.Configuration.QUEUE_NAME);
 
            Console.WriteLine($"Receiver started for SessionId: '{receiver.SessionId}'.");
 
            ServiceBusReceivedMessage message = null;
            do
            {
                message = await receiver.ReceiveMessageAsync(TimeSpan.FromSeconds(1), cancellationToken: cts.Token);
                if (message != null)
                {
                    try
                    {
                        Console.WriteLine($"Received: '{message.Body}', Ack: Complete");
                        await receiver.CompleteMessageAsync(message, cts.Token);
                    }
                    catch
                    {
                        Console.WriteLine($"Received: '{message.Body}', Ack: Abondon");
                        await receiver.AbandonMessageAsync(message, cancellationToken: cts.Token);
                    }
                }
            }
            while (message != null && !cts.IsCancellationRequested);
            await receiver.CloseAsync();
        }
        while (!cts.IsCancellationRequested);
    }
}

```

