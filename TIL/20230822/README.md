# 내일배움캠프 5일차 TIL | C# 배열정렬과 델리게이트

![image_720](https://github.com/KimMaYa1/NBC/assets/141565207/e84deae9-27a9-4728-a617-7bc512f9d10b)

<htr>https://github.com/shehdrbs123/spartaB10/blob/main/%EC%A1%B0%EB%B2%94%EC%A4%80/week5/Program.cs

## 느낀점

- 어제는 아파서 캠프에 참가를 못하였다 그래서 오늘은 어제만큼 열심히 해서 다른사람들을 따라가고 싶어요
  구현 하는데 어려움이 많았던 하루라서 새로 배우는게 많은거 같아 괜찮은 하루 였던거 같아요
  생각하는 힘이 늘어나는 느낌 ㅎㅎ

## 배열 정렬

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