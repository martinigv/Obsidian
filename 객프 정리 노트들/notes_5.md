# 상속
## 업 캐스팅
자식 클래스 포인터가 부모 클래스 포인터에 치횐되는 것
형태: ex) Parent* p = c (Child* p , Parent* p)
자식 클래스가 부모 클래스를 포함하는 관계이므로 -> casting이 필요없음
비유: 람보르기니는 자동차의 특성을 모두 가지고 있음.
## 다운 캐스팅
부모 클래스 포인터가 자식 클래스 포인터에 치환되는 것
형태: ex) Child* c = (Child*) p
자식 클래스가 부모 클래스의 확장 관계 -> casting이 필요함

## 접근 지정자 protected

### 8-2

```c++
#include <iostream>
#include <string>
using namespace std;
class Point {
protected:
	int x, y; //한 점 (x,y) 좌표값
public:
	void set(int x, int y);
	void showPoint();
};
void Point::set(int x, int y) {
	this->x = x;
	this->y = y;
}
void Point::showPoint() {
	cout << "(" << x << "," << y << ")" << endl;
}
class ColorPoint : public Point {
	string color;
public:
	void setColor(string color);
	void showColorPoint();
	bool equals(ColorPoint p);
};
void ColorPoint::setColor(string color) {
	this->color = color;
}
void ColorPoint::showColorPoint() {
	cout << color << ":";
	showPoint(); // Point 클래스의 showPoint() 호출
}
bool ColorPoint::equals(ColorPoint p) {
	if (x == p.x && y == p.y && color == p.color) // ①
		return true;
	else
		return false;
}
int main() {
	Point p; // 기본 클래스의 객체 생성
	p.set(2, 3); // ②
	p.x = 5; // ③
	p.y = 5; // ④
	p.showPoint();
	ColorPoint cp; // 파생 클래스의 객체 생성
	cp.x = 10; // ⑤ ->오류 why? 객체는 class의 상속과 개념이 다르게 적용된다. -> 블랙박스에서 x,y를 꺼낼 수는 없다.
	cp.y = 10; // ⑥ -> 오류
	cp.set(3, 4);
	cp.setColor("Red");
	cp.showColorPoint();
	ColorPoint cp2;
	cp2.set(3, 4);
	cp2.setColor("Red");
	cout << ((cp.equals(cp2)) ? "true" : "false"); // ⑦
}
```

### 오류 : 3 4 5 6

#### 오류 3
protected로 선언된 맴버 변수들은 private과 마찬가지로 외부에서 직접 접근할 수가 없다.
* 그럼 보통 어떻게 맴버 변수들의 데이터 값을 들여다볼 수 있는가?
보통 클래스 맴버 함수에서 get()를 만들어서 값을 가져올 수도 있고 위 처럼 set()를 만들어서 값을 조정할 수도 있다.
#### 오류 4
오류 3번과 같은 이유

#### 오류 5
부모 클래스를 상속 받았다고 해도 자식 클래스 안에서 부모의 protected 변수들 접근이 가능한거지 객체에서 불러오는건 가능하지 않다.(클래스의 상속과 객체는 별개다)

#### 오류 6
오류 5번과 같은 이유

### 부모/자식 클래스 간의 생성자 호출 관계 
#### 질문 1
* 파생 클래스의 객체가 생성될 때 파생 클래스의 생성자와 기본 클래스의 생성자가 모두 실행이 되는가?
둘 다 실행이 된다.

#### 질문 2
* 파생 클래스의 생성자와 기본 클래스의 생성자 중 어떤 생성자가 먼저 실행되는가?
기본 클래스의 생성자 다음 파생 클래스의 생성자

#### 자식 클래스 생성자에서 부모 클래스 호출

```c++
class A { 
public: 
	A() { cout << "생성자 A" << endl; } 
	A(int x) { cout << "매개변수생성자 A" << x << endl; } 
};
```

```c++
class B : public A {
public:
	B() { cout << "생성자 B" << endl; }
};
```

```c++
int main() { B b; }
```
실행결과:
생성자 A
생성자 B
-> 컴파일러는 묵시적으로 부모 클래스의 기본 생성자를 호출하도록 컴파일한다

#### 부모 클래스에 기본 생성자가 없는 경우

```c++
class A { 
public: 
	 
	A(int x) { cout << "매개변수생성자 A" << x << endl; } 
};
```

```c++
class B : public A {
public:
	B() { cout << "생성자 B" << endl; }
};
```
컴파일 에러

이유: 부모 클래스의 기본 생성자를 호출했는데 매개변수 생성자만 존재하므로 컴파일 에러가 발생한다

#### 자식 클래스의 매개변수 생성자를 호출

```c++
class A { 
public: 
	A() { cout << "생성자 A" << endl; } 
	A(int x) { cout << "매개변수생성자 A" << x << endl; } 
};
```

```c++
class B : public A {
public:
	B() { cout << "생성자 B" << endl; }
	B(int x) {
		cout << "매개변수생성자 B" << x << endl;
	}
};
```

```c++
int main() { B b(5); }
```

실행결과:
생성자 A
매개변수생성자 B5

이유: 묵시적으로 부모 클래스의 기본 생성자를 호출

#### 어떻게 부모 클래스의 매개변수생성자를 호출할 수 있는가?

```c++
class B : public A {
public:
	B() { cout << "생성자 B" << endl; }
	B(int x) : A(x+3){
		cout << "매개변수생성자 B" << x << endl;
	}
};
```

실행결과:
매개변수생성자 A8
매개변수생성자 B5

명시를 해주면 된다.

### 8-3

```c++
#include <iostream>
#include <string>
using namespace std;
class TV {
	int size; // 스크린 크기
public:
	TV() { size = 20; }
	TV(int size) { this->size = size; } // size = 32
	int getSize() { return size; }
};
class WideTV : public TV { // TV를 상속받는 WideTV
	bool videoIn;
public:
	WideTV(int size, bool videoIn) : TV(size) { // size = 32, videoIn = true
		this->videoIn = videoIn;
	}
	bool getVideoIn() { return videoIn; }
};
class SmartTV : public WideTV { // WideTV를 상속받는 SmartTV
	string ipAddr; // 인터넷 주소
public:
	SmartTV(string ipAddr, int size) : WideTV(size, true) { // ipAddr = 192.0.0.1, size = 32 
		this->ipAddr = ipAddr;
	}
	string getIpAddr() { return ipAddr; }
};

int main() {
	// 32 인치 크기에 "192.0.0.1"의 인터넷 주소를 가지는 스마트 TV 객체 생성
	SmartTV htv("192.0.0.1", 32);
	cout << "size=" << htv.getSize() << endl;
	cout << "videoIn=" << boolalpha << htv.getVideoIn() << endl;
	cout << "IP=" << htv.getIpAddr() << endl;
}
```

매개변수 전달 받는 순서:
아래 -> 위(값은 주석 참고)
![[Pasted image 20231111190416.png]]

## 상속 지정