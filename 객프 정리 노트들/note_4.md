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

