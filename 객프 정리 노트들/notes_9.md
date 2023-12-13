# 12장

## 텍스트 파일

- 사람들이 사용하는 글자 혹은 문자(특수문자 포함)들로만 구성되는 파일

- 각 문자마다 코드(이진수)할당
    - 아스키코드 유니코드

## 바이너리 파일

- 문자로 표현되지 않는 이미지, 오디오, 그래픽 등을 포함한 바이너리 데이터가 기록된 파일(mp3, ppt, exe 확장다 등)

- 텍스트 파일의 각 바이트는 문자로 해석

- 바이너리 파일의 각 바이트는 문자로 해석되지 않는 것도 있음

hwp는 바이너리 파일

→바이너리 정보 포함해서. ex) 글자 색, 서체, 비트맵 이미지, 표 등

파일 입출력 스트림은 파일을 프로그램과 연결

- ex) istream의 get, read()함수 → 연결된 장치로부터 읽는 함수, 키보드에 연결되면 키 입력을, 파일에 연결되면 파일에서 입력

- ostream의 put(), write()함수 → 연결된 장치에 데이터를 쓰는 기능. 스크린에 연결되어 있을 경우 화면에 데이터를 출력하고, 파일에 연결되어 있을 경우 파일에 데이터를 출력

텍스트 I/O: 이 방식은 문자 단위로 파일에 쓰거나 파일에서 읽음

즉, 문자를 기록하고 읽은 바이트를 문자로 해석한다. 이 방식은 텍스트 파일에만 적용

바이너리 I/O: 이 방식은 바이트 단위로 파일에 쓰거나 파일에서 읽음

데이터를 문자로 해석하지 않고, 있는 그대로 기록하거나 읽는다. 이 방식은 텍스트 파일과 바이너리 파일 모두에 입출력이 가능

12-1

```
#include <iostream>
#include <fstream>
using namespace std;

int main() {
    char name[10], dept[20];
    int sid;
    
    // 키보드로부터 읽기
    cout << "이름>>";
    cin >> name; // 키보드에서 이름 입력 받음
    cout << "학번>>";
    cin >> sid; // 키보드에서 학번 입력 받음
    cout << "학과>>";
    cin >> dept; // 키보드에서 학과 입력 받음
    
    // 파일 열기. student.txt 파일을 열고, 출력 스트림 생성
    ofstream fout("c:\\temp\\student.txt");
    if (!fout) { // 열기 실패 검사
        cout << "c:\\temp\\student.txt 파일을 열 수 없다";
        return 0;
    }
    
    // 파일 쓰기
    fout << name << endl; // name 쓰기
    fout << sid << endl; // sid 쓰기
    fout << dept << endl; // dept 쓰기
    
    fout.close(); // 파일 닫기
}
```

사용자로부터 이름, 학번, 학과를 입력받아서 "c:\temp\student.txt" 파일에 저장하는 예제

12-2

```
#include <iostream>
#include <fstream>
using namespace std;

int main() {
    // 스트림 객체 생성 및 파일 열기
    ifstream fin;
    fin.open("c:\\temp\\student.txt");
    if (!fin) { // 파일 열기 실패 확인
        cout << "파일을 열 수 없다";
        return 0;
    }
    
    char name[10], dept[20];
    int sid;
    
    // 파일 읽기
    fin >> name; // 파일에 있는 문자열을 읽어서 name 배열에 저장
    fin >> sid; // 정수를 읽어서 sid 정수형 변수에 저장
    fin >> dept; // 문자열을 읽고 dept 배열에 저장
    
    // 읽은 텍스트를 화면에 출력
    cout << name << endl;
    cout << sid << endl;
    cout << dept << endl;
    
    fin.close(); // 파일 읽기를 마치고 파일을 닫는다.
}
```

"c:\temp\student.txt" 파일을 열고, 파일에서 문자열과 정수를 읽어와서 화면에 출력하는 예제

파일모드

