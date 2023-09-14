# 내일배움캠프 8일차 TIL | C# 알고리즘 풀이 (풀이중 알아낸 정보)

![image_720](https://github.com/KimMaYa1/NBC/assets/141565207/e84deae9-27a9-4728-a617-7bc512f9d10b)

<htr>https://github.com/KimMaYa1/BaekJoonEx

## 느낀점

- 오늘 알고리즘을 풀어가면서 더 빠른 방법이 있는지 찾아보면서 StringBuilder를 알게 되었는데
  이걸 사용하기위해 좀더 많이 써보고 받아들여야할거같다
  모든 문제들이 그런거 같다 많이 써버고 어디서 뭘 써야 효율적인지 확인하는것이 알고리즘이 아닌가 생각한다

## 풀어낸 문제들

- 가변 배열
    ``` 
    int[][] a = new int[5][];
    for(int i = 0; i < a.Length; i++)
    {
        a[i] = new int[2];
    }

    int[][] b = new int[3][];
    b[0] = new int[] { 31, 32 };
    b[1] = new int[] { 33, 34, 35 };
    b[2] = new int[] { 36, 37, 38, 39 };
    ```
 - 가변 배열같은 경우 1차원 배열의 크기는 같지만 2차원 배열의 크기가 다를때 자주 사용하는데
   1차원 배열같은 경우 선언 과 동시에 초기화가 가능하지만 2차원 배열부터는 따로 초기화 시켜주어야 한다


- 다차원 배열의 길이 구하기
    ```
    int[,] a= new int[2,3];
    int b = a.GetLength(0);     //2
    int c = a.GetLength(1);     //3

    int[][] d = new int[3][];
    d[0] = new int[] { 31, 32 };
    d[1] = new int[] { 33, 34, 35 };
    d[2] = new int[] { 36, 37, 38, 39 };
    b = d.Length;
    c = d[배열의 번호].Length; //0 -> 2 | 1 -> 3 | 2 -> 4
    ```
 - 다차원 배열의 길이를 구할때 가변배열로 선언된 배열같은경우 [].Length를 하면 되지만 [,]이렇게 선언된 경우에는 GetLength(배열의 번호)를 사용하면 된다

- Format()메서드
    ```
    int num1 = 1;
    int num2 = 123;
    Console.WriteLine("num1 = {0} | {1} = {2}", num1, "num2", num2);  //출력문 => num1 = 1 | num2 = 2
    ```
  - 문자열뒤에 ,나눠진 변수들을 {번호} 번호 순으로 차례 차례 들어간다 | 뒤 변수에서 변수를 새로 만들어서 넣어도 된다
    출력포멧들을 하나의 문자열로 만들기 떄문에 불필요한 문자열 결합이 없다

- 문자열 보간
    ```
    int num = 1;
    Console.WriteLine($"num = {num}");  //출력문 => num = 1
    ```
 - C# 6.0 부터 사용가능한 문법 Format()방법과 달리 {}안에 직접 변수를 넣는다 (시간이 살짝 더 걸리는거 같다)

- StringBilder
    ```
    int num = int.Parse(Console.ReadLine());
    int sum = 0;
    StringBuilder bulider = new StringBuilder();
    for (int i = 0; i < num; i++)
    {
      string[] str = Console.ReadLine().Split();
      bulider.Append(int.Parse(str[0]) + int.Parse(str[1]) + "\n");
    }
    Console.WriteLine(bulider.ToString());
    ```
 - String은 문자열 조합시마다 새롭게 만드는 변수를 추가하는것이 아닌 재창조하는것으로 메모리 소비가 높아진다
   stringBuilder는 문자열을 생성할때 뒤에 붙어주는 방식으로 자체적으로 함수를 가지고 있어 조합하거나 삭제할때 메모리를 추가로 사용하지 않는다
   설계시 문자열을 입력하고 변경횟수를 알 수 없거나 많은수의 변경이 예상되면 stringbuilder를 사용하고 그렇지 않고 문자수가 적거나 문자열 작성간 광범위한 검색이 이루어진다면 string을 쓰자
   (모아두었던 string을 Cosole.Write()에서 출력하는 방식이다)

## 어려웠던 알고리즘

- 색종이
  ```
  nt arr = int.Parse(Console.ReadLine());
  int area = 0;
  int[,] pan = new int[100,100];
  int x, y;

  for(int i = 0; i < arr; i++)
  {
      string[] str = Console.ReadLine().Split();
      x = int.Parse(str[0]);
      y = int.Parse(str[1]);
      for(int j = 0; j < 10; j++)
      {
        for(int z = 0; z < 10; z++)
        {
          ㅔan[x + j, y + z] = 1;
        }
      }
  }

  for(int i = 0; i < pan.GetLength(0); i++)
  {
    for(int j = 0; j< pan.GetLength(1); j++)
    {
      if(pan[i,j] == 1)
      {
        area++;
      }
    }
  }
  Console.WriteLine(area);
  ```
 - 색종이 문제를 풀어내면서 문자열의 길이를 얻어내는 방법과 가변형 문자의 선언과 초기화 방법을 알게되었다
   크게 문제될건 없었지만 처음에 for문의 3개나 써야하는건가 싶어서 망설임이 있었지만 다른 방법이 떠오르지 않아 3중for문으로 작성했다

## 버퍼

- 버퍼를 사용하는 이유
 - 버퍼는 쇼핑카트와 같다고 생각하자 한개씩 옮기는거보다 한번에 닮아서 옮기면 편하잖아요~

- 출력 버퍼
 - 출력 버퍼내에 있는 데이터를 비운다는건 데이터를 전송한다는 의미이다

- 입력 버퍼
 - 입력 버퍼내에 있는 데이터를 비운다는건 출력버퍼와 달리 소멸

## TIL을 마치며

 - string에서 빠른 입출력을 알아봤을때는 신기했지만 만약 내가 프로그램을 작성할때 쓸지는 미지수 인거같다
   다른사람들과 팀프로젝트를 하거나 더 멀리봐서 직장에서 프로젝트를 진행할때 팀원들에게 물어보고 결정해봐야겠다.