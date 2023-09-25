## 16Slide
1. class 이름은 Rectangle
2. width, height int타입의 맴버 변수 필요
3. getArea() 맴버 함수 필요
```c++
#include <iostream>
using namespace std;
class Rectangle {
public:
	int width;
	int height;

	int getArea() {
		return width * height;
	}
};


int main() {
	Rectangle rect;
	rect.width = 3;
	rect.height = 5;
	cout << "사각형의 면적은 " << rect.getArea() << endl;
}
```
## 23Slide

#### 위임 생성자가 왜 필요할까?

1. 사람이 위임을 하는 이유-> 일을 분할(내가 할 일을 전달)
-> 효율 업!  -> 그럼 컴퓨터에서는? -> 코드의 간결화, 
메모리 절약 (2개 생성자-> 1개 생성자), 개발자의 코드 작성 최소화-> 효율 증가
## 30slide(3-6 실습)
1. width, height 변수 필요(가로, 세로)
2. 생성자 2개( Rectangle(n), Rectangle(w,h))(**Rectangle()은 Rectangle(n)에 위임**)
3. isSquare() 맴버 함수 필요(bool 타입)
4. isSquare() 구현
```c++
#include <iostream>
using namespace std;

class Rectangle {
public:
	int width;
	int height;

	Rectangle(int w, int h) {
		width = w;
		height = h;
	}
	Rectangle() : Rectangle(1) {}
	Rectangle(int n) {
		width = n;
		height = n;
	}
	bool isSquare() {
		return width == height;
	}
};

int main() {
	Rectangle rect1;
	Rectangle rect2(3, 5);
	Rectangle rect3(3);
	if (rect1.isSquare()) cout << "rect1은 정사각형이다." << endl;
	if (rect2.isSquare()) cout << "rect2는 정사각형이다." << endl;
	if (rect3.isSquare()) cout << "rect3는 정사각형이다." << endl;
}

```
## 11번 실습(파일 나누기)
main cpp파일(실행할 때 필요한 것들), Box.h 헤더파일(Box Class의 맴버 변수들& 함수들 선언), Box.cpp파일(Box Class의 맴버함수들 구현) 으로 나눈다
구현은 문제에 다 나와있으므로 이 부분 설명은 생략
#### main.cpp
```c++
#include <iostream>
#include "Box.h"
using namespace std;


int main() {
	Box b(10, 2);
	b.draw();
	cout << endl;
	b.setSize(7, 4);
	b.setFill('^');
	b.draw();
}
```
#### Box.cpp
```c++
#include <iostream>
#include "Box.h"
using namespace std;

Box :: Box(int w, int h) {
	setSize(w, h);
	fill = '*';
}
void Box::setFill(char f) {
	fill - f;
}
void Box::setSize(int w, int h) {
	width = w;
	height = h;
}
void Box::draw() {
	for (int i = 0; i < height; i++) {
		for (int j = 0; j < width; j++) {
			cout << fill;
		}
		cout << endl;
	}
}

```
#### Box.h
```c++
#pragma once

class Box {
	int width, height;
	char fill;
public:
	Box(int w, int h);
	void setFill(char f);
	void setSize(int w, int h);
	void draw();
};

```