- 파일 입출력에 대한 구체적인 작업 형태에 대한 지정

- 파일 모드 지정 - 파일 열 때
    
    - open(“파일이름”, 파일모드)
    
    - ifstream(“파일이름”, 파일모드)
    
    - ofstream(“파일이름”, 파일모드)

12-3

```
#include <iostream>
#include <fstream>
using namespace std;

int main() {
    const char* file = "c:\\windows\\system.ini";
    ifstream fin(file);
    if (!fin) {
        cout << file << " 열기 오류" << endl;
        return 0;
    }
    
    int count = 0;
    int c;
    while ((c = fin.get()) != EOF) { // EOF를 만날 때까지 문자 읽기
        cout << (char)c;
        count++;
    }
    
    cout << "읽은 바이트 수는 " << count << endl;
    fin.close(); // 파일 닫기
}
```

"c:\windows\system.ini" 파일을 열고, 파일에서 문자를 하나씩 읽어와서 화면에 출력하는 예제

파일의 끝을 인지하는 방법

```
while (true) {
    int c = fin.get(); // 파일에서 문자(바이트)를 읽는다.
    if (c == EOF) {
        // 파일의 끝을 만난 경우. 이에 대응하는 코드를 작성
        break; // while 루프에서 빠져나온다.
    } else {
        // 읽은 문자(바이트) c를 처리한다.
        // ...
    }
}
```

- `**get()**` **함수를 사용하고, 리턴된 값이 EOF인지 확인하는 방법**

```
int c;
while ((c = fin.get()) != EOF) {
    // 파일에서 읽은 값 c를 처리하는 코드
    // ...
}
```

- `**get()**` **함수를 사용하고, 리턴된 값과 EOF를 비교하여 while 루프를 종료하는 방법**

아래코드는 잘못된 방법!

```
while (!fin.eof()) {
    int c = fin.get(); // 마지막 읽은 EOF(-1) 값이 c에 리턴된다.
    // 읽은 값 c를 처리하는 코드
    // ...
}
```

- `**eof()**` **함수를 사용하는 방법은 파일의 끝을 잘못 인지할 수 있음**

- EOF 값을 c에 읽어 사용한 후 다음 루프의 while 조건문에서 EOF에 도달한 사실을 알게 된다.

12-4

```
#include <iostream>
#include <fstream>
using namespace std;

int main() {
    const char* firstFile = "c:\\temp\\student.txt";
    const char* secondFile = "c:\\windows\\system.ini";
    
    fstream fout(firstFile, ios::out | ios::app); // 쓰기 모드로 파일 열기
    if (!fout) { // 열기 실패 검사
        cout << firstFile << " 열기 오류";
        return 0;
    }
    
    fstream fin(secondFile, ios::in); // 읽기 모드로 파일 열기
    if (!fin) { // 열기 실패 검사
        cout << secondFile << " 열기 오류";
        return 0;
    }
    
    int c;
    while ((c = fin.get()) != EOF) { // 파일 끝까지 문자 읽기
        fout.put(c); // 읽은 문자 기록
    }
    
    fin.close(); // 입력 파일 닫기
    fout.close(); // 출력 파일 닫기
}
```

"c:\temp\student.txt" 파일을 쓰기 모드로 열고, "c:\windows\system.ini" 파일을 읽기 모드로 열어서 내용을 복사하는 예제

**텍스트 파일을 라인 단위로 읽는 두 가지 방법**

**(1)** `**istream**`**의** `**getline()**` **함수 이용**

```
char buf[81]; // 한 라인이 최대 80개의 문자로 구성된다고 가정
ifstream fin("c:\\windows\\system.ini");
while(fin.getline(buf, 81)) { // 한 라인이 최대 80개의 문자로 구성. 끝에 '\0' 문자 추가
    // 읽은 라인(buf[])을 활용하는 코드
}
```

**(2) 전역 함수** `**getline(ifstream& fin, string& line)**` **이용**

