## Pointer
int a = 0;
int* p;
p = &a;
```
*p = 3;
```

a의 데이터값은 3;

# slide04(4-1)
## main.cpp
```c++
#include<iostream>
#include "Circle.h"


using namespace std;

int main() {
	Circle donut;
	Circle pizza(30);

	cout << donut.getArea() << endl;

	Circle* p;
	p = &donut;
	cout << p->getArea() << endl; // donut의 getArea()
	cout << (*p).getArea() << endl; // 위와 같음

	p = &pizza;
	cout << p->getArea() << endl; // pizza의 getArea()
	cout << (*p).getArea() << endl; // 위와 같음

}
```
## Circle.h
```c++
#pragma once
#include <iostream>

using namespace std;

class Circle {
	int radius;
public:
	Circle() {
		radius = 1;
	}
	Circle(int r) {
		radius = r;
	}
	void setRadius(int r) {
		radius =r;
	}
	double getArea() {
		return 3.14 * radius * radius;
	}
};
```
# Slide06(4-2)
## main.cpp(헤더파일은 위와 같음)
배열에 직접 접근해서 함수를 통해 반지름을 초기화한다
맴버함수 setRadius(int r) 을 추가하여 초기화 할 수 있게 만든다.

```c++
#include <iostream>
#include "Circle.h"
int main() {
	Circle circleArray[3];
	circleArray[0].setRadius(10);
	circleArray[1].setRadius(20);
	circleArray[2].setRadius(30);
	for (int i=0; i < 3; i++)
		cout << "Circle" << i << "의 면적은" << circleArray[i].getArea() << endl;
	Circle *p;
	p = circleArray;
	for (int i=0; i < 3; i++) {
		cout << "Circle" << i << "의 면적은" << p->getArea() << endl;
		p++;
	}
	
}
```
# Slide10(4-3)
## main.cpp
배열에서 바로 초기화 하는건 어떻게 할까? 
배열 선언과 동시에 생성자를 사용하여 바로 초기화 한다.
```c++
#include <iostream>
using namespace std;

class Circle {
	int radius;
public:
	Circle() { radius = 1 }
	Circle(int r) { radius =r }
	double getArea() {
		return 3.14 * radius * radius;
	}
}
int main() {
	Circle circleArray[3] = {Circle(10), Circle(20), Circle()}; // Circle 배열 초기화하는 방법
	for (int i = 0; i < 3; i++)
		cout << "Circle" << i << "의 면적은" << circleArray[i].getArea() << endl;
}
```

# Slide12(4-4)

## main.cpp(헤더파일은 Circle.h로 동일)

```c++
#include <iostream>
#include "Circle.h"
using namespace std;


int main() {
	Circle circles[2][3];
	circles[0][0].setRadius(1);
	circles[0][1].setRadius(2);
	circles[0][2].setRadius(3);
	circles[1][0].setRadius(4);
	circles[1][1].setRadius(5);
	circles[1][2].setRadius(6);
	for (int i = 0; i < 2; i++) // 배열의 각 원소 객체의 멤버 접근
		for (int j = 0; j < 3; j++) {
			cout << "Circle [" << i << "," << j << "]의 면적은 ";
			cout << circles[i][j].getArea() << endl;
		}
}
```

# Slide16(4-5)

## main.cpp
```c++
#include <iostream>
using namespace std;
int main() {
	int* p;
	p = new int;
	if (!p) {
		cout << "메모리를 할당할 수 없습니다.";
		return 0;
	}
	*p = 5; // 할당 받은 정수 공간에 5 삽입
	int n = *p;
	cout << "*p = " << *p << '\n';
	cout << "n = " << n << '\n';
	delete p;
}
```

# Slide19(4-6)

## 문제
사용자로부터 입력할 정수의 개수를 입력 받아 배열을 동적 할당 받고, 하나씩 정수를 입력 받은 후
합을 출력하는 프로그램을 작성하라.

## 과정
n이라는 정수의 개수를 입력받을 변수를 선언하고 cin을 이용하여 n에 입력받은 수를 할당한다.
(단, 입력이 0일 경우 다시 입력하도록 한다)
포인터 변수 p를 선언하고 n개의 배열을 동적 할당한다.
(단, 할당할 수 없을 시 return)
각 p의 인덱스에 접근하여 입력받은 수를 할당한다.
똑같이 p의 인덱스에 차례대로 접근하여 sum변수에 더해준다.
sum을 출력한다
마지막 중요 **동적할당으로 사용된 저장공간을 다시 반환해줘야한다**

## main.cpp
```c++
#include <iostream>
using namespace std;

int main() {
	int n;
	do {
		cout << "정수 개수 적어라";
		cin >> n;
	} while (!n);

	int* p = new int[n];
	if (!p) {
		cout << "할당 안돼요";
		return 0;
	}
	cout << "정수 입력해라";
	for (int i = 0; i < n; i++) {
		cin >> p[i];
	}
	int sum{ 0 };
	for (int i = 0; i < n; i++) {
		sum += p[i];
	}
	cout << sum << endl;
	delete[] p;
}
```

# Slide27(4-10)

## 문제
원의 개수를 입력받고 Circle 배열을 동적 생성하라. 반지름 값을 입력 받아 Circle 배열에 저장하고
면적이 100에서 200 사이인 원의 개수를 출력하라.
## Circle.h
```c++
#include <iostream>
using namespace std;
class Circle {
	int radius;
public:
	Circle();
	~Circle() { }
	void setRadius(int r) { radius = r; }
	double getArea() { return 3.14 * radius * radius; }
};
Circle::Circle() {
	radius = 1;
}
```

## main.cpp
int main() {
	cout << "생성하고자 하는 원의 개수?";
	int n, radius;
	cin >> n; // 원의 개수 입력
	Circle* pArray = new Circle[n]; // n 개의 Circle 배열 생성
	for (int i = 0; i < n; i++) {
		cout << "원" << i + 1 << ": "; // 프롬프트 출력
		cin >> radius; // 반지름 입력
		pArray[i].setRadius(radius); // 각 Circle 객체를 반지름으로 초기화
	}
	int count = 0; // 카운트 변수
	Circle* p = pArray;
	for (int i = 0; i < n; i++) {
		cout << p->getArea() << ' '; // 원의 면적 출력
		if (p->getArea() >= 100 && p->getArea() <= 200)
			count++;
		p++;
	}
	cout << endl << "면적이 100에서 200 사이인 원의 개수는 "
		<< count << endl;
	delete[] pArray; // 객체 배열 소멸