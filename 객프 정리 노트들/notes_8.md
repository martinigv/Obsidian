# 11장

## 스트림

스트림(Stream)은 데이터의 흐름을 나타내는 소프트웨어 모듈로, 프로그램과 장치를 연결하여 데이터를 주고 받는 개념 스트림은 데이터를 보낸 순서대로 전달하며, 입출력의 기본 단위는 바이트이다

C++ 스트림 종류:

1. 입력 스트림: 입력 장치, 네트워크, 파일 등에서 데이터를 받아오는 스트림.

2. 출력 스트림: 프로그램에서 생성된 데이터를 출력 장치, 네트워크, 파일 등으로 전송하는 스트림.

**키 입력 스트림 버퍼:**

- 입력장치에서 받은 데이터를 일시 저장하며, 키 입력 중에 수정 가능.

- <Backspace> 키 입력 시 이전 키를 지우고, <Enter> 키 입력 시 프로그램이 버퍼에서 읽기 시작.

**스크린 출력 스트림 버퍼:**

- 프로그램에서 출력된 데이터를 임시 저장하며, 출력 장치를 효율적으로 사용.

- 버퍼가 꽉 차거나 강제 출력 명령 시에 출력 장치로 데이터를 전송.

### **스트림 입출력 방식(Stream I/O):**

- **특징:**
    
    - 스트림 버퍼를 이용한 입출력 방식.
    
    - 입력된 키는 버퍼에 저장되며, <Enter> 키 입력 시 프로그램이 버퍼에서 읽어가는 방식.
    
    - 출력되는 데이터는 일시적으로 스트림 버퍼에 저장되고, 버퍼가 꽉 차거나 특정 조건에서만 출력 장치로 전송됨.

### **저 수준 입출력 방식(Raw Level Console I/O):**

- **특징:**
    
    - 키가 입력되는 즉시 프로그램에게 키 값이 즉시 전달됨.
    
    - <Backspace> 키도 프로그램에게 바로 전달됨.
    
    - 게임 등 키 입력이 즉각적으로 필요한 상황에 사용됨.
    
    - 프로그램이 출력하는 즉시 출력 장치로 데이터가 전송됨.
    
    - 컴파일러나 라이브러리에 따라 구현이 다를 수 있음.
    

* 각각의 장단점은 ?

- **스트림 입출력 방식**
    
    - 입출력 동작을 효율적으로 관리할 수 있음.
    
    - 안정성
    
    - 표준화?
    
    - 데이터가 버퍼에 쌓여야 출력되므로 일부 상황에서는 딜레이가 발생 할지도?

- 저 수준 입출력 방식
    
    - 즉각적인 입출력 처리
    
    - 빠른 입력이 필요한 게임 같은 곳 에서는 유용할지도?
    
    - 버퍼 없이 바로 입출력 되므로 안정성 떨어짐
    
    - 표준화 부족
    
    - 호환성 문제?
    

11-1

```
#include <iostream>
using namespace std;

int main() {
	
	cout.put('H');    
	cout.put('i');     
	cout.put(33);      
	cout.put('\n');   


	cout.put('C').put('+').put('+').put(' '); 

	char str[] = "I love programming";
	cout.write(str, 6); 
}
```

- `**cout.put(char)**`은 단일 문자를 출력

- `**cout.put(int)**`은 ASCII 코드에 해당하는 문자를 출력

- `**cout.put('\n')**`은 개행 문자를 출력하여 다음 줄로 넘어감

- `**cout.write(buffer, count)**`는 버퍼에서 일정 개수의 문자를 출력

11-2

```
#include <iostream>
using namespace std;

void get1() {
    int ch;
    while ((ch = cin.get()) != EOF) {
        cout.put(ch);
        if (ch == '\n')
            break;
    }
}

void get2() {
    char ch;
    while (true) {
        cin.get(ch);
        if (cin.eof())
            break;
        cout.put(ch);
        if (ch == '\n')
            break;
    }
}

int main() {
    get1();
    get2();
}
```

get1

- `**cin.get()**`을 사용하여 입력을 받음

- `**<Enter>**` 키까지 입력을 받고, 입력된 문자들을 화면에 출력

- `**<Enter>**` 키가 입력되면 읽기를 중단

get2

- `**cin.get(char&)**`을 사용하여 입력을 받음

- `**<Enter>**` 키까지 입력을 받고, 입력된 문자들을 화면에 출력

- `**<Enter>**` 키가 입력되면 읽기를 중단

main

- `**get1()**`과 `**get2()**` 함수를 호출하여 두 가지 방법으로 입력을 받고 출력

11-3

