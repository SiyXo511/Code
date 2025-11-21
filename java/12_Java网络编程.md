# Java网络编程

本教程将学习Java中的网络编程，包括Socket编程、TCP/UDP通信、HTTP客户端等。

## 1. Socket编程基础

### TCP客户端

```java
import java.io.*;
import java.net.Socket;

public class TCPClient {
    public static void main(String[] args) {
        try {
            // 连接到服务器
            Socket socket = new Socket("localhost", 8080);
            System.out.println("已连接到服务器");
            
            // 发送数据
            PrintWriter out = new PrintWriter(socket.getOutputStream(), true);
            out.println("Hello, Server!");
            
            // 接收数据
            BufferedReader in = new BufferedReader(
                new InputStreamReader(socket.getInputStream()));
            String response = in.readLine();
            System.out.println("服务器响应: " + response);
            
            // 关闭连接
            socket.close();
        } catch (IOException e) {
            System.out.println("连接失败: " + e.getMessage());
        }
    }
}
```

### TCP服务器

```java
import java.io.*;
import java.net.ServerSocket;
import java.net.Socket;

public class TCPServer {
    public static void main(String[] args) {
        try {
            // 创建服务器Socket
            ServerSocket serverSocket = new ServerSocket(8080);
            System.out.println("服务器启动，等待客户端连接...");
            
            while (true) {
                // 等待客户端连接
                Socket clientSocket = serverSocket.accept();
                System.out.println("客户端已连接: " + clientSocket.getInetAddress());
                
                // 处理客户端请求
                handleClient(clientSocket);
            }
        } catch (IOException e) {
            System.out.println("服务器错误: " + e.getMessage());
        }
    }
    
    private static void handleClient(Socket clientSocket) {
        new Thread(() -> {
            try {
                // 接收数据
                BufferedReader in = new BufferedReader(
                    new InputStreamReader(clientSocket.getInputStream()));
                String request = in.readLine();
                System.out.println("收到请求: " + request);
                
                // 发送响应
                PrintWriter out = new PrintWriter(
                    clientSocket.getOutputStream(), true);
                out.println("服务器响应: " + request.toUpperCase());
                
                clientSocket.close();
            } catch (IOException e) {
                System.out.println("处理客户端错误: " + e.getMessage());
            }
        }).start();
    }
}
```

### UDP通信

```java
import java.net.*;

public class UDPClient {
    public static void main(String[] args) {
        try {
            // 创建UDP Socket
            DatagramSocket socket = new DatagramSocket();
            
            // 准备数据
            String message = "Hello, UDP Server!";
            byte[] data = message.getBytes();
            
            // 创建数据包
            InetAddress address = InetAddress.getByName("localhost");
            DatagramPacket packet = new DatagramPacket(
                data, data.length, address, 8081);
            
            // 发送数据包
            socket.send(packet);
            System.out.println("已发送: " + message);
            
            // 接收响应
            byte[] buffer = new byte[1024];
            DatagramPacket response = new DatagramPacket(buffer, buffer.length);
            socket.receive(response);
            
            String responseMessage = new String(response.getData(), 
                                               0, response.getLength());
            System.out.println("收到响应: " + responseMessage);
            
            socket.close();
        } catch (Exception e) {
            System.out.println("UDP客户端错误: " + e.getMessage());
        }
    }
}

public class UDPServer {
    public static void main(String[] args) {
        try {
            // 创建UDP Socket
            DatagramSocket socket = new DatagramSocket(8081);
            System.out.println("UDP服务器启动，等待数据...");
            
            while (true) {
                // 接收数据包
                byte[] buffer = new byte[1024];
                DatagramPacket packet = new DatagramPacket(buffer, buffer.length);
                socket.receive(packet);
                
                String message = new String(packet.getData(), 
                                           0, packet.getLength());
                System.out.println("收到: " + message + " 来自: " 
                                 + packet.getAddress());
                
                // 发送响应
                String response = "服务器收到: " + message;
                byte[] responseData = response.getBytes();
                DatagramPacket responsePacket = new DatagramPacket(
                    responseData, responseData.length,
                    packet.getAddress(), packet.getPort());
                socket.send(responsePacket);
            }
        } catch (Exception e) {
            System.out.println("UDP服务器错误: " + e.getMessage());
        }
    }
}
```

## 2. HTTP客户端

### 使用HttpURLConnection（Java原生）

