# C语言 Linux 系统调用详解

本教程将深入学习Linux系统调用，包括文件操作、进程管理、信号处理等系统级编程。

## 1. 系统调用基础

### 系统调用vs库函数

```c
#include <stdio.h>
#include <unistd.h>
#include <sys/syscall.h>

// 库函数（高级接口）
void library_function() {
    printf("使用库函数printf\n");
    FILE *file = fopen("test.txt", "w");
    fprintf(file, "Hello\n");
    fclose(file);
}

// 系统调用（底层接口）
void system_call_example() {
    // write系统调用
    const char *msg = "使用系统调用write\n";
    syscall(SYS_write, STDOUT_FILENO, msg, strlen(msg));
    
    // open系统调用
    int fd = syscall(SYS_open, "test.txt", O_CREAT | O_WRONLY, 0644);
    if (fd >= 0) {
        syscall(SYS_write, fd, "Hello\n", 6);
        syscall(SYS_close, fd);
    }
}
```

## 2. 文件系统调用

### 文件操作系统调用

```c
#include <stdio.h>
#include <fcntl.h>
#include <unistd.h>
#include <sys/stat.h>
#include <sys/types.h>
#include <string.h>
#include <errno.h>

void file_syscalls() {
    int fd;
    char buffer[100];
    ssize_t bytes_read, bytes_written;
    
    // open: 打开文件
    fd = open("test.txt", O_CREAT | O_RDWR | O_TRUNC, 0644);
    if (fd < 0) {
        perror("open失败");
        return;
    }
    
    // write: 写入文件
    const char *data = "Hello, Linux System Call!";
    bytes_written = write(fd, data, strlen(data));
    printf("写入 %zd 字节\n", bytes_written);
    
    // lseek: 移动文件指针
    lseek(fd, 0, SEEK_SET);
    
    // read: 读取文件
    bytes_read = read(fd, buffer, sizeof(buffer) - 1);
    buffer[bytes_read] = '\0';
    printf("读取: %s\n", buffer);
    
    // fstat: 获取文件信息
    struct stat st;
    if (fstat(fd, &st) == 0) {
        printf("文件大小: %ld 字节\n", st.st_size);
        printf("文件权限: %o\n", st.st_mode & 0777);
    }
    
    // close: 关闭文件
    close(fd);
}
```

### 目录操作

```c
#include <stdio.h>
#include <dirent.h>
#include <sys/stat.h>
#include <unistd.h>

void list_directory(const char *dirname) {
    DIR *dir = opendir(dirname);
    if (dir == NULL) {
        perror("无法打开目录");
        return;
    }
    
    struct dirent *entry;
    struct stat st;
    char path[1024];
    
    printf("目录内容: %s\n", dirname);
    printf("--------------------------------\n");
    
    while ((entry = readdir(dir)) != NULL) {
        snprintf(path, sizeof(path), "%s/%s", dirname, entry->d_name);
        
        if (stat(path, &st) == 0) {
            if (S_ISDIR(st.st_mode)) {
                printf("[目录] %s\n", entry->d_name);
            } else if (S_ISREG(st.st_mode)) {
                printf("[文件] %s (%ld 字节)\n", entry->d_name, st.st_size);
            }
        }
    }
    
    closedir(dir);
}

// 创建目录
void create_directory_example() {
    if (mkdir("newdir", 0755) == 0) {
        printf("目录创建成功\n");
    } else {
        perror("目录创建失败");
    }
}

// 删除目录
void remove_directory_example() {
    if (rmdir("newdir") == 0) {
        printf("目录删除成功\n");
    } else {
        perror("目录删除失败");
    }
}
```

## 3. 进程系统调用

### fork和exec

```c
#include <stdio.h>
#include <unistd.h>
#include <sys/types.h>
#include <sys/wait.h>
#include <stdlib.h>

void fork_example() {
    pid_t pid = fork();
    
    if (pid < 0) {
        perror("fork失败");
        exit(EXIT_FAILURE);
    } else if (pid == 0) {
        // 子进程
        printf("子进程 PID: %d, 父进程 PID: %d\n", getpid(), getppid());
        exit(EXIT_SUCCESS);
    } else {
        // 父进程
        printf("父进程 PID: %d, 子进程 PID: %d\n", getpid(), pid);
        
        int status;
        waitpid(pid, &status, 0);
        printf("子进程已结束，退出码: %d\n", WEXITSTATUS(status));
    }
}

void exec_example() {
    pid_t pid = fork();
    
    if (pid == 0) {
        // 子进程：执行新程序
        execl("/bin/ls", "ls", "-l", NULL);
        perror("exec失败");
        exit(EXIT_FAILURE);
    } else {
        // 父进程等待
        wait(NULL);
        printf("ls命令执行完成\n");
    }
}
```

