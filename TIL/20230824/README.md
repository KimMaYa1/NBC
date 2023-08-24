# 내일배움캠프 6일차 TIL | C# 알고리즘풀이

![image_720](https://github.com/KimMaYa1/NBC/assets/141565207/e84deae9-27a9-4728-a617-7bc512f9d10b)

<htr>https://github.com/KimMaYa1/BaekJoonEx

## 느낀점

- 오늘은 알고리즘을 풀어내면서 문제를 풀어나가는 법에 대해서 좀더 깊게 배웠습니다
  오늘 푸는 문제중에 어려웠던 부분들은 없지만 정확하게 지정해주지 않은 부분들에서 오류가 많이 나와
  앞으로 코드를 짤때 좀더 세심히 짜야겠다고 느껴졌다.

## 어려웠던 알고리즘

- 문자열을 정수로 바꾸기
    ```
    public class Solution {
        public int solution(string s) {
            int answer = 0;
            string str = s.Substring(0,1);      //s문자열에 0번째부터 길이 1만큼 str에 넣어줍니다
        
            if(int.TryParse(str, out int x))    //첫 문자가 숫자라면
            {
                str = s.Substring(0, s.Length);     //s문자열에 0번째부터 길이 s문자열만큼 str에 저장
                answer = int.Parse(str);            //str을 변환해 answer에 숫자로 넣어준다
            }
            else                                //첫 문자가 숫자가 아니라면
            {
                str = s.Substring(1, s.Length-1);   //s문자열에 1번째부터 길이 s문자열 - 1 만큼 str에 저장
                answer = int.Parse(str);        //str을 변환해 answer에 숫자로 넣어준다
                if(s[0] == '-')
                {
                    answer = answer * -1;       //마이너스 값을 위한 -1을 곱해준다
                }
            }
        
            return answer;
        }
    }
    ```
 - 문자열 첫번째에는 부호가 들어갈수있는데 그냥 Parse를 써버리면 바꿀수있지만 좀더 길게 늘여서 써보니 좀 어려웠다
   함수가 아닌 그냥 짜보려고하니 좀더 생각할게 많았지만 좀더 깊게 알아볼수있어서 괜찮았던거 같다

- 명령 프롬프트
    ```
    internal class Program
    {
        static void Main(string[] args)
        {
            int arr = int.Parse(Console.ReadLine());        //몇개를 적을지 결정
            string[] str = new string[arr];                 //아래에서 적어낸걸 저장할 배열
            string abc = "";                                //출력될 문자열
            for (int i = 0; i < arr; i++)
            {
                string s = ""; //abc에 넣을 문자열을 안에서 선언 for문을 다시 돌때 초기화시키기 위해 안에서 선언
                str[i] = Console.ReadLine();
                if (i > 0)  //처음적는게 아니라면
                {
                    for (int j = 0; j < str[i].Length; j++) //적은 문자열 길이만큼 반복
                    {
                        if (abc[j] != str[i][j])    //최종 출력될 문자열[j]번째와 입력한 문자열[j]번째가 다르다면
                            s += "?";               //s뒷부분에 ?를 추가해준다
                        else                        //최종 출력될 문자열[j]번째와 입력한 문자열[j]번째가 같다면
                            s += str[i][j];         //s뒷부분에 입력한 문자열[j]번째를 추가해준다
                    }
                    abc = s;                        //abc에 s를 넣어준다
                }
                else       //처음 적는거라면
                {
                    abc = str[0];       //배열 0번에 처음 적은 문자열을 넣어준다
                }
            }
            Console.WriteLine(abc);
        }
    }
    ```
 - 문자열도 문자들의 배열이기 때문에 사용했는데 처음에는 Char인지 망각하고 사용하여 오류가 많이 나서 애먹었습니다

 ## TIL을 마치며

 - 알고리즘을 계속 풀다보니 어려웠던 부분을 풀때 좌절도 좀 하지만 방법을 찾아내면서(구글링, 주변 조원들에게 질문) 몰랐던 부분들을 알아가게 되서
   뜻깊은 날이었던거 같습니다
   남은 알고리즘도 많이 풀어내어 생각하는 개발자가 되기 위해 노력하고 싶네요 핳