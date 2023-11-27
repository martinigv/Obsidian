# 11장

### 함수 중복의 약점 - 중복 함수의 코드 중복

**코드 중복의 문제:** 두 개의 `**myswap**` 함수가 거의 동일한 역할을 하고 있습니다. 이로 인해 코드 중복이 발생합니다.

### 일반화와 템플릿

제네릭 또는 일반화는 함수나 클래스를 틀에서 찍어 내듯이 생산하는 기법으로, 매개 변수의 타입을 지정하여 코드를 일반화시킨다.

템플릿은 변수나 매개 변수의 타입이 다르더라도 코드 부분이 동일한 함수를 일반화시키는 데에 사용된다.

구체화는 템플릿의 제네릭 타입에 특정한 타입을 지정하여, 템플릿 함수로부터 구체화된 함수의 소스 코드를 생성하는 프로세스

10-1

```
#include <iostream>
using namespace std;

class Circle {
	int radius;
public:
	Circle(int radius=1) { this->radius = radius; }
	int getRadius() { return radius; }
};
template <class T>
void myswap(T & a, T & b) {
	T tmp;
	tmp = a;
	a = b;
	b = tmp;
}

int main() {
	int a=4, b=5;
	myswap(a, b);
	cout << "a=" << a << ", " << "b=" << b << endl;

	double c=0.3, d=12.5;
	myswap(c, d);
	cout << "c=" << c << ", " << "d=" << d << endl;

	Circle donut(5), pizza(20);
	myswap(donut, pizza);
	cout << "donut반지름=" << donut.getRadius() << ", ";
	cout << "pizza반지름=" << pizza.getRadius()<< endl;
}
```

- `**int**`, `**double**`, 그리고 `**Circle**` 클래스에 대해 `**myswap**` 함수가 사용하여 서로 다른 데이터 타입에 대해 값 교환을 수행

### 템플릿 장점과 제너릭 프로그래밍

템플릿 장점:

- 함수 코드의 재사용을 통해 높은 소프트웨어 생산성과 유용성을 제공한다

템플릿 단점:

- 포팅에 취약하며, 컴파일러에 따라 지원하지 않을 수 있다.

- 컴파일 오류 메시지가 빈약하고 디버깅이 어려울 수 있다.

**제네릭 프로그래밍:**

- 일반화 프로그래밍으로 불리며, 제네릭 함수나 제네릭 클래스를 활용하는 프로그래밍 기법이다.

- C++에서는 STL(Standard Template Library)이 이를 지원하며, 이를 통해 보다 효과적인 프로그래밍이 가능하다.

10-2

```
#include <iostream>
using namespace std;

template <class T>
T bigger(T a, T b) { // 두 개의 매개 변수를 비교하여 큰 값을 리턴
	if(a > b)
		return a;
	else
		return b;
}

int main() {
	int a=20, b=50;
	char c='a', d='z';
	cout << "bigger(20, 50)의 결과는 " << bigger(a, b) << endl;
	cout << "bigger('a', 'z')의 결과는 " << bigger(c, d) << endl;
}
```

- 템플릿을 사용하여 구현된 `**bigger**` 함수를 통해 두 값을 비교하고, 더 큰 값을 반환

- 코드에서는 정수형과 문자형에 대해 `**bigger**` 함수를 호출하여 큰 값을 출력하고 있음

템플릿 함수에서는 T타입을 사용하고 있지만, 실제로는 어떻게 컴파일러가 반환 값을 추론하는가?

- 템플릿 함수가 호출될 때 컴파일러가 인자의 타입을 분석하고, 그에 따라 타입을 추론하여 최종적인 리턴 타입을 정하게 됨

10-3