### 进程间通信

#### 管道

```c
#include <stdio.h>
#include <unistd.h>
#include <sys/types.h>
#include <sys/wait.h>
#include <string.h>

void pipe_example() {
    int pipefd[2];
    char buffer[100];
    
    // 创建管道
    if (pipe(pipefd) == -1) {
        perror("pipe创建失败");
        return;
    }
    
    pid_t pid = fork();
    
    if (pid == 0) {
        // 子进程：写入管道
        close(pipefd[0]);  // 关闭读端
        
        const char *message = "Hello from child!";
        write(pipefd[1], message, strlen(message));
        close(pipefd[1]);
        
        exit(EXIT_SUCCESS);
    } else {
        // 父进程：从管道读取
        close(pipefd[1]);  // 关闭写端
        
        ssize_t n = read(pipefd[0], buffer, sizeof(buffer) - 1);
        buffer[n] = '\0';
        printf("父进程收到: %s\n", buffer);
        
        close(pipefd[0]);
        wait(NULL);
    }
}
```

#### 共享内存

```c
#include <stdio.h>
#include <sys/shm.h>
#include <sys/ipc.h>
#include <string.h>
#include <unistd.h>
#include <sys/wait.h>

void shared_memory_example() {
    key_t key = ftok("/tmp", 'R');
    int shmid = shmget(key, 1024, IPC_CREAT | 0666);
    
    if (shmid == -1) {
        perror("共享内存创建失败");
        return;
    }
    
    // 附加到共享内存
    char *shm = (char *)shmat(shmid, NULL, 0);
    if (shm == (char *)-1) {
        perror("共享内存附加失败");
        return;
    }
    
    pid_t pid = fork();
    
    if (pid == 0) {
        // 子进程：写入共享内存
        strcpy(shm, "Hello from shared memory!");
        shmdt(shm);
        exit(EXIT_SUCCESS);
    } else {
        // 父进程：读取共享内存
        wait(NULL);
        printf("从共享内存读取: %s\n", shm);
        
        shmdt(shm);
        shmctl(shmid, IPC_RMID, NULL);  // 删除共享内存
    }
}
```

## 4. 信号处理

### 信号系统调用

```c
#include <stdio.h>
#include <signal.h>
#include <unistd.h>
#include <sys/types.h>

volatile sig_atomic_t keep_running = 1;

void signal_handler(int sig) {
    switch (sig) {
        case SIGINT:
            printf("\n收到SIGINT信号\n");
            keep_running = 0;
            break;
        case SIGTERM:
            printf("\n收到SIGTERM信号\n");
            keep_running = 0;
            break;
        case SIGUSR1:
            printf("收到SIGUSR1信号\n");
            break;
    }
}

void signal_example() {
    // 注册信号处理函数
    signal(SIGINT, signal_handler);
    signal(SIGTERM, signal_handler);
    signal(SIGUSR1, signal_handler);
    
    printf("进程PID: %d\n", getpid());
    printf("发送信号: kill -SIGUSR1 %d\n", getpid());
    
    while (keep_running) {
        printf("运行中...\n");
        sleep(1);
    }
    
    printf("程序退出\n");
}

// 发送信号
void send_signal_example(pid_t pid, int sig) {
    if (kill(pid, sig) == -1) {
        perror("发送信号失败");
    } else {
        printf("信号 %d 已发送到进程 %d\n", sig, pid);
    }
}
```

### 高级信号处理（sigaction）

```c
#include <stdio.h>
#include <signal.h>
#include <unistd.h>

void advanced_signal_handler(int sig, siginfo_t *info, void *context) {
    printf("收到信号 %d\n", sig);
    printf("发送者PID: %d\n", info->si_pid);
    printf("发送者UID: %d\n", info->si_uid);
}

void sigaction_example() {
    struct sigaction sa;
    
    sa.sa_sigaction = advanced_signal_handler;
    sigemptyset(&sa.sa_mask);
    sa.sa_flags = SA_SIGINFO;  // 使用sa_sigaction而不是sa_handler
    
    sigaction(SIGINT, &sa, NULL);
    sigaction(SIGTERM, &sa, NULL);
    
    printf("进程PID: %d，按Ctrl+C测试\n", getpid());
    pause();  // 等待信号
}
```

## 5. 内存管理

### mmap系统调用

