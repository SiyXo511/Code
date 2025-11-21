# C语言 Windows API 编程

本教程将学习C语言在Windows平台上的API编程，包括窗口创建、消息处理、文件操作等。

## 1. Windows API 基础

### 第一个Windows程序

```c
#include <windows.h>

// 窗口过程函数
LRESULT CALLBACK WindowProc(HWND hwnd, UINT uMsg, WPARAM wParam, LPARAM lParam) {
    switch (uMsg) {
        case WM_DESTROY:
            PostQuitMessage(0);
            return 0;
        case WM_PAINT: {
            PAINTSTRUCT ps;
            HDC hdc = BeginPaint(hwnd, &ps);
            TextOut(hdc, 10, 10, L"Hello, Windows!", 15);
            EndPaint(hwnd, &ps);
            return 0;
        }
    }
    return DefWindowProc(hwnd, uMsg, wParam, lParam);
}

int WINAPI WinMain(HINSTANCE hInstance, HINSTANCE hPrevInstance,
                   LPSTR lpCmdLine, int nCmdShow) {
    // 注册窗口类
    const wchar_t CLASS_NAME[] = L"Sample Window Class";
    
    WNDCLASS wc = {0};
    wc.lpfnWndProc = WindowProc;
    wc.hInstance = hInstance;
    wc.lpszClassName = CLASS_NAME;
    wc.hbrBackground = (HBRUSH)(COLOR_WINDOW + 1);
    wc.hCursor = LoadCursor(NULL, IDC_ARROW);
    
    RegisterClass(&wc);
    
    // 创建窗口
    HWND hwnd = CreateWindowEx(
        0,
        CLASS_NAME,
        L"我的第一个Windows程序",
        WS_OVERLAPPEDWINDOW,
        CW_USEDEFAULT, CW_USEDEFAULT, 800, 600,
        NULL, NULL, hInstance, NULL
    );
    
    if (hwnd == NULL) {
        return 0;
    }
    
    ShowWindow(hwnd, nCmdShow);
    
    // 消息循环
    MSG msg = {0};
    while (GetMessage(&msg, NULL, 0, 0)) {
        TranslateMessage(&msg);
        DispatchMessage(&msg);
    }
    
    return 0;
}

// 编译: gcc -mwindows win_program.c -o win_program.exe
```

## 2. 文件操作

### Windows文件API

```c
#include <windows.h>
#include <stdio.h>

void windows_file_operations() {
    HANDLE hFile;
    DWORD dwBytesWritten;
    DWORD dwBytesRead;
    char buffer[256] = "Hello, Windows File API!";
    char readBuffer[256] = {0};
    
    // 创建/打开文件
    hFile = CreateFile(
        L"test.txt",
        GENERIC_READ | GENERIC_WRITE,
        0,
        NULL,
        CREATE_ALWAYS,
        FILE_ATTRIBUTE_NORMAL,
        NULL
    );
    
    if (hFile == INVALID_HANDLE_VALUE) {
        printf("无法创建文件\n");
        return;
    }
    
    // 写入文件
    WriteFile(hFile, buffer, strlen(buffer), &dwBytesWritten, NULL);
    printf("写入 %lu 字节\n", dwBytesWritten);
    
    // 移动到文件开头
    SetFilePointer(hFile, 0, NULL, FILE_BEGIN);
    
    // 读取文件
    ReadFile(hFile, readBuffer, sizeof(readBuffer) - 1, &dwBytesRead, NULL);
    readBuffer[dwBytesRead] = '\0';
    printf("读取: %s\n", readBuffer);
    
    // 关闭文件
    CloseHandle(hFile);
}

// 获取文件信息
void get_file_info(const wchar_t *filename) {
    WIN32_FILE_ATTRIBUTE_DATA fileInfo;
    
    if (GetFileAttributesEx(filename, GetFileExInfoStandard, &fileInfo)) {
        printf("文件大小: %llu 字节\n",
               ((unsigned long long)fileInfo.nFileSizeHigh << 32) |
               fileInfo.nFileSizeLow);
        
        SYSTEMTIME st;
        FileTimeToSystemTime(&fileInfo.ftLastWriteTime, &st);
        printf("修改时间: %04d-%02d-%02d %02d:%02d:%02d\n",
               st.wYear, st.wMonth, st.wDay,
               st.wHour, st.wMinute, st.wSecond);
    }
}
```

## 3. 进程和线程

### 创建进程

```c
#include <windows.h>
#include <stdio.h>

void create_process_example() {
    STARTUPINFO si = {0};
    PROCESS_INFORMATION pi = {0};
    si.cb = sizeof(si);
    
    // 创建新进程
    if (CreateProcess(
        NULL,                    // 应用程序名
        L"notepad.exe",          // 命令行
        NULL,                    // 进程安全属性
        NULL,                    // 线程安全属性
        FALSE,                   // 句柄继承
        0,                       // 创建标志
        NULL,                    // 环境变量
        NULL,                    // 当前目录
        &si,                     // 启动信息
        &pi                      // 进程信息
    )) {
        printf("进程已创建，PID: %lu\n", pi.dwProcessId);
        
        // 等待进程结束
        WaitForSingleObject(pi.hProcess, INFINITE);
        
        CloseHandle(pi.hProcess);
        CloseHandle(pi.hThread);
    } else {
        printf("创建进程失败\n");
    }
}
```

### 创建线程

