# Question 10 — C Program: Signal Handling

## Objective
Write a C program to demonstrate **signal handling** using multiple processes.

The program should:
- Have a **parent process** that runs indefinitely
- Create a **child process** that sends `SIGTERM` after 5 seconds
- Create another **child process** that sends `SIGINT` after 10 seconds
- The **parent process handles each signal differently** and exits gracefully

Signals and system calls to be used:
- `signal()` or `sigaction()`
- `fork()`
- `kill()`
- `sleep()`

---

## Source File
**Filename:** signal_handling.c

---

## Program Code

```c
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <signal.h>

// Signal handler for SIGTERM
void handle_sigterm(int sig) {
    printf("\nParent received SIGTERM (Signal %d).\n", sig);
    printf("Performing cleanup for SIGTERM...\n");
}

// Signal handler for SIGINT
void handle_sigint(int sig) {
    printf("\nParent received SIGINT (Signal %d).\n", sig);
    printf("Performing cleanup for SIGINT...\n");
    printf("Exiting gracefully.\n");
    exit(0);
}

int main() {
    pid_t pid1, pid2;

    // Register signal handlers
    signal(SIGTERM, handle_sigterm);
    signal(SIGINT, handle_sigint);

    printf("Parent process running. PID: %d\n", getpid());

    // First child sends SIGTERM after 5 seconds
    pid1 = fork();
    if (pid1 == 0) {
        sleep(5);
        kill(getppid(), SIGTERM);
        exit(0);
    }

    // Second child sends SIGINT after 10 seconds
    pid2 = fork();
    if (pid2 == 0) {
        sleep(10);
        kill(getppid(), SIGINT);
        exit(0);
    }

    // Parent runs indefinitely
    while (1) {
        pause();
    }

    return 0;
}