```c
#include <stdio.h>
#include <sys/mman.h>
#include <fcntl.h>
#include <unistd.h>
#include <string.h>

void mmap_example() {
    int fd = open("mmap_test.txt", O_CREAT | O_RDWR, 0644);
    if (fd < 0) {
        perror("打开文件失败");
        return;
    }
    
    // 扩展文件大小
    ftruncate(fd, 4096);
    
    // 内存映射
    char *mapped = (char *)mmap(NULL, 4096, PROT_READ | PROT_WRITE,
                                MAP_SHARED, fd, 0);
    if (mapped == MAP_FAILED) {
        perror("mmap失败");
        close(fd);
        return;
    }
    
    // 写入内存映射区域
    strcpy(mapped, "Hello from mmap!");
    
    // 同步到磁盘
    msync(mapped, 4096, MS_SYNC);
    
    // 取消映射
    munmap(mapped, 4096);
    close(fd);
}
```

## 6. 网络系统调用

### Socket系统调用

```c
#include <stdio.h>
#include <sys/socket.h>
#include <netinet/in.h>
#include <arpa/inet.h>
#include <unistd.h>
#include <string.h>

void socket_syscalls() {
    int sockfd;
    struct sockaddr_in server_addr;
    char buffer[1024] = "Hello, Server!";
    char recv_buffer[1024] = {0};
    
    // 创建socket
    sockfd = socket(AF_INET, SOCK_STREAM, 0);
    if (sockfd < 0) {
        perror("socket创建失败");
        return;
    }
    
    // 设置服务器地址
    memset(&server_addr, 0, sizeof(server_addr));
    server_addr.sin_family = AF_INET;
    server_addr.sin_port = htons(8080);
    inet_pton(AF_INET, "127.0.0.1", &server_addr.sin_addr);
    
    // 连接服务器
    if (connect(sockfd, (struct sockaddr *)&server_addr,
                sizeof(server_addr)) < 0) {
        perror("连接失败");
        close(sockfd);
        return;
    }
    
    // 发送数据
    send(sockfd, buffer, strlen(buffer), 0);
    
    // 接收数据
    recv(sockfd, recv_buffer, sizeof(recv_buffer) - 1, 0);
    printf("收到: %s\n", recv_buffer);
    
    close(sockfd);
}
```

## 7. 时间系统调用

```c
#include <stdio.h>
#include <time.h>
#include <sys/time.h>
#include <unistd.h>

void time_syscalls() {
    // time: 获取时间戳
    time_t now = time(NULL);
    printf("当前时间戳: %ld\n", now);
    
    // gettimeofday: 高精度时间
    struct timeval tv;
    gettimeofday(&tv, NULL);
    printf("秒: %ld, 微秒: %ld\n", tv.tv_sec, tv.tv_usec);
    
    // clock_gettime: 更精确的时间
    struct timespec ts;
    clock_gettime(CLOCK_REALTIME, &ts);
    printf("秒: %ld, 纳秒: %ld\n", ts.tv_sec, ts.tv_nsec);
    
    // sleep: 睡眠（秒）
    printf("睡眠2秒...\n");
    sleep(2);
    
    // usleep: 微秒级睡眠
    usleep(500000);  // 0.5秒
    
    // nanosleep: 纳秒级睡眠
    struct timespec req = {0, 100000000};  // 0.1秒
    nanosleep(&req, NULL);
}
```

## 8. 综合示例

### 简单的Shell实现

```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <unistd.h>
#include <sys/wait.h>

#define MAX_LINE 1024
#define MAX_ARGS 64

void simple_shell() {
    char line[MAX_LINE];
    char *args[MAX_ARGS];
    char *token;
    int argc;
    
    while (1) {
        printf("myshell> ");
        
        if (fgets(line, MAX_LINE, stdin) == NULL) {
            break;
        }
        
        // 解析命令行
        argc = 0;
        token = strtok(line, " \t\n");
        while (token != NULL && argc < MAX_ARGS - 1) {
            args[argc++] = token;
            token = strtok(NULL, " \t\n");
        }
        args[argc] = NULL;
        
        if (argc == 0) {
            continue;
        }
        
        // 内置命令
        if (strcmp(args[0], "exit") == 0) {
            break;
        }
        
        if (strcmp(args[0], "cd") == 0) {
            if (argc > 1) {
                chdir(args[1]);
            }
            continue;
        }
        
        // 执行外部命令
        pid_t pid = fork();
        if (pid == 0) {
            execvp(args[0], args);
            perror("exec失败");
            exit(EXIT_FAILURE);
        } else {
            wait(NULL);
        }
    }
}

int main(void) {
    simple_shell();
    return 0;
}
```

## 练习

1. 实现一个文件复制程序（使用系统调用）
2. 创建一个进程监控程序
3. 实现一个简单的IPC通信系统
4. 编写一个信号处理程序，实现优雅关闭

