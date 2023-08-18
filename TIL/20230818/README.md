# 내일배움캠프 3일차 TIL | C# 알고리즘, 스택

![image_720](https://github.com/KimMaYa1/NBC/assets/141565207/e84deae9-27a9-4728-a617-7bc512f9d10b)

<htr>https://github.com/shehdrbs123/spartaB10/blob/main/%EC%A1%B0%EB%B2%94%EC%A4%80/week4/Program.cs


## 느낀점

오늘 특강중에 들었던 고민하는 개발자가 되어라 라는 말이 너무 맘에 와닿았어요

## 스택

- 스택의 기본 개념
    - 스택은 기본적으로 LIFO 구조로 스택에서 변수를 꺼낼때 마지막에 들어간 변수가 나오게 된다

- 스택에 요소 넣기
    ```
    stack.push(1);
    ```
    - 스택에 1을 넣는다

- 스택에 마지막 요소 받기
    ```
    int num = stack.peek();
    ```
    - 마지막 요소를 삭제하지 않고 마지막 변수 값만 받아온다

- 스택에 마지막 요소 받기
    ```
    int num = stack.pop();
    ```
    - 마지막 요소를 삭제하고 마지막 변수 값만 받아온다
