# 내일배움캠프 6일차 TIL | C# 개인과제

![image_720](https://github.com/KimMaYa1/NBC/assets/141565207/e84deae9-27a9-4728-a617-7bc512f9d10b)

<htr>https://github.com/shehdrbs123/spartaB10/blob/main/%EC%A1%B0%EB%B2%94%EC%A4%80/PrivateProject/Program.cs

## 느낀점

- 개인 과제를 진행하면서 초기 구성을 어떻게 해야 좋은지 많이 생각해보게 되는 과제였습니다
  추가 기능이 들어가면서 계속 클래스가 바뀌어서 힘든점이 많았습니다

## 개인과제 진행중 어려웠던 부분
   
 - 장착시 변경되는 점
    ```
    Item[] mountingItem = new Item[2];      //0번 무기 1번 방어구
    
    public void ItemPerformanceApplication(Item item)        //아이템이 장착유무에 따른 효과 적용 함수
    {
        if (item.mountingStatus)                //아이템이 장착 상태로 들어왔다면
        {   
            if (item.sortation == "무기")
            {
                if (mountingItem[0] == null)            //무기넣는 배열에 아무것도 없다면
                {
                mountingItem[0] = item;             //무기를 넣는다
                playerAttack += item.itemPerformance;
                }
                else                                    //무기넣는 배열에 무기가 존재한다면
                {
                    playerAttack -= mountingItem[0].itemPerformance;    //무기배열안에 있는 무기의 효과를 빼고
                    mountingItem[0].mountingStatus = false;         //무기배열의 장착상태를 false로 바꾸고
                    mountingItem[0] = item;                         //무기를 넣는다
                    playerAttack += item.itemPerformance;
                }
            }
            else if(item.sortation == "방어구")
            {
                if (mountingItem[1] == null)
                {
                    mountingItem[1] = item;
                    playerDefense += item.itemPerformance;
                }
                else
                {
                    playerDefense -= mountingItem[1].itemPerformance;
                    mountingItem[1].mountingStatus = false;
                    mountingItem[1] = item;
                    playerDefense += item.itemPerformance;
                }
            }
        }
    }
    ```
   - 크기가 2인 배열을 새롭게 만들어 아이템을 장착하라고 했을때 내가 끼고있는 무기나 방어구를 해제하고 입력받은 아이템을 장착하는것을 표현한 메서드입니다
     해당 메서드를 만들때 처음에 없던 배열을 만듦으로써 내가 가지고 있는 아이템의 장착 유무로 판단하는 것이 아닌 배열에 저장되어있으면 효과를 받게 함으로써
     관리하기 편리해졌다

 - 반복되는 작업을 재귀적이지 않게
    ```
    static void Main(string[] args)
    {
        bool isExit = true;
        while (isExit)
        {
            isExit = Abcd();
        }
    }

    public static bool Abcd()
    {
        Console.WriteLine("0입력하면 종료");
        string input = Console.ReadLine();


        if(int.TryParse(input, out int x))
        {
            if (x == 0)
            {
                return false;
            }
        }
        return true;
    }
    ```
   - 만약 0을 입력하지 않으면 true를 반환하기떄문에 메인문에서 다시 while문을 들어가기때문에 Abcd()메서드가 다시 실행된다

 - 배열의 추가나 삭제
    ```
    public void StoreItemAdd(Item item)
    {
        Array.Resize(ref items, items.Length + 1);          //items 배열의 정보는 유지하면서 배열의 크기 1증가시켜준다
        item.mountingStatus = false;
        items[items.Length - 1] = item;
    }
    ```
   - 배열의 크기를 추가하려고 new Item[숫자]를 한다면 기존에 있던 정보를 날리기에 ref를 사용하여 items배열을 참조해 items길이에 + 1 만큼 해서 다시 배열길이을 지정해주어서
     안에 정보를 그대로 두고 길이를 1 추가하여 새로운 아이템을 마지막 배열에 추가하게 된다

## 개인 과제를 진행하면서 느낀점

 - 처음 구조를 짤때 확장성보다는 그때그때 문제를 해결해 나가려고만 코드를 짜다보니 나중에 새로운 정보를 입력할때 고칠게 많았다
   나중에 팀프로젝트를 진행하게 되면 처음부터 어떤 부분이 필요한지 생각을 하고 확장성 있게 코드를 짜면 원활하게 짤수있을거같다
   오늘 배운걸 잊지 말자