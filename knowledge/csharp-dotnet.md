# dotnet project management
Bootstrapping a new project using dotnet cli.
```shell
    dotnet new {project-template} -o {directory-and-project-name}
```

# SignalR
Documentation:
-   <https://docs.microsoft.com/en-us/aspnet/core/tutorials/signalr?view=aspnetcore-3.1&tabs=visual-studio-code>
-   Hub Protocol: <https://github.com/dotnet/aspnetcore/blob/master/src/SignalR/docs/specs/HubProtocol.md>

Protocol Terms:
-   **Caller:** The node that is issuing an Invocation, StreamInvocation, CancelInvocation, Ping messages and receiving Completion, StreamItem and Ping messages (a node can be both Caller and Callee for different invocations simultaneously)
-   **Callee:** The node that is receiving an Invocation, StreamInvocation, CancelInvocation, Ping messages and issuing Completion, StreamItem and Ping messages (a node can be both Callee and Caller for different invocations simultaneously)
-   **Binder:** The component on each node that handles mapping Invocation and StreamInvocation messages to method calls and return values to Completion and StreamItem messages

\* *The client and server have specific roles to fill, but either can play the role of Caller or Callee according to this protocol (a norm for two-communication protocols).*

SignalR is tested to fullfill:

> Reliable, in-order, delivery of messages - Specifically, the SignalR protocol provides no facility for retransmission or reordering of messages. If that is important to an application scenario, the application must either use a transport that guarantees it (i.e. TCP) or provide their own system for managing message order.

## Adding the SignalR client library
```shell
dotnet tool isntall -g Microsoft.Web.LibraryManager.Cli
libman install @microsoft/signalr@latest -p unpkg -d wwwroot/js/signalr --files dist/browser/signalr.js --files dist/browser/signalr.min.js
```

## SignalR Hub

A hub is a class that serves as a high-level pipeline for client-server communication.

``` csharp
    using Microsoft.AspNetCore.SignalR;
    using System.Threading.Tasks;
    
    namespace SignalRChat.Hubs
    {
        public class ChatHub : Hub
        {
            public async Task SendMessage(string user, string message)
            {
                await Clients.All.SendAsync("ReceiveMessage", user, message);
            }
        }
    }
```

## SignalR Configuration

AspNet starup configuration that passes SignalR messages to the SignalR library.

    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Threading.Tasks;
    using Microsoft.AspNetCore.Builder;
    using Microsoft.AspNetCore.Hosting;
    using Microsoft.AspNetCore.HttpsPolicy;
    using Microsoft.Extensions.Configuration;
    using Microsoft.Extensions.DependencyInjection;
    using Microsoft.Extensions.Hosting;
    using SignalRChat.Hubs; // note
    
    namespace SignalRChat
    {
        public class Startup
        {
            public Startup(IConfiguration configuration)
            {
                Configuration = configuration;
            }
    
            public IConfiguration Configuration { get; }
    
            // This method gets called by the runtime. Use this method to add services to the container.
            public void ConfigureServices(IServiceCollection services)
            {
                services.AddRazorPages();
                services.AddSignalR(); //note
            }
    
            // This method gets called by the runtime. Use this method to configure the HTTP request pipeline.
            public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
            {
                if (env.IsDevelopment())
                {
                    app.UseDeveloperExceptionPage();
                }
                else
                {
                    app.UseExceptionHandler("/Error");
                    // The default HSTS value is 30 days. You may want to change this for production scenarios, see https://aka.ms/aspnetcore-hsts.
                    app.UseHsts();
                }
    
                app.UseHttpsRedirection();
                app.UseStaticFiles();
    
                app.UseRouting();
    
                app.UseAuthorization();
    
                app.UseEndpoints(endpoints =>
                {
                    endpoints.MapRazorPages();
                    endpoints.MapHub<ChatHub>("/chathub"); //note
                });
            }
        }
    }


## SignalR Client Code

Client JS code is provided by the SignalR library on installation (typescript and WASM C# can also be used).

    <script src="~/js/signalr/dist/browser/signalr.js"></script>
    <!-- chat.js code is below, it is our example client implementation -->
    <script src="~/js/chat.js"></script>

This javascript attempts to access document nodes that aren&rsquo;t shown above, but the general process is as follows:

-   Create and start a conneciton
-   adds to a submit button a handler that sends messages **to** the hub.
-   adds to the conneciton object a handler that recieves messages **from** the hub and displays them.
```javascript
    "use strict";
    
    var connection = new signalR.HubConnectionBuilder().withUrl("/chatHub").build();
    
    //Disable send button until connection is established
    document.getElementById("sendButton").disabled = true;
    
    connection.on("ReceiveMessage", function (user, message) {
        var msg = message.replace(/&/g, "&amp;").replace(/</g, "&lt;").replace(/>/g, "&gt;");
        var encodedMsg = user + " says " + msg;
        var li = document.createElement("li");
        li.textContent = encodedMsg;
        document.getElementById("messagesList").appendChild(li);
    });
    
    connection.start().then(function () {
        document.getElementById("sendButton").disabled = false;
    }).catch(function (err) {
        return console.error(err.toString());
    });
    
    document.getElementById("sendButton").addEventListener("click", function (event) {
        var user = document.getElementById("userInput").value;
        var message = document.getElementById("messageInput").value;
        connection.invoke("SendMessage", user, message).catch(function (err) {
            return console.error(err.toString());
        });
        event.preventDefault();
    });
```
