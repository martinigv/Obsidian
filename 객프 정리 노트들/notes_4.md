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
