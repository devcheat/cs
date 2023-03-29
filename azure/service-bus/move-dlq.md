**Move message to DeadLetterQueue**
===


move.cs
```cs
string ServiceBusConnectionString = "";
string ServiceBusQueueName = "";
string locktoken = ""


var conStr = new ServiceBusConnectionStringBuilder(ServiceBusConnectionString);
conStr.EntityPath = ServiceBusQueueName; //Optional if it's not a part of connection string

var client = new QueueClient(conStr);

client.DeadLetterAsync(locktoken, message);
```
