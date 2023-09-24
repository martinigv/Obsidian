# 25 slide

### 접근법
while 문을 통해 암호가 틀리면 암호를 다시 입력하게 만든다.
pw(password)를 선언하고 입력받아 암호랑 비교를 한다.
비교해서 같으면 while문을 빠져나오고 아니면 암호를 다시 입력한다.
```c++
#include <iostream>

using namespace std;


int main() {
	string pw;
	bool done = false;
	cout << "프로그램을 종료하려면 암호를 입력하세요." << endl;
	
	while (!done) {
		cout << "암호 >>";
		cin >> pw;
		if (pw == "C++") { // strcmp(password,
			cout << "프로그램을 정상 종료합니다" << endl;
			done = true;
		}
		else {
			cout << "암호가 틀립니다~~" << endl;
		}
	}
	return 0;
}
```
# 8번 문제
### 접근법 
1. 입력 이름에 space 공간이 있기 때문에 getline() 함수를 써보자
2. 5명 이름이니까 5번 반복을 하는 반복문을 사용하면 되겠군
3. 반복문에 어디까지 포함해야 할까? -> 이름을 입력받았을 때 출력하는 문자열, 문자열의 크기를 저장 그리고 비교 -> 가장 긴 이름 갱신
4. 이름을 입력받는 변수, 가장 긴 문자열의 길이를 받는 변수, 가장 긴 이름을 받는 변수. 이렇게 처음에 설정해본다. 
5. 이름을 입력받는 변수를 string 타입으로 하고 싶다. -> getline() 함수 사용 가능
6. string의 문자열 크기 -> string.length()을 사용
7. 가장 긴 문자열의 길이를 맨 처음 입력받는 길이로 해줘야하기 때문에 for 문에서 처음 입력받은 문자열만 maxlength 변수에 저장한다. 그 뒤 비교.
```c++
#include <iostream>
#include <string>
using namespace std;
int main() {
	string name;
	int maxlength;
	string maxname;
	cout << "5 명의 이름을 ';'으로 구분하여 입력하세요." << endl;
	for (int i = 0; i < 5; i++) {
		getline(cin, name, ';');
		cout << i + 1 << ": " << name << endl;
		if (i == 0) {
			maxlength = name.length();
		}
		if (name.length() > maxlength) {
			maxlength = name.length();
			maxname = name;
		}
	}
	cout << "가장 긴 이름은 " << maxname;
	return 0;
}
```
