# 내일배움캠프 8일차 TIL | C# 알고리즘 풀이 (풀이중 알아낸 정보)

![image_720](https://github.com/KimMaYa1/NBC/assets/141565207/e84deae9-27a9-4728-a617-7bc512f9d10b)

<htr>https://github.com/KimMaYa1/BaekJoonEx

## 느낀점

- 

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

- 문자열 보간
    ```
    int num = 1;
    Console.WriteLine($"num = {num}");  //출력문 => num = 1
    ```
  - C# 6.0 부터 사용가능한 문법 Format()방법과 달리 {}안에 직접 변수를 넣는다 (시간이 살짝 더 걸리는거 같다)

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

## TIL을 마치며

 - 