```
#include <iostream>
using namespace std;

template <class T>
T add(T data [], int n) { // 배열 data에서 n개의 원소를 합한 결과를 리턴
	T sum = 0;
	for(int i=0; i<n; i++) {
		sum += data[i];
	}
	return sum; // sum와 타입과 리턴 타입이 모두 T로 선언되어 있음
}

int main() {
	int x[] = {1,2,3,4,5};
	double d[] = {1.2, 2.3, 3.4, 4.5, 5.6, 6.7};
	
	cout << "sum of x[] = " << add(x, 5) << endl; // 배열 x와 원소 5개의 합을 계산
	cout << "sum of d[] = " << add(d, 6) << endl; // 배열 d와 원소 6개의 합을 계산
}
```

- 배열의 합을 구하여 리턴하는 제네릭 `**add**` 함수를 구현한 예제

- 정수형과 실수형 배열에 대해 `**add**` 함수를 호출하여 각 배열의 합을 출력

10-4

```
#include <iostream>
using namespace std;

// 두 개의 제네릭 타입 T1, T2를 가지는 copy()의 템플릿
template <class T1, class T2>
	void mcopy(T1 src [], T2 dest [], int n) { // src[]의 n개 원소를 dest[]에 복사하는 함수
		for(int i=0; i<n; i++)
			dest[i] = (T2)src[i]; // T1 타입의 값을 T2 타입으로 변환한다.
}

int main() {
	int x[] = {1,2,3,4,5};
	double d[5];
	char c[5] = {'H', 'e', 'l', 'l', 'o'}, e[5];
	
	mcopy(x, d, 5); // int x[]의 원소 5개를 double d[]에 복사
	mcopy(c, e, 5); // char c[]의 원소 5개를 char e[]에 복사
	
	for(int i=0; i<5; i++) cout << d[i] << ' '; // d[] 출력
	cout << endl;
	for(int i=0; i<5; i++) cout << e[i] << ' '; // e[] 출력
	cout << endl;
}
```

- 배열을 복사하는 제네릭 `**mcopy**` 함수를 구현한 예제

- 정수형 배열과 문자형 배열을 대상으로 복사를 수행하고, 결과를 출력

배열을 출력하는 print() 템플릿 함수의 문제점

```
#include <iostream>
using namespace std;

template <class T>
void print(T array [], int n) {
	for(int i=0; i<n; i++)
		cout << array[i] << '\t';
	cout << endl;
}

int main() {
	int x[] = {1,2,3,4,5};
	double d[5] = { 1.1, 2.2, 3.3, 4.4, 5.5 };
	print(x, 5);
	print(d, 5);
	
	char c[5] = {1, 2, 3, 4, 5};
	print(c, 5);
}
```

- 문자형 배열인 `**c**`를 출력할 때, 각 원소를 숫자로 출력하는데, 숫자대신 문자로 출력되는 문제 발생

어떻게 해결해야하는가

- 문자형 배열에 대한 특수 처리를 추가하여 각 문자를 정수형으로 변환하지 않고도 올바르게 출력할 수 있도록 수정

- `**cout**`의 출력 형식을 일반화하여 모든 데이터 타입에 대해 적절한 출력이 이루어질 수 있도록 수정

10-5

```
#include <iostream>
using namespace std;

template <class T>
void print(T array [], int n) {
	for(int i=0; i<n; i++)
		cout << array[i] << '\t';
	cout << endl;
}

void print(char array [], int n) { // char 배열을 출력하기 위한 함수 중복
	for(int i=0; i<n; i++)
		cout << (int)array[i] << '\t'; // array[i]를 int 타입으로 변환하여 정수 출력
	cout << endl;
}

int main() {
	int x[] = {1,2,3,4,5};
	double d[5] = { 1.1, 2.2, 3.3, 4.4, 5.5 };
	print(x, 5);
	print(d, 5);
	
	char c[5] = {1,2,3,4,5};
	print(c, 5);
}
```

- 템플릿 함수와 중복 함수가 함께 정의되어 있음

- 템플릿 함수가 아닌 중복 함수가 우선적으로 호출되고 있음

