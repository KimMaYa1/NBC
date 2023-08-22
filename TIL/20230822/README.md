# 내일배움캠프 5일차 TIL | C# 배열정렬과 델리게이트

![image_720](https://github.com/KimMaYa1/NBC/assets/141565207/e84deae9-27a9-4728-a617-7bc512f9d10b)

<htr>https://github.com/shehdrbs123/spartaB10/blob/main/%EC%A1%B0%EB%B2%94%EC%A4%80/week5/Program.cs

## 느낀점

- 어제는 아파서 캠프에 참가를 못하였다 그래서 오늘은 어제만큼 열심히 해서 다른사람들을 따라가고 싶어요
  구현 하는데 어려움이 많았던 하루라서 새로 배우는게 많은거 같아 괜찮은 하루 였던거 같아요
  생각하는 힘이 늘어나는 느낌 ㅎㅎ

## 배열 정렬 & 델리게이트

- Sort

  ```
  if ( num == 1)
  {
     Array.Sort(items, delegate (Item item1, Item item2)             //긴 문자가 위로
     {
         return item2.itemName.Replace(" ", "").Length.CompareTo(item1.itemName.Replace(" ", "").Length);    //Replace로 공백을 제거
     });
  }
  else if( num == 2)
  {
     Array.Sort(items, delegate (Item item1, Item item2)             //장착한 장비가 위로
     {
         return item2.mountingStatus.CompareTo(item1.mountingStatus);
     });
  }
  else
  {
     Array.Sort(items, delegate (Item item1, Item item2)             //능력치가 높은 순서로
     {
         return item2.itemPerformance.CompareTo(item1.itemPerformance);
     });

     if(num == 3)        //방어구
     {
         Array.Sort(items, delegate (Item item1, Item item2)             //방어구가 위로
         {
             if (item1.sortation == "방어구")
             {
                 return -1;
             }
             return 1;
         });
     }
     else                //무기
     {
         Array.Sort(items, delegate (Item item1, Item item2)             //무기가 위로
         {
             if (item1.sortation == "무기")
             {
                 return -1;
             }
             return 1;
         });
     }
  }
  ```

    - 위 코드에서 Array.Sort에 배열만 넣으면 숫자를 오름차순으로 정렬해주는 메서드인데 
      Array.Sort에 델리게이트를 넣어

 - 델리게이트 선언 & 초기화 방법

    - delegate 반환형 델리게이트명(매개변수..);

  ```
  delegate int DItem (int x, int y);

  static int Add(int x, int y)
  {
    return x + y;
  }
  
  class Program
  {
    static void Main()
    {
        DItem ditem = Add;  //메서드 등록(참조한다)

        int result = calc(3, 5);
        Consol.WriteLine("결과 : " + result);
    }
  }
  ```
  - 결과 : 8

  - 위처럼 메서드를 등록할수도 있으며 위 정렬에서 사용했던거처럼
    메서드를 선언하지 않고 그자리에서 메서드를 즉석에서 만드는 느낌이다

  ```
  Array.Sort(items, delegate (Item item1, Item item2)             //능력치가 높은게 0번으로 정렬
  {
    return item2.itemPerformance.CompareTo(item1.itemPerformance);    //item2(기준값) > item1(비교값) 1 | 반환 item2(기준값) < item1(비교값) -1 반환 | item2(기준값) = item1(비교값) 0반환
  });
  ```
 - 위 배열은 내림차순으로 정렬한 것이다 (item1과 2가 바꾸어 오름차순으로 만들고 Array.Reverse를 사용해 내림차순으로 만들수도 있다)

  ```
  Array.Sort(items, delegate (Item item1, Item item2)             //방어구가 0번으로 정렬
  {
    if (item1.sortation == "방어구")        //item1 이 방어구가 맞다면 -1를 반환
    {
        return -1;
    }
    return 1;
  });
  ```
 - 위 배열은 방어구가 맞다면 앞으로 배치하고 아니면 그대로 두느 정렬이다

 ## TIL을 마치며

 - 오늘은 개인 과제를 진행하면서 어려웠던 부분도 많았지만 튜터님의 도움으로 재귀적인 함수 호출을 풀어내어 메모리 소모를 줄이는 방법을 알아냈고
   델리게이트와 Sort ref등을 사용하여 코드들을 구성했는데 생각보다 어려워서 당황했다
   분명 배웠던 내용인데도 기억이 나지 않고 활용하지 못해서 인터넷에 검색을 많이 하는 하루 였다
   이 개인과제가 끝나면 문법들을 한번더 복습하는 시간을 가져야겠다