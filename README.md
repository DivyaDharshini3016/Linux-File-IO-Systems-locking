# Linux-File-IO-Systems-locking
Ex07-Linux File-IO Systems-locking
# AIM:
To Write a C program that illustrates files copying and locking

# DESIGN STEPS:

### Step 1:

Navigate to any Linux environment installed on the system or installed inside a virtual environment like virtual box/vmware or online linux JSLinux (https://bellard.org/jslinux/vm.html?url=alpine-x86.cfg&mem=192) or docker.

### Step 2:

Write the C Program using Linux IO Systems locking

### Step 3:

Execute the C Program for the desired output. 

# PROGRAM:

## 1.To Write a C program that illustrates files copying 
```
#include <stdio.h>
#include <fcntl.h>
#include <unistd.h>
#include <stdlib.h> // for exit()

int main() {
    int src, dest, n;
    char buffer[100];

    // Open source file
    src = open("source.txt", O_RDONLY);
    if (src < 0) {
        perror("Error opening source file");
        exit(1);
    }

    // Open/create destination file
    dest = open("destination.txt", O_WRONLY | O_CREAT | O_TRUNC, 0644);
    if (dest < 0) {
        perror("Error opening destination file");
        close(src);
        exit(1);
    }

    // Copy file content
    while ((n = read(src, buffer, sizeof(buffer))) > 0) {
        if (write(dest, buffer, n) != n) {
            perror("Error writing to destination file");
            close(src);
            close(dest);
            exit(1);
        }
    }

    if (n < 0) {
        perror("Error reading source file");
    }

    printf("File copied successfully\n");

    close(src);
    close(dest);

    return 0;
}
```

## 2.To Write a C program that illustrates files locking
```
#include <stdio.h>
#include <stdlib.h>
#include <fcntl.h>
#include <unistd.h>
#include <string.h>

int main() {
    int fd;
    struct flock lock;
    char *text = "This is a locked write example.\n";

    // Open file for writing
    fd = open("file.txt", O_WRONLY | O_CREAT | O_APPEND, 0644);
    if (fd < 0) {
        perror("Error opening file");
        exit(1);
    }

    // Initialize lock structure
    memset(&lock, 0, sizeof(lock));
    lock.l_type = F_WRLCK;  // write lock
    lock.l_whence = SEEK_SET;
    lock.l_start = 0;
    lock.l_len = 0;  // lock whole file

    // Apply lock
    if (fcntl(fd, F_SETLKW, &lock) == -1) {
        perror("Error locking file");
        close(fd);
        exit(1);
    }

    printf("File locked. Writing data...\n");

    // Write to the file
    write(fd, text, strlen(text));
    printf("Data written successfully.\n");

    // Unlock file
    lock.l_type = F_UNLCK;
    if (fcntl(fd, F_SETLK, &lock) == -1) {
        perror("Error unlocking file");
    } else {
        printf("File unlocked.\n");
    }

    close(fd);
    return 0;
}
```

## OUTPUT

<img width="870" height="342" alt="image" src="https://github.com/user-attachments/assets/c5392f28-4f7a-4114-a55e-17882cc54e27" />

# RESULT:
The programs are executed successfully.
