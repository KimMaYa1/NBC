# 내일배움캠프 9일차 TIL | C# 팀과제

![image_720](https://github.com/KimMaYa1/NBC/assets/141565207/e84deae9-27a9-4728-a617-7bc512f9d10b)

<htr>https://github.com/KimMaYa1/SPT-B12

## 느낀점

- 오늘 과제를 진행하면서 팀프로젝트의 진행방법을 조금 느낀거 같아요
  처음에는 깃을 활용하기도 어려웠고 팀원간의 커뮤니케이션이 어려웠는데 시간이 조금 지나니 손발이 하나둘 맞아서 과제를 하는 재미가 있었습니다.

## LinQ

- Where
 - 배열에서 조건에 맞는 요소만으로 배열을 만들어냅니다.
    ``` 
    int[] numbers = { 5, 4, 1, 3, 9, 8, 6, 7, 2, 0 };

    var lowNums = from num in numbers
                  where num < 5
                  select num; //4,1,3,2,0
    int[] num = numbers.Where(num => num != 5 && num != 4).ToArray();   //1,3,9,8,6,7,2,0
    ``` 
 - 배열에서 추가로 삭제할때 좋은점이 있을거같다.

## TIL을 마치며

- 오늘은 새로운 팀원을 만나서 그런지 얘기를 많이했고 팀 과제를 진행하다보니 팀 프로젝트를 어떻게 진행하는지 대략적인 구조를 알게되서 괜찮은 날이었습니다.
  목요일까지 최선을 다해서 만족할수있는 프로젝트를 만들고 싶어요