```
string line;
ifstream fin("c:\\windows\\system.ini");
while(getline(fin, line)) { // 한 라인을 읽어 line에 저장. 파일 끝까지 반복
    // 읽은 라인(line)을 활용하는 코드 작성
}
```

12-5

```
#include <iostream>
#include <fstream>
using namespace std;

int main() {
    ifstream fin("c:\\windows\\system.ini");
    if (!fin) {
        cout << "c:\\windows\\system.ini 열기 실패" << endl;
        return 0;
    }

    char buf[81]; // 한 라인이 최대 80개의 문자로 구성된다고 가정
    while (fin.getline(buf, 81)) { // 한 라인이 최대 80개의 문자로 구성
        cout << buf << endl; // 라인 출력
    }

    fin.close();
    return 0;
}
```

`**istream**`**의** `**getline()**` **함수를 사용하여 텍스트 파일을 읽고 화면에 출력하는 예제**

12-6

```
#include <iostream>
#include <fstream>
#include <string>
#include <vector>
using namespace std;

void fileRead(vector<string>& v, ifstream& fin) {
    string line;
    while (getline(fin, line)) {
        v.push_back(line);
    }
}

void search(vector<string>& v, string word) {
    for (int i = 0; i < v.size(); i++) {
        int index = v[i].find(word);
        if (index != -1) {
            cout << v[i] << endl;
        }
    }
}

int main() {
    vector<string> wordVector;
    ifstream fin("words.txt");
    if (!fin) {
        cout << "words.txt 파일을 열 수 없습니다." << endl;
        return 0; // 열기 오류
    }
    fileRead(wordVector, fin);
    fin.close();
    cout << "words.txt 파일을 읽었습니다." << endl;

    while (true) {
        cout << "검색할 단어를 입력하세요 >> ";
        string word;
        getline(cin, word);
        if (word == "exit") {
            break; // 프로그램 종료
        }
        search(wordVector, word);
    }

    cout << "프로그램을 종료합니다." << endl;
    return 0;
}
```

`**getline(ifstream&, string&)**` **함수를 사용하여 "words.txt" 파일을 읽고, 사용자로부터 입력받은 단어를 검색하는 예제**

`**fileRead**` 함수에서 파일을 라인 별로 읽어 벡터에 저장하는데, 파일이 큰 경우에는 메모리 부담이 있을 수 있는데 어떻게 하는게 좋을까?

12-7

```
#include <iostream>
#include <fstream>

using namespace std;

int main() {
    // 소스 파일과 목적 파일의 이름
    const char* srcFile = "c:\\temp\\desert.jpg";
    const char* destFile = "c:\\temp\\copydesert.jpg";

    // 소스 파일 열기
    ifstream fsrc(srcFile, ios::in | ios::binary);
    if (!fsrc) { // 열기 실패 검사
        cout << srcFile << " 열기 오류" << endl;
        return 0;
    }

    // 목적 파일 열기
    ofstream fdest(destFile, ios::out | ios::binary);
    if (!fdest) { // 열기 실패 검사
        cout << destFile << " 열기 오류" << endl;
        return 0;
    }

    // 소스 파일에서 목적 파일로 복사하기
    int c;
    while ((c = fsrc.get()) != EOF) { // 소스 파일을 끝까지 한 바이트씩 읽는다.
        fdest.put(c); // 읽은 바이트를 파일에 출력한다.
    }

    cout << srcFile << "을 " << destFile << "로 복사 완료" << endl;

    fsrc.close();
    fdest.close();

    return 0;
}
```

주어진 파일 경로에서 소스 파일을 열어 바이너리 형태로 읽고, 다른 파일에 바이너리 형태로 쓰는 방식으로 파일을 복사하는 예제

파일을 복사할 때 한 바이트씩 처리하는 이유??

→더 효율적인 방법은 버퍼를 사용하여 여러 바이트를 한 번에 읽어와 쓰는 방식일듯?

12-8