템플릿 함수와 중복 함수가 함께 정의된 경우, 컴파일러는 어떻게 우선순위를 결정하고 중복 함수가 템플릿 함수보다 우선적으로 선택되는 이유가 무엇일까?

- 컴파일러는 함수 호출 시 정확한 매개변수 타입 매칭을 우선순위로 하며, 템플릿 함수와 중복 함수가 함께 정의된 경우 정확한 타입 매칭이 가능한 중복 함수가 더 우선적으로 선택된다.

10-6

```
#include <iostream>
using namespace std;

template <class T>
class MyStack {
	int tos;// top of stack
	T data [100]; // T 타입의 배열. 스택의 크기는 100
public:
	MyStack();
	void push(T element); // element를 data [] 배열에 삽입
	T pop(); // 스택의 탑에 있는 데이터를 data[] 배열에서 리턴
};

template <class T>
MyStack<T>::MyStack() { // 생성자
	tos = -1; // 스택은 비어 있음
}

template <class T>
void MyStack<T>::push(T element) {
	if(tos == 99) {
		cout << "stack full";
		return;
	}
	tos++;
	data[tos] = element;
}

template <class T>
T MyStack<T>::pop() {
	T retData;
	if(tos == -1) {
		cout << "stack empty";
		return 0; // 오류 표시
	}
	retData = data[tos--];
	return retData;
}

int main() {
	MyStack<int> iStack; // int 만 저장하는 스택
	iStack.push(3);
	cout << iStack.pop() << endl;
	
	MyStack<double> dStack; // double 만 저장하는 스택
	dStack.push(3.5);
	cout << dStack.pop() << endl;
	
	MyStack<char> *p = new MyStack<char>(); // char만 저장하는 스택
	p->push('a');
	cout << p->pop() << endl;
	delete p;
}
```

- 제네릭 스택 클래스를 구현한 예제

- 템플릿 사용하여 다양한 데이터 타입 처리할 수 있도록 설계되어 있으며, 정수형, 실수형, 문자형을 저장하는 세 가지 다른 스택을 사용함

10-7

```
#include <iostream>
using namespace std;

template <class T>
class MyStack {
	int tos;// top of stack
	T data [100]; // T 타입의 배열. 스택의 크기는 100
public:
	MyStack();
	void push(T element); // element를 data [] 배열에 삽입
	T pop(); // 스택의 탑에 있는 데이터를 data[] 배열에서 리턴
};

template <class T>
MyStack<T>::MyStack() { // 생성자
	tos = -1; // 스택은 비어 있음
}

template <class T>
void MyStack<T>::push(T element) {
	if(tos == 99) {
		cout << "stack full";
		return;
	}
	tos++;
	data[tos] = element;
}

template <class T>
T MyStack<T>::pop() {
	T retData;
	if(tos == -1) {
		cout << "stack empty";
		return 0; // 오류 표시
	}
	retData = data[tos--];
	return retData;
}

class Point {
	int x, y;
public:
	Point(int x=0, int y=0) { this->x = x; this->y = y; }
	void show() { cout << '(' << x << ',' << y << ')' << endl; }
};

int main() {
	MyStack<int *> ipStack; // int* 만을 저장하는 스택
	int *p = new int [3];
	for(int i=0; i<3; i++) p[i] = i*10; // 0, 10, 20으로 초기화
	ipStack.push(p); // 포인터 푸시
	int *q = ipStack.pop(); // 포인터 팝
	for(int i=0; i<3; i++) cout << q[i] << ' '; // 화면 출력
	cout << endl;
	delete [] p;
	
	MyStack<Point> pointStack; // Point 객체 저장 스택
	Point a(2,3), b;
	pointStack.push(a); // Point 객체 a 푸시. 복사되어 저장
	b = pointStack.pop(); // Point 객체 팝
	b.show(); // Point 객체 출력
	
	MyStack<Point*> pStack; // Point* 포인터 스택
	pStack.push(new Point(10,20)); // Point 객체 푸시
	Point* pPoint = pStack.pop(); // Point 객체의 포인터 팝
	pPoint->show(); // Point 객체 출력
	
	MyStack<string> stringStack; // 문자열만 저장하는 스택
	string s="c++";
	stringStack.push(s);
	stringStack.push("java");
	cout << stringStack.pop() << ' ‘;
	cout << stringStack.pop() << endl;
}
```

