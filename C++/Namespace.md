# [C++/MacOS] Namespace란? using namespace std가 뭘까?

c++ 코드를 짜보면 아래와 같은 코드를 많이 봤을 것이다. 
`using namespace std;` 
저 줄은 무엇이고 왜 사용하는 것일까?
- - -

## C++ Namespace란?
간단하게 __namespace__ 란 __이름 공간__ 이라고 생각하면 된다. 

보통 프로그램을 짤 때 여러 파일들이 생기기 마련이다.
그 파일들을 한꺼번에 컴파일하려고 할 때 같은 이름의 함수나 구조체가 존재한다면 컴퓨터는 이에 대한 오류를 낸다. 

같은 이름의 변수나 함수, 구조체가 있을 때 오류가 발생하는 것은 당연한 일이다. 이름 충돌이 일어났기 때문이다. 

Namespace는 이러한 이름충돌을 방지하기 위해서 만들어진 것이다. 

__Namespace는 변수, 함수, 구조체 등의 소속을 정해주는 것이다.__
__소속을 지정해줌으로써 같은 이름이어도 헷갈리지 않도록 도와주는 것이다.__

아래 코드를 봐보자.
```
#include <iostream>
using namespace std; //이거는 나중에 설명하도록 하겠다

int cal(int a, int b){     //더하는 함수
    int result = a + b;
    return result;
}

int cal(int a, int b){    //빼는 함수
    int result = a - b;
    return result;
}
int main(){
    int a = 5;
    int b = 3;
    
    int add = cal(a, b);
    int sub = cal(a, b);
    cout << sum << endl;
    cout << sub << endl;
}
```
cal이라는 함수 두개가 이름 충돌을 일으키고 있기 때문에 아래와 같은 오류가 뜬다. 
```
namespace.cpp:8:5: error: redefinition of 'cal'
int cal(int a, int b){
    ^
namespace.cpp:3:5: note: previous definition is here
int cal(int a, int b){
    ^
1 error generated.
```

- - -
## Namespace 사용법
__Namespace로 감싸기!__
Namespace는 함수뿐만 아니라 구조체, 변수 등 모두 사용할 수 있다. 

```
namespace 네임스페이스 이름 {
    함수, 구조체, 변수 등 선언
}
```

오류가 발생했던 두 개의 cal 함수를 namespace로 감싸면 이름 충돌을 해결할 수 있다.

```
namespace Add {
    int cal(int a, int b){     //더하는 함수
        int result = a + b;
        return result;
    }
}

namespace Sub {
    int cal(int a, int b){    //빼는 함수
        int result = a - b;
        return result;
    }
}
```

- - -
## Namespace 요소 접근법
1. 한정된 이름(qualified name)을 사용한 접근 
`Namespace_name::요소` 

감싸주었던 namespace 이름과 콜론 두개를 적어주고 호출하고 싶은 요소 이름을 이어 적어주면 된다. 
아래 코드처럼 말이다. 

- __::__ <- scope resolution operator
```
int main(){
    int a = 5;
    int b = 3;
    
    int add = Add::cal(a, b);
    int sub = Sub::cal(a, b);
    cout << sum << endl;
    cout << sub << endl;
}
```
이 방법은 코드가 길어질 수 있고 번거로울 수 있지만 명확하게 어떤 namespace에 들어가는지 한 눈에 볼 수 있다.

2. 전역 using 지시어 선언(declaration)
매번 namespace 이름을 적기 번거롭다면 이 방법을 쓰면 좋다. 이 방법을 사용함으로써 매번 namespace 이름을 적는 일을 줄 일 수 있다. 

`using Add::cal;` 

```
using Add::cal;

int main(){
    int a = 5;
    int b = 3;
    
    int add = cal(a, b);
    //int add = Add::cal(a, b);     //이 방법 또한 가능하다.
    int sub = Sub::cal(a, b);
    cout << add << endl;
    cout << sub << endl;
}
```
※ 주의해야 할 점은 Sub라는 namespace 안에 들어있는 cal 함수를 사용하고 싶다면 무조건 Sub::를 붙여서 호출해주어야 한다. 

 3. 전역 using 지시어 사용(directive)
 만약 특정 namespace 안에 있는 모든 함수나, 변수, 구조체를 namespace 이름을 적어주지 않고 자체 이름으로만 호출하고 싶다면 이 방법을 사용하면 된다. 

`using namespace 네임스페이스이름;` 

아래 코드를 봐보자.

