# 내일배움캠프 10일차 TIL | C# 팀과제

![image_720](https://github.com/KimMaYa1/NBC/assets/141565207/e84deae9-27a9-4728-a617-7bc512f9d10b)

<htr>https://github.com/KimMaYa1/SPT-B12

## 느낀점

- 어제는 필수요소들을 만들었다면 오늘은 선택요구사항에서 몇가지를 만들었습니다
  프로젝트를 진행하면서 어떤부분을 맞춰가야하는지 알아봐서 좋은 하루였던거 같습니다.

## 팀과제

- 저같은 경우는 팀에서 씬화면을 보여주는 담당자였습니다 아래는 주요 코드들 입니다.

### 정보 입력 창

 - 디스플레이 창에서 대부분의 입력처리를 처리하는 메서드입니다. 
  ```
  public int InputString(int min, int max, int num, string str)
  {
    Console.WriteLine("{0}", str);
    Console.Write(">> ");
    string input = Console.ReadLine();
    int inputNum;
    if (num != 0 )
    {
        bool isSelect = int.TryParse(input, out inputNum);
        
        bool ismon = false;
        int monnum = inputNum - 1;
        if(monnum >= 0 && monnum < _monsters.Length)
        {
            if (_monsters[monnum].Hp <= 0)
                ismon = true;
        }

        while (!isSelect || !(inputNum >= min && inputNum <= max) || ismon)
        {
            Console.WriteLine("=====================");
            Console.WriteLine("  잘못된 대상입니다");
            Console.WriteLine("=====================");
            Console.WriteLine();
            Console.WriteLine("다시 입력해주세요.");
            Console.Write(">> ");
            input = Console.ReadLine();
            isSelect = int.TryParse(input, out inputNum);
            monnum = inputNum - 1;
            ismon = false;
            if (monnum >= 0 && monnum < _monsters.Length)
            {
                if (_monsters[monnum].Hp <= 0)
                    ismon = true;
            }
        }
    }
    else
    {
        while (!int.TryParse(input, out inputNum) || !(inputNum >= min && inputNum <= max))
        {
            Console.WriteLine("=====================");
            Console.WriteLine("  잘못된 입력입니다");
            Console.WriteLine("=====================");
            Console.WriteLine();
            Console.WriteLine("다시 입력해주세요.");
            Console.Write(">> ");
            input = Console.ReadLine();
        }
    }
    return inputNum;
  }
  ```
 - 위 코드를 불러올때 매개변수로 int min, int max, int num, string str 이렇게 있는데 min과 max는 선택지의 범위를 넣어주고 
   선택지를 벗어나게 되면 while문에 걸려 잘못된 입력으로 판단하고 선택지를 입력하게 while문을 돌립니다
   num같은 경우는 1을 넣으면 죽은 몬스터를 공격했는지 판단하기 위해 넣었습니다 마지막 str은 어떤 문장을 출력할지 넣어주는 부분입니다

### 게임 재시작

 - 게임을 재시작하거나 나가게하는 메서드입니다
  ```
  public void ReStart()
  {
    Console.WriteLine("0. 다시시작");
    Console.WriteLine("1. 나가기");
    Console.WriteLine();
    int input = InputString(0, 1, 0, "다시 시작하시겟습니까?");
    Console.WriteLine();
    if (input == 0)
    {
        RestartApplication();
    }
    else if(input == 1)
    {
        Environment.Exit(0);
    }
  }
  public void RestartApplication()
  {
    string fileName = Process.GetCurrentProcess().MainModule.FileName;
    Process.Start(fileName);
    Environment.Exit(0);
  }
  ```
 - RestartApplication메서드에서 현재 콘솔이름정보를 fileName에 넣고 재시작을 해버린후 Environment.Exit(0)을 사용하여 현재 콘솔을 종료시킵니다.

### 공격 정보 출력

 - 공격을 실행했을때 어떤캐릭터가 공격하고 받았는지 알려주는 메서드입니다.
  ```
  public void AttackInfo(Character aCharacter, Character tCharacter)
  {
    Console.WriteLine();
    if (tCharacter.Evasion())
    {
        Console.WriteLine("========================================");
        Console.WriteLine(" {0}이(가) {1}의 공격을 회피하였습니다.", tCharacter.Name, aCharacter.Name);
        Console.WriteLine("========================================");
    }
    else
    {
        int damage = tCharacter.TakeDamage(aCharacter.Atk);
        Console.WriteLine("==============================================");
        Console.WriteLine(" {0}이(가) {1}에게 {2}의 데미지를 입혔습니다", aCharacter.Name, tCharacter.Name, damage);
        Console.WriteLine("==============================================");
        Console.WriteLine();
        if (tCharacter.Hp < damage)
        {
            Console.WriteLine("{0}의 체력 {1} -> 0", tCharacter.Name, tCharacter.Hp);
            tCharacter.Hp = 0;
        }
        else
        {
            Console.WriteLine("{0}의 체력 {1} -> {2}", tCharacter.Name, tCharacter.Hp, tCharacter.Hp - damage);
            tCharacter.Hp -= damage;
        }
    }
    Console.WriteLine();
    Thread.Sleep(1000);
  }
  ```
 - 몬스터클래스와 플레이어클래스는 캐릭터 클래스를 상속받기때문에 매개변수로 앞에는 때린 캐릭터 뒤에는 맞은캐릭터를 넣어서 처리했습니다.

## TIL을 마치며

- 오늘은 크게 어려운 문제들이 많지 않아 오류에대해 어떤 풀이가 있는지 적지 않았지만 프로젝트자체는 진척이 많이되서 좋았습니다
  내일도 힘내자 홧팅