```java
import java.io.*;
import java.net.HttpURLConnection;
import java.net.URL;

public class HTTPClient {
    public static void main(String[] args) {
        try {
            // 创建URL
            URL url = new URL("http://httpbin.org/get");
            
            // 打开连接
            HttpURLConnection connection = (HttpURLConnection) url.openConnection();
            
            // 设置请求方法
            connection.setRequestMethod("GET");
            
            // 设置请求头
            connection.setRequestProperty("User-Agent", "Java Client");
            
            // 获取响应码
            int responseCode = connection.getResponseCode();
            System.out.println("响应码: " + responseCode);
            
            // 读取响应
            BufferedReader in = new BufferedReader(
                new InputStreamReader(connection.getInputStream()));
            String inputLine;
            StringBuilder response = new StringBuilder();
            
            while ((inputLine = in.readLine()) != null) {
                response.append(inputLine);
            }
            in.close();
            
            System.out.println("响应内容:\n" + response.toString());
            
            connection.disconnect();
        } catch (Exception e) {
            System.out.println("HTTP请求错误: " + e.getMessage());
        }
    }
}
```

### POST请求示例

```java
import java.io.*;
import java.net.HttpURLConnection;
import java.net.URL;

public class HTTPPost {
    public static void main(String[] args) {
        try {
            URL url = new URL("http://httpbin.org/post");
            HttpURLConnection connection = (HttpURLConnection) url.openConnection();
            
            // 设置为POST请求
            connection.setRequestMethod("POST");
            connection.setDoOutput(true);
            connection.setRequestProperty("Content-Type", "application/json");
            
            // 发送数据
            String jsonData = "{\"name\":\"张三\",\"age\":25}";
            try (OutputStream os = connection.getOutputStream()) {
                byte[] input = jsonData.getBytes("utf-8");
                os.write(input, 0, input.length);
            }
            
            // 读取响应
            int responseCode = connection.getResponseCode();
            System.out.println("响应码: " + responseCode);
            
            BufferedReader in = new BufferedReader(
                new InputStreamReader(connection.getInputStream()));
            String inputLine;
            StringBuilder response = new StringBuilder();
            
            while ((inputLine = in.readLine()) != null) {
                response.append(inputLine);
            }
            in.close();
            
            System.out.println("响应内容:\n" + response.toString());
            
            connection.disconnect();
        } catch (Exception e) {
            System.out.println("POST请求错误: " + e.getMessage());
        }
    }
}
```

## 3. URL和URI

```java
import java.net.*;

public class URLDemo {
    public static void main(String[] args) {
        try {
            // 创建URL对象
            URL url = new URL("https://www.example.com:8080/path/to/resource?id=123#section");
            
            // 获取URL各部分
            System.out.println("协议: " + url.getProtocol());
            System.out.println("主机: " + url.getHost());
            System.out.println("端口: " + url.getPort());
            System.out.println("路径: " + url.getPath());
            System.out.println("查询: " + url.getQuery());
            System.out.println("锚点: " + url.getRef());
            System.out.println("完整URL: " + url.toString());
            
            // URI操作
            URI uri = new URI("https://www.example.com/path/to/resource?id=123");
            System.out.println("URI: " + uri.toString());
            System.out.println("是否绝对: " + uri.isAbsolute());
            
            // URL编码
            String encoded = URLEncoder.encode("Hello World", "UTF-8");
            System.out.println("编码: " + encoded);
            
            // URL解码
            String decoded = URLDecoder.decode(encoded, "UTF-8");
            System.out.println("解码: " + decoded);
        } catch (Exception e) {
            System.out.println("URL操作错误: " + e.getMessage());
        }
    }
}
```

## 4. 综合示例

### 简单的聊天程序（Socket）

```java
import java.io.*;
import java.net.Socket;
import java.util.Scanner;

public class ChatClient {
    public static void main(String[] args) {
        try {
            Socket socket = new Socket("localhost", 8080);
            System.out.println("已连接到聊天服务器");
            
            // 接收消息的线程
            Thread receiveThread = new Thread(() -> {
                try {
                    BufferedReader in = new BufferedReader(
                        new InputStreamReader(socket.getInputStream()));
                    String message;
                    while ((message = in.readLine()) != null) {
                        System.out.println("收到: " + message);
                    }
                } catch (IOException e) {
                    System.out.println("接收消息错误: " + e.getMessage());
                }
            });
            receiveThread.start();
            
            // 发送消息
            PrintWriter out = new PrintWriter(socket.getOutputStream(), true);
            Scanner scanner = new Scanner(System.in);
            
            while (true) {
                String message = scanner.nextLine();
                if ("exit".equals(message)) {
                    break;
                }
                out.println(message);
            }
            
            socket.close();
            scanner.close();
        } catch (IOException e) {
            System.out.println("客户端错误: " + e.getMessage());
        }
    }
}
```

## 练习

1. 实现一个简单的HTTP服务器
2. 创建一个文件传输程序（使用Socket）
3. 实现一个多客户端聊天服务器
4. 编写一个网络爬虫基础程序
5. 实现一个简单的REST API客户端