```
#include <iostream>
#include <cstring>
using namespace std;

int main() {
    char cmd[80];
    
    cout << "cin.get(char*, int)로 문자열을 읽습니다." << endl;

    while (true) {
        cout << "종료하려면 exit을 입력하세요 >> ";
        cin.get(cmd, 80); 


        if (strcmp(cmd, "exit") == 0) {
            cout << "프로그램을 종료합니다....";
            return 0;
        } else {
            cin.ignore(1); 
        }
    }
}
```

`**cin.get(cmd, 80);**`: 사용자로부터 최대 79개의 영어 문자를 입력받아 `**cmd**` 배열에 저장

한글 문자는 38개 까지 읽을 수 있음. ‘\n'은 입력 스트림 버퍼에 남겨둠.

`**else cin.ignore(1);**`: "exit"이 아닌 경우, `**cin.ignore(1)**`을 사용하여 버퍼에 남아 있는 <Enter> 키('\n')를 제거

**왜** `**cin.get()**`**이 아닌** `**cin.getline()**`**을 사용하지 않았을까?**

- `**cin.getline()**`은 개행 문자를 만날 때까지 문자를 받아오는 함수로, 한 줄을 읽기에 더 적합

- 하지만 여기서는 개행 문자가 아닌 "exit"이 입력되기를 기다리고 있어 `**cin.get()**`을 사용한게 아닐까

11-4

```
#include <iostream>
using namespace std;

int main() {
	char line[80];
	cout << "cin.getline() 함수로 라인을 읽습니다." << endl;
	cout << "exit를 입력하면 루프가 끝납니다." << endl;

	int no = 1; 
	while(true) {
		cout << "라인 " << no << " >> ";
		cin.getline(line, 80); 
		if(strcmp(line, "exit") == 0)
			break;
		cout << "echo --> ";;		
		cout << line << endl; 
		no++; 
	}
}
```

`getline()`을 이용하여 빈 칸을 포함하는 한 줄을 읽고 다시 그대로 출력

입력 스트림에서 문자 건너뛰기

```
cin.ignore(10); // 입력 스트림에 입력된 문자 중 10개 제거

cin.ignore(10, ';'); // 입력 스트림에서 10개의 문자 제거. 제거 도중 ';'을 만나면 종료
```

최근에 읽은 문자 개수 리턴

```
char line[80];
cin.getline(line, 80); 
int n = cin.gcount(); // 최근의 실행한 getline() 함수에서 읽은 문자의 개수 리턴
```

포맷 플러그

- 입출력 스트림에서 입출력 형식을 지정하기 위한 플래그

세팅하는 멤버 함수

```
cout.unsetf(ios::dec); // 10진수 해제
cout.setf(ios::hex); // 16진수로 설정
cout << 30 << endl; // 1e가 출력됨
```

```
cout.setf(ios::dec | ios::showpoint); // 10진수 표현과 동시에 실수에 
			 // 소숫점이하 나머지는 0으로 출력
cout << 23.5 << endl; // 23.5000으로 출력
```

11-5

```
#include <iostream>
using namespace std;

int main() {
	cout << 30 << endl; // 10진수로 출력

	cout.unsetf(ios::dec); // 10진수 해제
	cout.setf(ios::hex); // 16진수로 설정
	cout << 30 << endl;

	cout.setf(ios::showbase); // 16진수로 설정
	cout << 30 << endl;

	cout.setf(ios::uppercase); // 16진수의 A~F는 대문자로 출력
	cout << 30 << endl;

	cout.setf(ios::dec | ios::showpoint); // 10진수 표현과 동시에 
														// 소숫점 이하 나머지는 0으로 출력
	cout << 23.5 << endl;

	cout.setf(ios::scientific); // 실수를 과학산술용 표현으로 출력
	cout << 23.5 << endl;

	cout.setf(ios::showpos); // 양수인 경우 + 부호도 함께 출력
	cout << 23.5; 
}
```

## width(), fill(), precision() 너비설정, 빈칸채우기, 유효숫자자리수

```
//너비설정
cout.width(10); // 다음에 출력되는 "hello"를 10 칸으로 지정
cout << "Hello" << endl;
cout.width(5); // 다음에 출력되는 정수 12를 5 칸으로 지정
cout << 12 << endl;
```

```
cout << '%';
cout.width(10); // 다음에 출력되는 "Korea/"만 10 칸으로 지정
cout << "Korea/" << "Seoul/" << "City" <<endl;
```

```
	//빈칸채우기
cout.fill('^');
cout.width(10);
cout << "Hello" << endl;
```

```
//유효숫자자리수
cout.precision(5);
cout << 11./3.;
```

11-6

