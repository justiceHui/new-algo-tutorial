---
title: "Introduce to PS"
date: 2020-12-20 00:00:00
categories:
- Tutorial
tags:
- Tutorial
---

**Problem Solving에 입문하신 여러분을 진심으로 환영합니다.**

### What is PS/CP
Competitive Programming(CP)은 여러 참가자들이 **자료구조, 알고리즘, 수학**과 괸련된 문제를 해결하는 프로그램을 작성하는 Mind Sport의 일종입니다. 정보 올림피아드와 ICPC 등의 알고리즘 경시대회, 넓은 의미에서는 코딩 테스트도 CP에 포함됩니다.<br>
CP에서 주어진 문제를 해결하는 것을 Problem Solving(PS)라고 부릅니다.

문제들은 보통 아래 3가지를 요구합니다.
* 주어진 입력에 대해 올바른 정답을 출력해야 한다.
* 주어진 **시간 제한** 안에 프로그램이 종료되어야 한다.
* 프로그램 실행 중, 한순간이라도 주어진 **메모리 제한**보다 메모리를 많이 사용하면 안 된다.

주어진 입력에 대해 올바른 정답을 출력해야 하므로 **논리적으로 정당한 프로그램**을 작성해야 합니다. 또한 프로그램의 실행 시간과 메모리 사용에 제한이 걸려 있기 때문에 **효율적인 프로그램**을 작성해야 합니다.

주어진 입력, 올바른 정답을 출력, 시간 제한, 메모리 제한... Online Judge에서 문제를 풀어보지 않았다면 모두 생소한 개념일 수 있습니다. 실제 문제들을 체험해보며 하나씩 알아봅시다.

> PS와 CP를 구분해서 부르는 사람도 있고, 그렇지 않은 사랍도 있습니다. 구분해서 부르는 사람들은 알고리즘 문제를 해결하는 것(PS)와 그것으로 대회/경쟁을 하는 것(CP)에 따라 구분합니다. 자세한 내용은 [이 글](https://www.acmicpc.net/blog/view/49)을 참고해주세요.

### Online Judge - 문제 체험
Online Judge(OJ)란, 알고리즘 문제를 풀고 실시간으로 채점할 수 있는 곳입니다. 한국어로 된 유명한 온라인 저지는 [Baekjoon Online Judge(BOJ)](https://acmicpc.net), [KOISTUDY](http://koistudy.net/), [oj.uz](https://oj.uz/) 등이 있습니다.<br>
이 강의 시리즈는 주로 Baekjoon Online Judge를 사용하여 진행합니다.

[Baekjoon Online Judge](https://www.acmicpc.net/)에 들어가서 회원가입을 하고, [BOJ1000 A+B](https://www.acmicpc.net/problem/1000)문제를 봅시다.

* 시간 제한: 2초
* 메모리 제한: 128MB
* 문제: 두 정수 A와 B를 입력받은 다음, A+B를 출력하는 프로그램을 작성하시오.
* 입력: 첫째 줄에 A와 B가 주어진다. (0 < A, B < 10)
* 출력: 첫째 줄에 A+B를 출력한다.
* 예제 입력 1: `1 2`
* 예제 출력 1: `3`

두 정수를 입력받은 다음, 두 수의 합을 출력하는 프로그램을 작성하면 됩니다.<br>
이때 프로그램은 **2초** 이내에 종료되어야 하고, 메모리는 **128MB** 이하로 사용해야 합니다. `while(true)`, `malloc(128*1024*1024*2)`와 같은 행위를 하면 안 된다는 것을 의미합니다. `malloc`을 이용해 256MB를 할당한 뒤 프로그램이 종료되기 전에 `free`를 해도 128MB 이상 사용한 순간이 있기 때문에 소용 없습니다.

위 문제를 해결하는 코드는 다음과 같습니다.
```cpp
#include <stdio.h>

int main(){
  int a, b;
  scanf("%d %d", &a, &b);
  printf("%d", a+b);
}
```

온라인 저지마다 차이가 있을 수 있는데, BOJ를 포함한 대부분의 온라인 저지는 표준 입출력(stdin/stdout)을 사용합니다. 그러므로 scanf/printf를 이용해서 입출력을 하면 됩니다.

### Online Judge - 작동 방식
온라인 저지는 알고리즘 문제를 풀고 **실시간으로 채점**할 수 있는 곳입니다. 이때 **채점**이 어떤 과정을 통해 진행되는지 알아봅시다.

사이트 운영자, 혹은 어떤 인공지능이 여러분들이 제출한 코드가 맞는 코드인지 확인하는 것은 아닙니다.<br>
예제 입출력과 같이 사이트 내부에는 아래 표와 같은 여러 입출력 쌍이 존재합니다. 사이트는 단순히 여러분이 제출한 프로그램에 입력 파일을 넣어보고, 출력 결과가 정답 출력과 같은지 비교합니다.

| 입력 파일    | 입력 | 출력 파일     | 정답 출력 | 프로그램의 출력 | 채점 결과 |
| ------------ | ---- | ------------- | --------- | --------------- | --------- |
| example01.in | 1 2  | example01.out | 3         | 3               | O         |
| 01.in        | 3 4  | 01.out        | 7         | 7               | O         |
| 02.in        | 5 6  | 02.out        | 11        | 10              | X         |
| 03.in        | 7 8  | 03.out        | 15        | 15              | O         |

더 자세한 내용은 [이 글](https://www.acmicpc.net/blog/view/55)을 참고하세요.

### 마무리
이제 본격적으로 문제를 풀기 위해 필요한 자료구조와 알고리즘을 배워봅시다.