```
#include <iostream>
#include <fstream>

using namespace std;

int main() {
    const char* file = "c:\\windows\\system.ini";
    ifstream fin;
    fin.open(file, ios::in | ios::binary); // 읽기 모드로 파일 열기

    if (!fin) { // 열기 실패 검사
        cout << "파일 열기 오류";
        return 0;
    }

    int count = 0;
    char s[32];

    while (!fin.eof()) { // 파일 끝까지 읽는다.
        fin.read(s, 32); // 최대 32 바이트를 읽어 배열 s에 저장
        int n = fin.gcount(); // 실제 읽은 바이트 수 알아냄
        cout.write(s, n); // 버퍼에 있는 n 개의 바이트를 화면에 출력
        count += n;
    }

    cout << "읽은 바이트 수는 " << count << endl;
    fin.close(); // 입력 파일 닫기

    return 0;
}
```

`**read()**` 함수를 사용하여 텍스트 파일을 바이너리 모드로 읽어오는 프로그램

파일을 32바이트씩 읽어오는 이유?

→32바이트씩 나누어 읽는 이유는 버퍼의 크기를 설정한듯

이렇게 작은 크기로 나누어 읽으면 메모리 효율성이 높아질 수 있다함

12-9

```
#include <iostream>
#include <fstream>

using namespace std;

int main() {
    const char* srcFile = "c:\\temp\\tulips.jpg";
    const char* destFile = "c:\\temp\\copytulips.jpg";

    ifstream fsrc(srcFile, ios::in | ios::binary);
    if (!fsrc) { // 열기 실패 검사
        cout << srcFile << " 열기 오류" << endl;
        return 0;
    }

    ofstream fdest(destFile, ios::out | ios::binary);
    if (!fdest) { // 열기 실패 검사
        cout << destFile << " 열기 오류" << endl;
        return 0;
    }

    // 소스 파일에서 목적 파일로 복사하기
    char buf[1024];
    while (!fsrc.eof()) { // 파일 끝까지 읽는다.
        fsrc.read(buf, 1024); // 최대 1024 바이트를 읽어 배열 buf에 저장
        int n = fsrc.gcount(); // 실제 읽은 바이트 수 알아냄
        fdest.write(buf, n); // 읽은 바이트 수 만큼 버퍼에서 목적 파일에 기록
    }

    cout << srcFile << "을 " << destFile << "로 복사 완료" << endl;

    fsrc.close();
    fdest.close();

    return 0;
}
```

`**read()**`와 `**write()**` 함수를 사용하여 이미지 파일을 복사하는 프로그램

`eof()` 함수를 사용하여 파일의 끝을 검사하는 방식은 잘못된 방식이라 했던거같은데 이걸 쓰는 이유?

→ ?

12-10

```
#include <iostream>
#include <fstream>

using namespace std;

int main() {
    char* file = "c:\\temp\\data.dat";
    ofstream fout;
    fout.open(file, ios::out | ios::binary); // 이진 모드로 파일 열기

    if (!fout) { // 열기 실패 검사
        cout << "파일 열기 오류";
        return 0;
    }

    int n[] = {0, 1, 2, 3, 4, 5, 6, 7, 8, 9};
    double d = 3.15;

    fout.write((char*)n, sizeof(n)); // int 배열 n을 한번에 파일에 쓴다.
    fout.write((char*)(&d), sizeof(d)); // double 값 하나를 파일에 쓴다.

    fout.close();

    // 배열 n과 d 값을 임의의 값으로 변경시킨다.
    for (int i = 0; i < 10; i++)
        n[i] = 99;
    d = 8.15;

    // 배열 n과 d 값을 파일에서 읽어 온다.
    ifstream fin(file, ios::in | ios::binary);

    if (!fin) { // 열기 실패 검사
        cout << "파일 열기 오류";
        return 0;
    }

    fin.read((char*)n, sizeof(n));
    fin.read((char*)(&d), sizeof(d));

    for (int i = 0; i < 10; i++)
        cout << n[i] << ' ';
    cout << endl << d << endl;

    fin.close();
    return 0;
}
```

