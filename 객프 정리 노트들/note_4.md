# 07 프렌드와 연산자 중복
## 프렌드 함수
1. 클래스의 멤버 함수가 아닌 외부 함수
2. 프렌드 키워드로 클래스 내에 선언된 함수
**클래스의 모든 맴버를 접근할 수 있게 됨** <- 주된 이유

### 프렌드 함수의 3가지 형태
* 전역 함수
```c++
class Rect {
...
	friend bool equals(Rect r, Rect s);
}
```
* 다른 클래스의 멤버 함수
```c++
class Rect {
	friend bool RectManager::equals(Rect r, Rect s);
}
```
* 다른 클래스 전체
```c++
class Rect {
	friend RectManager;
}
```

## slide07

### 요약
외부 함수 equals()를 Rect 클래스 안에서 friend 함수로 선언하여 equals() 에서 Rect의 private 변수에 접근할 수 있게 된다. 
### code
```c++
#include <iostream>
using namespace std;
class Rect;
bool equals(Rect r, Rect s); // equals() 함수 선언
class Rect { // Rect 클래스 선언
	int width, height;
public:
	Rect(int width, int height) { this->width = width; this->height = height; }
	friend bool equals(Rect r, Rect s);
};
bool equals(Rect r, Rect s) { // 외부 함수
	if (r.width == s.width && r.height == s.height) return true;
	else return false;
}
int main() {
	Rect a(3, 4), b(4, 5);
	if (equals(a, b)) cout << "equal" << endl;
	else cout << "not equal" << endl;
}
```
## slide09
### 요약
다른 클래스 RectManager 전체를 friend 함수로 Rect 클래스에 선언하여 RectManager의 멤버 함수에서 Rect의 private 멤버 변수에 접근이 가능하다.
### code
```c++
#include <iostream>
using namespace std;
class Rect;
class RectManager { // RectManager 클래스 선언
public:
	bool equals(Rect r, Rect s);
	void copy(Rect& dest, Rect& src);
};
class Rect { // Rect 클래스 선언
	int width, height;
public:
	Rect(int width, int height) { this->width = width; this->height = height; }
	friend RectManager;
};
bool RectManager::equals(Rect r, Rect s) { // r과 s가 같으면 true 리턴
	if (r.width == s.width && r.height == s.height) return true;
	else return false;
}
void RectManager::copy(Rect& dest, Rect& src) { // src를 dest에 복사
	dest.width = src.width; dest.height = src.height;
}
int main() {
	Rect a(3, 4), b(5, 6);
	RectManager man;

	man.copy(b, a);
	if (man.equals(a, b))
		cout << "equal" << endl;
	else
		cout << "not equal" << endl;
}
```

## 연산자 중복

### 요약 (+ 비유)
빨강 + 파랑 = 보라
2 + 3 = 5
**같은 +여도 다른 역할을 한다**
-> 프로그래밍 언어에서는 어떻게 구현할까?
클래스 멤버 함수로 **리턴 타입 operater 연산자 (매개변수)** 
코드로 예시를 살펴보자
## slide17

### 요약
컴파일러가  a + b => a.+(b)로 해석 -> operater+() 정의대로 계산된다.
### code
```c++
#include <iostream>
using namespace std;
class Power {
	int kick;
	int punch;
public:
	Power(int kick = 0, int punch = 0) {
		this->kick = kick; this->punch = punch;
	}
	void show();
	Power operator+ (Power op2); // + 연산자 함수 선언
};
void Power::show() {
	cout << "kick=" << kick << ',' << "punch=" << punch << endl;
}
Power Power::operator+(Power op2) {
	Power tmp; // 임시 객체 생성
	tmp.kick = this->kick + op2.kick; // kick 더하기
	tmp.punch = this->punch + op2.punch; // punch 더하기
	return tmp; // 더한 결과 리턴
}
int main() {
	Power a(3, 5), b(4, 6), c;
	c = a + b; // 파워 객체 + 연산
	a.show();
	b.show();
	c.show();
}
```

## slide25

### 요약
전위 연산자 함수를 멤버 함수로 구현하여 kick과 punch를 증감시킨다.
*주의사항*
```
Power& Power::operater++() {
	return *this
}
```
this -> 객체를 가리키는 포인터이므로

```
*this 
```
는 객체의 주소를 가리킴
리턴해야 할 타입이 Power& 이므로 this (x)  * this(o)
### code
```c++
#include <iostream>
using namespace std;
class Power {
	int kick;
	int punch;
public:
	Power(int kick = 0, int punch = 0) {
		this->kick = kick; this->punch = punch;
	}
	void show();
	Power& operator++ (); // 전위 ++ 연산자 함수 선언
};
void Power::show() {
	cout << "kick=" << kick << ',' << "punch=" << punch << endl;
}
Power& Power::operator++() {
	kick++;
	punch++;
	return *this; // 변경된 객체 자신(객체 a)의 참조 리턴
}
int main() {
	Power a(3, 5), b;
	a.show();
	b.show();
	b = ++a; // 전위 ++ 연산자 사용
	a.show();
	b.show();
}

```

## slide 30

### 요약

### code
```c++
#include <iostream>
using namespace std;
class Power {
	int kick;
	int punch;
public:
	Power(int kick = 0, int punch = 0) {
		this->kick = kick; this->punch = punch;
	}
	void show();
	friend Power operator+(int op1, Power op2); // 프렌드 선언
};
void Power::show() {
	cout << "kick=" << kick << ',' << "punch=" << punch << endl;
}
Power operator+(int op1, Power op2) {
	Power tmp; // 임시 객체 생성
	tmp.kick = op1 + op2.kick; // kick 더하기
	tmp.punch = op1 + op2.punch; // punch 더하기
	return tmp; // 임시 객체 리턴
}
int main() {
	Power a(3, 5), b;
	a.show();
	b.show();
	b = 2 + a; // 파워 객체 더하기 연산
	a.show();
	b.show();
}
```