- 제너릭 스택의 제너릭 타입을 포인터나 클래스로 구체화 함

- 포인터와 사용자 정의 클래스인 `**Point**` 그리고 문자열을 저장하는 세 가지 다른 타입의 스택을 활용

10-8

```
#include <iostream>
using namespace std;

template <class T1, class T2> // 두 개의 제네릭 타입 선언
class GClass {
	T1 data1;
	T2 data2;
public:
	GClass();
	void set(T1 a, T2 b);
	void get(T1 &a, T2 &b);
};

template <class T1, class T2>
GClass<T1, T2>::GClass() {
	data1 = 0; data2 = 0;
}

template <class T1, class T2>
void GClass<T1, T2>::set(T1 a, T2 b) {
	data1 = a; data2 = b;
}

template <class T1, class T2>
void GClass<T1, T2>::get(T1 & a, T2 & b) {
	a = data1; b = data2;
}

int main() {
	int a;
	double b;
	GClass<int, double> x;
	x.set(2, 0.5);
	x.get(a, b);
	cout << "a=" << a << '\t' << "b=" << b << endl;

	char c;
	float d;
	GClass<char, float> y;
	y.set('m', 12.5);
	y.get(c, d);
	cout << "c=" << c << '\t' << "d=" << d << endl;
}
```

- 두 개의 제네릭 타입을 가진 클래스인 `**GClass**`를 정의
    
    - `**GClass<int, double>**`와 같이 템플릿을 인스턴스화할 때 `**T1**`은 `**int**`로, `**T2**`는 `**double**`로 대체되어 실제 클래스가 생성
    
    - 이렇게 함으로써 동일한 템플릿을 여러 타입에 대해 유연하게 사용

- 이 클래스는 두 가지 다른 타입의 데이터를 저장하고 설정하며, 설정된 데이터를 가져오는 기능을 제공

- `**main**` 함수에서는 정수와 실수, 문자와 실수를 저장하는 두 개의 `**GClass**` 객체를 생성하여 활용

### C++ 표준 템플릿 라이브러리, STL

STL은 C++ 표준 라이브러리 중 하나로, 다양한 제네릭 클래스와 제네릭 함수를 포함하고 있어 개발자가 효율적으로 응용 프로그램을 작성할 수 있도록 도와주는 도구

**컨테이너 (Containers):**

- 자료 구조를 표현한 클래스로, 데이터를 효율적으로 저장하고 관리하는 역할

- 예: 리스트, 큐, 스택, 맵, 셋, 벡터 등

**이터레이터 (Iterators):**

- 컨테이너의 원소에 대한 포인터 역할을 하는 객체로, 원소들을 순회하며 접근하는데 사용

**알고리즘 (Algorithms):**

- 템플릿 함수로 이루어진 독립적인 함수들로, 컨테이너 원소에 대한 다양한 작업을 수행

- 예: 검색, 정렬, 삭제, 복사 등의 기능을 제공

### vector 컨테이너

- **가변 길이 배열 구현:** Vector는 가변 길이 배열을 구현한 제네릭 클래스로, 배열의 크기에 대한 고민 없이 동적으로 크기가 조절

- **원소의 다양한 조작 기능:** 저장, 삭제, 검색 등 다양한 멤버 함수를 제공하여 벡터의 유연한 조작이 가능

- **인덱스로의 접근:** 벡터에 저장된 원소들은 인덱스를 통해 접근 가능. 이때, 인덱스는 0부터 시작