`**ofstream**`를 사용하여 이진 파일에 데이터를 쓰고, 그 후에 `**ifstream**`를 사용하여 이진 파일에서 데이터를 읽어오는 프로그램

`ios::binary` 옵션이 필요한 이유?

→`**ios::binary**` 옵션은 파일을 바이너리 모드로 열 때 사용 일반적으로 텍스트 파일을 다룰 때는 개행 문자 등을 특별하게 처리하기 때문에 텍스트 파일 모드로 열게 되는데, 이때 개행 문자 등이 OS에 맞게 변환

텍스트 I/O 든 바이너리 I/O 든 파일의 끝을 만나면 EOF 리턴하므로 둘은 파일의 끝을 처리하는 방법에는 차이가 없다

하지만 개행 문자 ‘\n’를 읽고 쓸 때 서로 다르게 작동한다

12-11

```
#include <iostream>
#include <fstream>

using namespace std;

void showStreamState(ios& stream) {
    cout << "eof() " << stream.eof() << endl;
    cout << "fail() " << stream.fail() << endl;
    cout << "bad() " << stream.bad() << endl;
    cout << "good() " << stream.good() << endl;
}

int main() {
    const char* noExistFile = "c:\\temp\\noexist.txt"; // 존재하지 않는 파일명
    const char* existFile = "c:\\temp\\student.txt"; // 존재하는 파일명

    ifstream fin(noExistFile); // 존재하지 않는 파일 열기

    if (!fin) { // 열기 실패 검사
        cout << noExistFile << " 열기 오류" << endl;
        showStreamState(fin); // 스트림 상태 출력
        cout << existFile << " 파일 열기" << endl;
        fin.open(existFile);
        showStreamState(fin); // 스트림 상태 출력
    }

    // 스트림을 끝까지 읽고 화면에 출력
    int c;
    while ((c = fin.get()) != EOF)
        cout.put((char)c);

    cout << endl;
    showStreamState(fin); // 스트림 출력
    fin.close();

    return 0;
}
```

파일 스트림의 상태를 확인하는 예제

1. **순차 접근 (Sequential Access):**
    
    - 읽은 다음 위치에서 읽거나, 쓴 다음 위치에 쓰는 방식
    
    - 디폴트 파일 입출력 방식으로, 파일을 처음부터 끝까지 차례대로 읽거나 쓰는 방법
    
    - 순차 접근에서는 읽은 위치와 쓴 위치를 따로 관리하지 않아도 됨

2. **임의 접근 (Random Access):**
    
    - 파일 내의 임의의 위치로 옮겨 다니면서 읽고 쓸 수 있는 방식
    
    - 파일 포인터를 사용하여 파일 내에서 원하는 위치로 이동하고 데이터를 읽거나 쓸 수 있음

3. **파일 포인터 (File Pointer):**
    
    - 파일은 연속된 바이트의 집합이며, 파일 포인터는 파일 내에서 다음에 읽거나 쓸 위치를 표시하는 특별한 마크
    
    - C++에서는 열려진 파일마다 두 개의 파일 포인터를 유지
        
        - **get pointer (입력용 포인터):** 파일 내에서 다음에 읽을 위치를 가리킴
        
        - **put pointer (출력용 포인터):** 파일 내에서 다음에 쓸 위치를 가리킴
        

임의 접근은 주로 `**seekg**` (seek get)와 `**seekp**` (seek put) 함수를 사용하여 파일 포인터를 이동시킴으로써 이루어짐. 이를 통해 파일 내에서 특정 위치로 이동한 후 데이터를 읽거나 쓸 수 있음

예를 들어, `**seekg(10)**`은 파일 포인터를 파일의 10번째 바이트로 이동시키는 것이며, 이후의 파일 입출력 작업은 해당 위치에서 이루어짐