```
#include <iostream>
using namespace std;

namespace MINMAX {
    int min(int a, int b){     //작은 수 반환
        if(a < b)
            return a;
        return b;
    }

    int max(int a, int b){    //큰 수 반환
        if(a > b)
            return a;
        return b;
    }
}

int main(){
    int a = 5;
    int b = 3;
    
    int min_num = MINMAX::min(a, b);
    int max_num = MINMAX::max(a, b);
    cout << "minimum number: " << min_num << endl;
    cout << "maximum number: " << max_num << endl;
}
```

min, max 함수는 각각 두 수 중 작거나 큰 수를 반환하는 간단한 함수이다. 
using namespace MINMAX; 를 해주면 MINMAX namespace 안에 들어있는 min, max 함수는 그 자체로 사용 가능하다. 

아래 코드 처럼말이다. 

```
using namespace MINMAX;

int main(){
    int a = 5;
    int b = 3;
    
    int min_num = min(a, b);
    int max_num = max(a, b);
    cout << "minimum number: " << min_num << endl;
    cout << "maximum number: " << max_num << endl;
}
```
minimum number: 3
maximum number: 5

4. 전역 namespace 접근 방법(global namespace)
지금까지 namespace를 접근하기 위해선 namespace이름::요소이름을 해주면 된다고 하였다.
그렇다면 글로벌로 같은 이름인 함수가 존재한다면 어떻게 글로벌요소를 호출할 수 있을까?

글로벌 요소를 호출하고 싶을 때는 두 가지 경우가 존재한다. 
- using namespace를 사용하지 않을 경우
- using namespace를 사용하는 경우

먼저 첫 경우를 보자. 
__<using namespace를 사용하지 않는 경우>__

```
#include <iostream>
using namespace std;

namespace PRINT {
    void print_sentence(){     
        cout << "이 함수는 PRINT라는 이름 영역 안에 있습니다." << endl;
    }
}

void print_sentence(){
    cout << "이 함수는 global로 존재합니다."  << endl;
}

int main(){
    int a = 5;
    
    print_sentence();   //우선순위 1 : 글로벌로 출력
    ::print_sentence(); //글로벌로 출력
    PRINT::print_sentence();    //namespace로 출력
    
    return 0;
}
```

이 함수는 global로 존재합니다.
이 함수는 global로 존재합니다.
이 함수는 PRINT라는 이름 영역 안에 있습니다.

위의 함수는 namespace 안에 존재하는 print함수와 글로벌로 존재하는 print함수를 어떻게 호출하는지 보여준다. 
이 경우 print_sentence 함수는 그 자체로 적었을 때 글로벌 함수먼저 우선순위를 가지게 된다. 

결국 이 경우는 아래와 같은 방법으로 글로벌 함수를 호출 할 수 있다. 

`요소; //print_sentence();
::요소; //::print_sentence();` 

__<using namespace를 사용하는 경우>__
그렇다면 using namespace를 사용하여 namespace 요소를 접근하는 경우는 결과가 어떨까?

예시부터 보자.

```
#include <iostream>
using namespace std;

namespace PRINT {
    void print_sentence(){
        cout << "이 함수는 PRINT라는 이름 영역 안에 있습니다." << endl;
    }
}

void print_sentence(){
    cout << "이 함수는 global로 존재합니다."  << endl;
}

using namespace PRINT;

int main(){
    int a = 5;
    
    //print_sentence();   //오류 발생
    ::print_sentence(); //글로벌로 출력
    PRINT::print_sentence();    //namespace로 출력
    
    return 0;
}
```
이 함수는 global로 존재합니다.
이 함수는 PRINT라는 이름 영역 안에 있습니다.

PRINT namespace를 using을 사용하여 그냥 접근할 수 있도록 했음에도 불구하고 그냥 함수 이름만 적을 경우에 오류가 발생한다. 
이는 전역 print_sentence함수인지 PRINT::print_sentence함수인지 명확하지 않기 때문이다. 

그래서 이 경우는 아래와 같은 방법으로만 글로벌 요소를 호출할 수 있다. 

`::요소; //::print_sentence();`

- - -
## using namespace std; 는 무엇일까?
지금까지 봐온 내용을 잘 이해하였다면 std는 namespace 이름이라는 것을 알 수 있을 것이다. 

__c++ 은 모든 표준 요소들을 std라는 이름 공간에 넣어준다.__

그래서 using namespace std;를 사용해줌으로써 코드를 더 간단하게 작성할 수 있는 것이다.

그 예로는 출력을 위한 cout 요소를 사용할 때 쉽게 볼 수 있다. 

```
//using namespace std;
std::cout << 5 << std::endl;

using namespace std;
cout << 5 << endl;
```

