# Question 9 — C Program: Zombie Process Prevention

## Objective
Write a C program to demonstrate **zombie process prevention**.

The program should:
- Create **multiple child processes**
- Ensure that terminated child processes **do not remain as zombies**
- The **parent process should print the PID** of each child it cleans up

System calls to be used:
- `fork()`
- `wait()` or `waitpid()`

---

## Source File
**Filename:** zombie_prevention.c

---

## Program Code

```c
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <sys/wait.h>

int main() {
    int i;
    pid_t pid;

    printf("Parent PID: %d\n", getpid());

    // Create multiple child processes
    for (i = 0; i < 3; i++) {
        pid = fork();

        if (pid == 0) {
            // Child process
            printf("Child process created. PID: %d\n", getpid());
            sleep(2);   // Simulate some work
            exit(0);    // Child exits
        }
        else if (pid < 0) {
            // Fork failed
            perror("fork failed");
            exit(1);
        }
    }

    // Parent process waits for all children
    for (i = 0; i < 3; i++) {
        pid = wait(NULL);
        printf("Parent cleaned up child process with PID: %d\n", pid);
    }

    printf("All child processes cleaned up. No zombies remain.\n");
    return 0;
}