```
#include <iostream>
using namespace std;

void showWidth() {
	cout.width(10); // 다음에 출력되는 "hello"를 10 칸으로 지정
	cout << "Hello" << endl;
	cout.width(5); // 다음에 출력되는 정수 12를 5 칸으로 지정
	cout << 12 << endl;

	cout << '%';
	cout.width(10); // 다음에 출력되는 "Korea/"만 10 칸으로 지정
	cout << "Korea/" << "Seoul/" << "City" <<endl;
}

int main() {
	showWidth(); 
	cout << endl;

	cout.fill('^'); 	
	showWidth();
	cout << endl;

	cout.precision(5); 
	cout << 11./3. << endl;
}
```

## ****조작자(Manipulator)****

- C++에서 스트림 입출력을 제어하는 함수로, `**<<**` 나 `**>>**` 연산자와 함께 사용

**조작자의 종류:**

- **매개 없는 조작자:** 매개 없이 사용되며, 출력 형식을 변경하거나 스트림 상태를 조절

- **매개 있는 조작자:** `**#include <iomanip>**` 필요하며, 특정한 작업을 위해 매개 변수를 사용

```
//매개 변수 없는 조작자
cout << hex << showbase << 30 << endl;
cout << dec << showpos << 100 << endl;

/* 결과 
0x1e
+100
*/
```

```
//매개 변수 있는 조작자
cout << setw(10) << setfill('^') << "Hello" << endl;

/* 결과
^^^^^Hello
*/
```

11-7

```
#include <iostream>
using namespace std;

int main() {
	cout << hex << showbase << 30 << endl;
	cout << dec << showpos << 100 << endl;
	cout << true << ' ' << false << endl;
	cout << boolalpha << true << ' ' << false << endl;
}
```

- `**cout << hex << showbase << 30 << endl;**`은 16진수로 출력하고, `**showbase**`를 사용하여 0x를 표시합니다. 결과는 "0x1e"가 됨

- `**cout << dec << showpos << 100 << endl;**`은 10진수로 출력하고, `**showpos**`를 사용하여 양수의 경우 부호를 표시합니다. 결과는 "+100"이 됨

- `**cout << true << ' ' << false << endl;**`는 `**true**`와 `**false**`를 숫자로 출력하고, 결과는 "1 0"

- `**cout << boolalpha << true << ' ' << false << endl;**`는 `**boolalpha**`를 사용하여 불리언 값을 문자열로 출력하고, 결과는 "true false"가 됨

11-8

```
#include <iostream>
#include <iomanip>
using namespace std;

int main() {
	cout << showbase;
	
	cout << setw(8) << "Number";
	cout << setw(10) << "Octal";
	cout << setw(10) << "Hexa" << endl;


	for(int i=0; i<50; i+=5) { 
		cout << setw(8) << setfill('.') << dec << i; 
		cout << setw(10) << setfill(' ') << oct << i; 
		cout << setw(10) << setfill(' ') << hex << i << endl; 
	}
}
```

- `**cout << setw(8) << "Number";**`과 같이 `**setw**` 함수를 사용하여 출력 폭을 지정하고, `**setfill**` 함수로 채울 문자를 지정

- `**for**` 루프를 통해 0부터 50까지 5씩 증가하면서 각각의 수를 십진수, 8진수, 16진수 형태로 출력

- `**setw**`, `**setfill**` 함수를 사용하여 출력 폭과 채울 문자를 지정

사용자 삽입 연산자 만들기

- 사용자 삽입 연산자는 주로 클래스의 객체를 출력할 때 특별한 형식으로 출력하고자 할 때 사용

- 이 연산자는 클래스 외부에서 정의되며, 객체의 출력을 담당

```
class Point {
	int x, y;
public:	
	Point(int x=0, int y=0) { this->x = x; this->y = y; }
};

Point p(3,4);
cout << p;


//결과 
//(3,4)
```

11-9

```
#include <iostream>
using namespace std;

class Point { 
	int x, y;
public:	
	Point(int x=0, int y=0) {
		this->x = x; 
		this->y = y;
	}
	friend ostream& operator << (ostream& stream, Point a); 
};

// << 연산자 함수
ostream& operator << (ostream& stream, Point a) {
	stream << "(" << a.x << "," << a.y << ")";
	return stream;
}

int main() {
	Point p(3,4);
	cout << p << endl;

	Point q(1,100), r(2,200);
	cout << q << r < endl;
}
```

- `**Point**` 클래스는 x, y 좌표를 가지며, 생성자를 통해 초기화

- `**ostream& operator<<(ostream& stream, Point a);**`는 `**Point**` 객체를 출력하기 위한 `**<<**` 연산자 함수. 이 함수는 `**Point**` 클래스에 `**friend**`로 선언되어 있으므로 `**Point**`의 private 멤버에 접근할 수 있음

- `**main**` 함수에서는 `**Point**` 객체를 생성하고, `**cout**`을 통해 출력함. 첫 번째 출력은 `**(3,4)**`가 될 것이며, 두 번째 출력은 `**(1,100)**`과 `**(2,200)**`이 연속으로 출력

