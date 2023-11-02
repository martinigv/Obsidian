# 07 프렌드와 연산자 중복
## 프렌드 함수
1. 클래스의 멤버 함수가 아닌 외부 함수
2. 프렌드 키워드로 클래스 내에 선언된 함수
3. 프렌드 선언의 필요성
4. 
## slide09
### 요약


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