```c
#include <windows.h>
#include <stdio.h>

DWORD WINAPI ThreadFunction(LPVOID lpParam) {
    int thread_id = *(int *)lpParam;
    
    for (int i = 0; i < 5; i++) {
        printf("线程 %d: 计数 %d\n", thread_id, i);
        Sleep(1000);
    }
    
    return 0;
}

void create_thread_example() {
    HANDLE hThread;
    DWORD threadId;
    int param = 1;
    
    hThread = CreateThread(
        NULL,                    // 安全属性
        0,                       // 栈大小
        ThreadFunction,          // 线程函数
        &param,                  // 参数
        0,                       // 创建标志
        &threadId               // 线程ID
    );
    
    if (hThread != NULL) {
        printf("线程已创建，ID: %lu\n", threadId);
        
        // 等待线程结束
        WaitForSingleObject(hThread, INFINITE);
        
        CloseHandle(hThread);
    }
}
```

## 4. 注册表和配置

### 注册表操作

```c
#include <windows.h>
#include <stdio.h>

void registry_operations() {
    HKEY hKey;
    DWORD dwValue = 12345;
    DWORD dwType = REG_DWORD;
    DWORD dwSize = sizeof(dwValue);
    
    // 打开/创建注册表项
    if (RegCreateKeyEx(
        HKEY_CURRENT_USER,
        L"Software\\MyApp",
        0,
        NULL,
        REG_OPTION_NON_VOLATILE,
        KEY_WRITE,
        NULL,
        &hKey,
        NULL
    ) == ERROR_SUCCESS) {
        // 写入值
        RegSetValueEx(
            hKey,
            L"MyValue",
            0,
            REG_DWORD,
            (BYTE *)&dwValue,
            sizeof(dwValue)
        );
        
        // 读取值
        DWORD readValue;
        if (RegQueryValueEx(
            hKey,
            L"MyValue",
            NULL,
            &dwType,
            (BYTE *)&readValue,
            &dwSize
        ) == ERROR_SUCCESS) {
            printf("读取值: %lu\n", readValue);
        }
        
        RegCloseKey(hKey);
    }
}
```

## 5. 网络编程（Windows）

### Winsock基础

```c
#include <winsock2.h>
#include <ws2tcpip.h>
#include <stdio.h>

#pragma comment(lib, "ws2_32.lib")

void winsock_example() {
    WSADATA wsaData;
    SOCKET sock;
    struct sockaddr_in server;
    char *message = "Hello, Server!";
    char buffer[1024] = {0};
    
    // 初始化Winsock
    if (WSAStartup(MAKEWORD(2, 2), &wsaData) != 0) {
        printf("WSAStartup失败\n");
        return;
    }
    
    // 创建socket
    sock = socket(AF_INET, SOCK_STREAM, 0);
    if (sock == INVALID_SOCKET) {
        printf("socket创建失败\n");
        WSACleanup();
        return;
    }
    
    // 设置服务器地址
    server.sin_family = AF_INET;
    server.sin_port = htons(8080);
    inet_pton(AF_INET, "127.0.0.1", &server.sin_addr);
    
    // 连接服务器
    if (connect(sock, (struct sockaddr *)&server, sizeof(server)) < 0) {
        printf("连接失败\n");
        closesocket(sock);
        WSACleanup();
        return;
    }
    
    // 发送数据
    send(sock, message, strlen(message), 0);
    
    // 接收数据
    recv(sock, buffer, sizeof(buffer) - 1, 0);
    printf("收到: %s\n", buffer);
    
    closesocket(sock);
    WSACleanup();
}
```

## 6. 综合示例

### 简单的文本编辑器

```c
#include <windows.h>

LRESULT CALLBACK WindowProc(HWND hwnd, UINT uMsg, WPARAM wParam, LPARAM lParam) {
    static HWND hEdit;
    
    switch (uMsg) {
        case WM_CREATE: {
            // 创建编辑框
            hEdit = CreateWindow(
                L"EDIT",
                NULL,
                WS_CHILD | WS_VISIBLE | WS_VSCROLL | ES_MULTILINE | ES_AUTOVSCROLL,
                10, 10, 760, 540,
                hwnd,
                NULL,
                ((LPCREATESTRUCT)lParam)->hInstance,
                NULL
            );
            return 0;
        }
        case WM_SIZE: {
            // 调整编辑框大小
            MoveWindow(hEdit, 10, 10, LOWORD(lParam) - 20, HIWORD(lParam) - 20, TRUE);
            return 0;
        }
        case WM_DESTROY:
            PostQuitMessage(0);
            return 0;
    }
    return DefWindowProc(hwnd, uMsg, wParam, lParam);
}

int WINAPI WinMain(HINSTANCE hInstance, HINSTANCE hPrevInstance,
                   LPSTR lpCmdLine, int nCmdShow) {
    const wchar_t CLASS_NAME[] = L"TextEditorClass";
    
    WNDCLASS wc = {0};
    wc.lpfnWndProc = WindowProc;
    wc.hInstance = hInstance;
    wc.lpszClassName = CLASS_NAME;
    wc.hbrBackground = (HBRUSH)(COLOR_WINDOW + 1);
    
    RegisterClass(&wc);
    
    HWND hwnd = CreateWindowEx(
        0,
        CLASS_NAME,
        L"简单文本编辑器",
        WS_OVERLAPPEDWINDOW,
        CW_USEDEFAULT, CW_USEDEFAULT, 800, 600,
        NULL, NULL, hInstance, NULL
    );
    
    ShowWindow(hwnd, nCmdShow);
    
    MSG msg = {0};
    while (GetMessage(&msg, NULL, 0, 0)) {
        TranslateMessage(&msg);
        DispatchMessage(&msg);
    }
    
    return 0;
}
```

## 练习

1. 创建一个Windows计算器程序
2. 实现一个文件浏览器（列出目录内容）
3. 创建一个简单的聊天程序（使用Winsock）
4. 实现一个系统托盘程序

