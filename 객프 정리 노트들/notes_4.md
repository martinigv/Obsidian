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
```c++
#include <iostream>
using namespace std;

class Circle {
	int radius;
public:
	Circle() { radius = 1 }
	Circle(int r) { radius =r }
	double 
}
```