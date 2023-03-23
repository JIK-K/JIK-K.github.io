---
layout: post
title: "lseek() 파일 크기 계산"
categories: SystemProgramming
tags: [SystemProgramming]
---

## lseek() 란?

파일의 내용을 읽거나 쓰면 현재 읽을 위치나 쓸 위치를 알려주는 offset이 자동으로 변경된다.

파일을 읽기/쓰기 모드로 열었을 때 파일 읽기 오프셋과 쓰기 오프셋이 별도로 있는 것이 아니므로 주의해야한다.

오프셋을 원하는 위치로 바꾸고 위치를 확인하려면 lseek() 함수를 사용한다.

```함수원형
#include<sys/types.h>
#include<unistd.h>

off_t lseek(int fd, off_t offset, int whence);
```

**인자 설명**

    fd : 파일 기술자 (File Descriptor)
    offset : 이동할 오프셋 위치
    whence : 오프셋의 기준 위치

**whence 값**

    SEEK_SET : 파일의 시작을 기준으로 계산
    SEEK_CUR : 현재 위치를 기준으로 계산
    SEEK_END : 파일의 끝을 기준으로 계산

파일 size 계산 프로그램

```c
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <fcntl.h>
int main(int argc, char *argv[]) {
    int fd;
    off_t filesize;
    if (argc != 2) {
        printf("Usage: %s <filename>\n", argv[0]);
        exit(EXIT_FAILURE);
    }
    fd = open(argv[1], O_RDONLY);
    if (fd == -1) {
        perror("open");
        exit(EXIT_FAILURE);
    }
    filesize = lseek(fd, 0, SEEK_END);
    if (filesize == -1) {
        perror("lseek");
        exit(EXIT_FAILURE);
    }
    printf("File size is %ld bytes.\n", (long) filesize);
    if (close(fd) == -1) {
        perror("close");
        exit(EXIT_FAILURE);
    }
    return 0;
}
```
