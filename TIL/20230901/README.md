# 내일배움캠프 13일차 TIL | C# 팀과제 - 콘솔꾸미기

![image_720](https://github.com/KimMaYa1/NBC/assets/141565207/e84deae9-27a9-4728-a617-7bc512f9d10b)

<htr>https://github.com/KimMaYa1/SPT-B12

## 팀과제

 - 9월 1일자로 팀프로젝트 제출 자체는 끝났지만 이후 프로젝트에서 어떤 방향으로 프로젝트를 진행하면 좋을지 서로 얘기를 많이 나눠보았습니다.


## 파일 경로 탐색

- 오늘까지 팀프로젝트를 진행하면서 크게 어려움은 없었지만 잘 몰랐던 파일 경로 읽어오는 법과 데이터 읽어오는 법을 몰라 공부를 하였습니다

### @의 의미

  ```
  string path1 = "C:\\Program Files\\Microsoft Office\\Office16\\EXCEL.EXE";
  string path2 = @"C:\Program Files\Microsoft Office\Office16\EXCEL.EXE";
  string sql = @"SELECT 
                  Seq
                  , Name 
                  , Flavor 
                  , Rank 
               FROM Fruit "
  ```
 - \ (이스케이프 시퀀스)를 @없이 사용한다면 \한번 더 눌러야 단일문자로 간주하지만 @를 사용한다면 이스케이프 시퀀스를 간주하지않고 바로 단일 문자로 간주하며 쿼리문 작성시에 문자열값을 여러줄 넣을때 +를 써야하는데 @를 사용하면 +를 사용하지 않고도 여러줄 표현이 가능하다

### 절대 파일 경로
  
  ```
  string path = @"C:\Users\82104\Desktop\Sparta\week1\ConsoleApp1\BaekJoonProject\BaekJoonProject\bin\Debug\net6.0\BaekJoonProject.exe";
  string directoryName = Path.GetDirectoryName(path);
  Console.WriteLine(directoryName);
  ```
 - 마지막 \뒤를 무시하고 반환해서 출력은 C:\Users\82104\Desktop\Sparta\week1\ConsoleApp1\BaekJoonProject\BaekJoonProject\bin\Debug\net6.0이렇게 됩니다

### 상대 경로로 절대 파일 경로 구하기

  ```
  string fullPath = Path.GetFullPath(@"..\net6.0");
  ```
 - 위에 절대 파일 경로 찾기처럼 똑같은 값이 fullpath에 저장이 됩니다.
 - 만약 만약 뒤에 \net6.0이 필요하지 않다면 @를 빼고 .. 만 넣어도 자신의 경로를 구할수있다

### 현재 파일 경로

  ```
  string a = Directory.GetCurrentDirectory().ToString();
  ```
 - 현재 실행되고있는 프로그램의 위치가 어디있는지 반환해줍니다.

### 경로를 구성하는 방법

  ```
  string localPath = "..";
  string dataPath = @"\Data";
  string fileName = @"MonsterData.csv";
  string fullPath = Path.Combine(localPath, dataPath, fileName);
  ```
 - 위에 상대경로로 절대 파일 경로 구하기로 폴더위치를 가져오고 Data 폴더안에 MonsterData.csv를 fullPath에 차곡차곡 넣어두는 형식이다. 출력은 아마 
 C:\Users\82104\Desktop\Sparta\week1\ConsoleApp1\BaekJoonProject\BaekJoonProject\bin\Debug\Data\MonsterData.csv
 이런 형식으로 될거같다

## 또 배우고 싶은것

- 오늘 파일 경로를 읽어오는것을 배워서 다음에는 그 파일 경로를 이용해 파일의 정보를 읽어와서 안에 있는 데이타를 출력하고 싶네요 그럼 다음에 봐용 안녕!