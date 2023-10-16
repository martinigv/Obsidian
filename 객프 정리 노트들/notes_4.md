## Pointer
int a = 0;
int* p;
p = &a;
```
*p = 3;
```

a의 데이터값은 3;

# slide04
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
	double getArea() {
		return 3.14 * radius * radius;
	}
};
```
