# 내일배움캠프 2일차 TIL | C# 클래스와 객체, 한글사용시 오류!

![image_720](https://github.com/KimMaYa1/NBC/assets/141565207/e84deae9-27a9-4728-a617-7bc512f9d10b)

<htr>https://github.com/shehdrbs123/spartaB10/blob/main/%EC%A1%B0%EB%B2%94%EC%A4%80/week3/Program.cs

## 느낀점

 오늘은 2일차인데 3주차 과제를 듣다가 이해가 안가는 부분이 많아서 팀원들과 소통을 많이했어요
 그래서 팀원들이랑 많이 친해지고 다른조에도 가서 소통을 조금 해서 재미있는 하루였어요

## 클래스/객체

 - class Name{}
    - 클래스 선언 하는건데 함수와 달리 클래스명뒤에 ()가 들어가지 않아
 - public Name(){}
    - 클래스안에 생성자야 위 같은 경우는 디폴트 생성자야
    - 클래스안에 생성자들은 매개변수를 다르게 받아서 생성할수도있어
 - [접근 제한자] [데이터 타입] [변수명] {get{} set{}}
    - 프로퍼티인데 다른 클래스에서 변수값을 가져갈때 get을 사용 다른 클래스에서 값을 지정할때 set을 사용해
        - 예시
        ```
        private string name;
        private int age;

        public string Name
        {
            get { return name; }
            set { name = value; }
        }

        public int Age
        {
            get { return age; }
            set { age = value; }
        }
        

## 이외 여러가지

- Console.SetCursorPosition(int x, int y)
    - 콘솔에 x,y좌표로 커서를 옮겨 줘서 그뒤에 코드 작성이 가능하다

- enum 예시
```
public enum Direction
{
    LEFT,
    RIGHT,
    UP,
    DOWN
}
```
새로운 상수 집합을 만들어준다


## 과제 풀이시 힘들었던 점 (오류)

- 3주차 숙제 스네이크 문제를 풀던 도중 음식을 먹어도 처리가 안되는 현상이 있었다
  아래는 음식이 안먹어지는 코드중 일부이다

  ```
  int foodCount = 0;
            int mapx = 80, mapy = 20;
            int gameSpeed = 200; 
            DrawWalls(mapx, mapy, foodCount);
            Point p = new Point(4, 5, '*');
            Snake snake = new Snake(p, 4, Direction.RIGHT);

            FoodCreator foodCreator = new FoodCreator(mapx, mapy, 'ㅁ');
            Point food = foodCreator.Create();
            food.Draw();

            snake.Draw();
  ```

  위 코드처럼 사용하니 음식 (ㅁ)을 먹어도 처리가 안되는 현상이 있었다
  위 코드를 아래처럼 바꿔 보았다

  ```
  int foodCount = 0;
            int mapx = 80, mapy = 20;
            int gameSpeed = 200; 
            DrawWalls(mapx, mapy, foodCount);
            Point p = new Point(4, 5, '*');
            Snake snake = new Snake(p, 4, Direction.RIGHT);

            FoodCreator foodCreator = new FoodCreator(mapx, mapy, '$');
            Point food = foodCreator.Create();
            food.Draw();

            snake.Draw();
  ```

 바뀐점을 한번에 찾기 어려울수 있지만 잘보면 FoodCreator foodCreator = new FoodCreator(mapx, mapy, '$');
 이 부분이 'ㅁ' 에서 '$' 로 바꾸었다 이렇게 사용하니 음식이 정상적으로 먹어졌는데
 이유는 ㅁ같은 한글은 2바이트를 잡고 있고 특수문자 혹은 영어 등은 1바이트를 잡아먹는다 그래서
 1바이트 특수문자인 '*'가 'ㅁ'위를 지나가도 나머지 한바이트에 ㅁ이 남아있어 콘솔에 남게되는 불상사가 발생했다
 이 작은 선택하나가 내 코드를 멍청이로 만들어서 많이 힘들었지만 잘 해결해서 다행인거 같다
 그리고 

```
 if (snake.Eat(food))
{
    foodCount++;

    food = foodCreator.Create();
    food.Draw();
    if (gameSpeed > 10)
    {
        gameSpeed -= 10;
    }
}
```

이 부분에서도 문제가 있었는데 음식을 새로 생성하기전에 음식그림을 다시 그려주지 않으니 다른 함수에서 모양을 바꿧어도
'$'그대로 있었다 이부분을 아래처럼

```
 if (snake.Eat(food))
{
    foodCount++;
    food.Draw();   
    food = foodCreator.Create();
    food.Draw();
    if (gameSpeed > 10)
    {
        gameSpeed -= 10;
    }
}
```

다시 그려주는 작업을 가지닌 정상적으로 '*'로 바뀌어 스네이크 코드가 정상 작동 하였다

## TIL을 마치며

 - 앞으로는 코드 작성에 좀더 심혈을 기울여야겠다 작은 선택 하나가 코드를 전부 못쓰게 만들어버리니 말이다
   그리고 남은날도 화이팅이다!