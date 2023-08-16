# 내일배움캠프 2일차 TIL | C# 클래스

![image_720](https://github.com/KimMaYa1/NBC/assets/141565207/e84deae9-27a9-4728-a617-7bc512f9d10b)

<htr>https://github.com/shehdrbs123/spartaB10/tree/main/%EC%A1%B0%EB%B2%94%EC%A4%80

## 느낀점

 오늘은 2일차인데 3주차 과제를 듣다가 이해가 안가는 부분이 많아서 팀원들과 소통을 많이했어요
 그래서 팀원들이랑 많이 친해지고 다른조에도 가서 소통을 조금 해서 재미있는 하루였어요

## 클래스

 - class Name{}
    - 클래스 선언 하는건데 함수와 달리 클래스명뒤에 ()가 들어가지 않아
 - public Name(){}
    - 클래스안에 생성자야 위 같은 경우는 디폴트 생성자야
    - 클래스안에 생성자들은 매개변수를 다르게 받아서 생성할수도있어
 - [접근 제한자] [데이터 타입] [변수명] {get{} set{}}
    - 프로퍼티인데 다른 클래스에서 변수값을 가져갈때 get을 사용 다른 클래스에서 값을 지정할때 set을 사용해
        - 예시
        '''C#
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
        '''