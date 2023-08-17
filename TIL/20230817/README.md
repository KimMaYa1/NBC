# 내일배움캠프 3일차 TIL | C# 인터스페이스

![image_720](https://github.com/KimMaYa1/NBC/assets/141565207/e84deae9-27a9-4728-a617-7bc512f9d10b)

<htr>https://github.com/shehdrbs123/spartaB10/blob/main/%EC%A1%B0%EB%B2%94%EC%A4%80/week4/Program.cs

## 느낀점

오늘 특강중에 들었던 고민하는 개발자가 되어라 라는 말이 너무 맘에 와닿았어요
오늘 진행한 강의와 과제에 진행하는 도중 구현이 어려운 부분들은 고민을 많이 하고 잘안풀려서 기죽었는데
자신감이 올라갔어요!

## 인터페이스

- 인터페이스 선언과 상속

 ```
 public interface IItemPickable
 {
    int item { get; set; }         //변수 선언
    void PickUp();              //함수 선언
 }

 public class Player : IItemPickable
 {
    int item { get; set; } 
    void PickUp(string i)
    {
        item = i;
    }
 }
 ```
    - 인터페이스 선언으로 변수나 함수를 선언만 해두고 클래스에서 재정의할수있다
    - 다중으로 상속할수있어, 같은 인터페이스를 가져와 다르게 정의할수있다

 ## 이외 여러가지

 - if(Console.KeyAvailable)
    - 키를 누르면 활성화
 - cards.RemoveAt(0);
    - 배열의 0번째 변수를 삭제후 앞에 변수들을 땡겨온다
 