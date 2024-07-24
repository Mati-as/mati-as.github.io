---
layout: single
title:  "Socket Programming Overview"
categories: "Network"
Tag: [Network]
toc: true
---
### General Overview of Socket Programming Example
This example demonstrates a basic implementation of a TCP server and client using C# and the `System.Net.Sockets` namespace. Socket programming enables communication between two nodes on a network, and this example showcases how to create a server that listens for incoming connections and a client that connects to the server, exchanges messages, and then closes the connection.

### Server Code Explanation
The server code initializes a socket to listen for incoming TCP connections. It binds the socket to an endpoint (defined by an IP address and port), starts listening for connections, and handles client requests in a loop. The server receives a message from the client, sends a response, and th n closes the connection.

1. **Namespace and Class Setup**: `namespace Servercore` and `class Program` define the scope and main class.
2. **DNS and IP Resolution**:
    - `string host = Dns.GetHostName()`: Gets the local machine's hostname.
    - `IPHostEntry ipHost = Dns.GetHostEntry(host)`: Resolves the hostname to an `IPHostEntry` instance containing IP addresses.
    - `IPAddress ipAddr = ipHost.AddressList[0]`: Selects the first IP address.
    - `IPEndPoint endPoint = new IPEndPoint(ipAddr, 7777)`: Creates an endpoint with the IP address and port 7777.
3. **Socket Creation**:
    - `Socket listenSocket = new Socket(endPoint.AddressFamily, SocketType.Stream, ProtocolType.Tcp)`: Creates a TCP socket.
4. **Binding and Listening**:
    - `listenSocket.Bind(endPoint)`: Binds the socket to the endpoint.
    - `listenSocket.Listen(10)`: Starts listening for incoming connections, with a backlog of 10.
5. **Handling Client Connections**:
    - In a `while (true)` loop:
        - `Socket clientSocket = listenSocket.Accept()`: Accepts an incoming connection.
        - `byte[] recvBuff = new byte[1024]`: Buffer for receiving data.
        - `int recvBytes = clientSocket.Receive(recvBuff)`: Receives data from the client.
        - `string recvData = Encoding.UTF8.GetString(recvBuff, 0, recvBytes)`: Decodes the received data.
        - `Console.WriteLine($"[From Client]: {recvData}")`: Prints the received data.
        - `byte[] sendBuff = Encoding.UTF8.GetBytes("Welcome to Mati's Server!")`: Prepares a response.
        - `clientSocket.Send(sendBuff)`: Sends the response to the client.
        - `clientSocket.Shutdown(SocketShutdown.Both)`: Shuts down the connection.
        - `clientSocket.Close()`: Closes the connection.
6. **Exception Handling**:
    - `catch (Exception e)`: Catches and prints any exceptions that occur.


    using System.Net;
using System.Net.Sockets;
using System.Text;
```C#
namespace Servercore
{
    class Program
    {
        static void Main(string[] args)
        {
            // get the hostname of the local machine
            string host = Dns.GetHostName();
            
            // get the IP address associated with the hostname
            IPHostEntry ipHost = Dns.GetHostEntry(host);
            IPAddress ipAddr = ipHost.AddressList[0];
            
            // define the endpoint with IP address and port
            IPEndPoint endPoint = new IPEndPoint(ipAddr, 7777);

            // create a socket to listen for incoming connections
            Socket listenSocket = new Socket(endPoint.AddressFamily, SocketType.Stream, ProtocolType.Tcp);

            try
            {
                // bind the socket to the endpoint
                listenSocket.Bind(endPoint);

                // start listening for incoming connections
                listenSocket.Listen(10);

                while (true)
                {
                    Console.WriteLine("Listening...");

                    // accept an incoming connection
                    Socket clientSocket = listenSocket.Accept();

                    // receive data from the client
                    byte[] recvBuff = new byte[1024];
                    int recvBytes = clientSocket.Receive(recvBuff);
                    string recvData = Encoding.UTF8.GetString(recvBuff, 0, recvBytes);
                    Console.WriteLine($"[From Client]: {recvData}");

                    // send a response to the client
                    byte[] sendBuff = Encoding.UTF8.GetBytes("Welcome to Mati's Server!");
                    clientSocket.Send(sendBuff);

                    // close the connection
                    clientSocket.Shutdown(SocketShutdown.Both);
                    clientSocket.Close();
                }
            }
            catch (Exception e)
            {
                Console.WriteLine(e.ToString());
            }
        }
    }
}
```
### Client Code Explanation