11-10

```
#include <iostream>
#include <string>
using namespace std;

class Book { 스
	string title; 
	string press;
	string author; 
public:	
	Book(string title="", string press="", string author="") {
		this->title = title;
		this->press = press;
		this->author = author;
	}
	friend ostream& operator << (ostream& stream, Book b); 
};

// << 연산자 함수
ostream& operator << (ostream& stream, Book b) {
	stream << b.title << "," << b.press << "," << b.author;
	return stream;
}

int main() {
	Book book("소유냐 존재냐", "한국출판사", "에리히프롬");
	cout << book; 
}
```

- `**Book**` 클래스는 책을 표현하는 클래스로서 `**title**`, `**press**`, `**author**` 세 가지 멤버를 가지고 있음

- `**Book**` 클래스의 생성자는 기본값을 갖는 매개변수를 사용하여 초기화될 수 있음

- `**operator<<**` 함수는 `**Book**` 클래스의 friend 함수로 선언되어 있음. 이 함수는 `**ostream**` 객체와 `**Book**` 객체를 받아 스트림에 적절한 형식으로 출력

- `**main**` 함수에서는 `**Book**` 클래스의 객체 `**book**`을 생성하고, `**cout**`을 통해 사용자 삽입 연산자를 호출하여 객체를 출력

사용자 추출 연산자 만들기

```
class Point {
	int x, y;
public:	
	Point(int x=0, int y=0) { this->x = x; this->y = y; }
};

Point p;
cin >> p; 
cout << p;

//결과
//x 좌표>>100
//y 좌표>>200
(100,200)
```

11-11

```
#include <iostream>
using namespace std;

class Point { // 한 점을 표현하는 클래스
	int x, y; // private 멤버
public:	
	Point(int x=0, int y=0) {
		this->x = x; 
		this->y = y;
	}
	friend istream& operator >> (istream& ins, Point &a); // friend 선언
	friend ostream& operator << (ostream& stream, Point a); // friend 선언
};

istream& operator >> (istream& ins, Point &a) { // >> 연산자 함수
	cout << "x 좌표>>";
	ins >> a.x;
	cout << "y 좌표>>";
	ins >> a.y;
	return ins;
}

ostream& operator << (ostream& stream, Point a) { // << 연산자 함수
	stream << "(" << a.x << "," << a.y << ")";
	return stream;
}

int main() {
	Point p; // Point 객체 생성
	cin >> p;  // >> 연산자 호출하여 x 좌표와 y 좌표를 키보드로 읽어 객체 p 완성
	cout << p;  // << 연산자 호출하여 객체 p 출력
}
```

`**friend**` 선언은 해당 함수가 클래스의 private 멤버에 접근할 수 있도록 하는 역할

`**main**` 함수에서 `**cin >> p;**`를 통해 `**>>**` 연산자가 호출되어 키보드로부터 `**Point**` 객체 `**p**`의 좌표값을 입력받고 그 후 `**cout << p;**`를 통해 `**<<**` 연산자가 호출되어 객체 `**p**`의 좌표를 출력

사용자 조작자

11-12

```
#include <iostream>
using namespace std;

ostream& fivestar(ostream& outs) {
	return outs << "*****";
}

ostream& rightarrow(ostream& outs) {
	return outs << "---->";
}

ostream& beep(ostream& outs) {
	return outs << '\a'; // 시스템 beep(삑 소리) 발생
}

int main() {
	cout << "벨이 울립니다" << beep << endl;
	cout << "C" << rightarrow << "C++" << rightarrow << "Java" << endl;
	cout << "Visual" << fivestar << "C++" << endl;	
}
```

- 사용자 정의 조작자를 활용하여 특정한 출력 동작을 추가하는 예제

- `**fivestar**`: "*****"를 출력

- `**rightarrow**`: "---->"을 출력

- `**beep**`: 벨 소리를 발생시켜 시스템 beep을 실행

11-13

```
#include <iostream>
#include <string>
using namespace std;

istream& question(istream& ins) {
	cout << "거울아 거울아 누가 제일 예쁘니?";
	return ins;
}

int main() {
	string answer;
	cin >> question >> answer;
	cout << "세상에서 제일 예쁜 사람은 " << answer << "입니다." << endl;
}
```

- 사용자 정의 조작자 `**question**`을 활용하여 질문을 출력하고, 사용자로부터 입력을 받아 출력하는 예제

- `**question**`은 단순히 질문을 출력하는 역할

- `**main**` 함수에서는 `**cin**`과 함께 `**question**`을 사용하여 사용자에게 질문을 하고, 그에 대한 답을 `**answer**`에 저장한 후 출력