The client code initializes a socket, connects to the server, sends a message, receives a response, and then closes the connection.

1. **Namespace and Class Setup**: `namespace DummyClient` and `class Program` define the scope and main class.
2. **DNS and IP Resolution**:
    - `string host = Dns.GetHostName()`: Gets the local machine's hostname.
    - `IPHostEntry ipHost = Dns.GetHostEntry(host)`: Resolves the hostname to an `IPHostEntry` instance containing IP addresses.
    - `IPAddress ipAddr = ipHost.AddressList[0]`: Selects the first IP address.
    - `IPEndPoint endPoint = new IPEndPoint(ipAddr, 7777)`: Creates an endpoint with the IP address and port 7777.
3. **Socket Creation**:
    - `Socket socket = new Socket(endPoint.AddressFamily, SocketType.Stream, ProtocolType.Tcp)`: Creates a TCP socket.
4. **Connecting to Server**:
    - `socket.Connect(endPoint)`: Connects to the server.
    - `Console.WriteLine($"Connected to {socket.RemoteEndPoint.ToString()}")`: Prints the connection status.
5. **Sending and Receiving Data**:
    - `byte[] sendBuff = Encoding.UTF8.GetBytes("Hello Mati!")`: Prepares a message to send.
    - `int sendBytes = socket.Send(sendBuff)`: Sends the message to the server.
    - `byte[] recvBuff = new byte[1024]`: Buffer for receiving data.
    - `int recvBytes = socket.Receive(recvBuff)`: Receives data from the server.
    - `string recvData = Encoding.UTF8.GetString(recvBuff, 0, recvBytes)`: Decodes the received data.
    - `Console.WriteLine($"[From Server]: {recvData}")`: Prints the received data.
6. **Closing the Connection**:
    - `socket.Shutdown(SocketShutdown.Both)`: Shuts down the connection.
    - `socket.Close()`: Closes the connection.
7. **Exception Handling**:
    - `catch (Exception ex)`: Catches and prints any exceptions that occur.

```C#
using System.Net.Sockets;
using System.Net;
using System.Text;

namespace DummyClient
{
    class Program
    {
        static void Main(string[] args)
        {
            string host = Dns.GetHostName();
            IPHostEntry ipHost = Dns.GetHostEntry(host);
            IPAddress ipAddr = ipHost.AddressList[0];
            IPEndPoint endPoint = new IPEndPoint(ipAddr, 7777);

            // create a socket to connect to the server
            Socket socket = new Socket(endPoint.AddressFamily, SocketType.Stream, ProtocolType.Tcp);

            try
            {
                // connect to the server
                socket.Connect(endPoint);
                Console.WriteLine($"Connected to {socket.RemoteEndPoint.ToString()}");

                // send a message to the server
                byte[] sendBuff = Encoding.UTF8.GetBytes("Hello Mati!");
                int sendBytes = socket.Send(sendBuff);

                // receive a message from the server
                byte[] recvBuff = new byte[1024];
                int recvBytes = socket.Receive(recvBuff);
                string recvData = Encoding.UTF8.GetString(recvBuff, 0, recvBytes);
                Console.WriteLine($"[From Server]: {recvData}");

                // close the connection
                socket.Shutdown(SocketShutdown.Both);
                socket.Close();
            }
            catch (Exception ex)
            {
                Console.WriteLine(ex.ToString());
            }
        }
    }
}
```
