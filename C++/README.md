# C++ 공부

## cin 효율화

---

```cpp
ios_base :: sync_with_stdio(false); 
cin.tie(NULL); 
cout.tie(NULL);
```

## cmp함수 레퍼런스사용해서 만들기

---

```python
bool cmp(const Student& s1, const Student& s2){
    return s1.p1 + s1.p2 + s1.p3 < s2.p1 + s2.p2 + s2.p3;
}
```

## 생성자 안만들고 자동 호출

---

`{}` 사용해서 객체 할당하면 변수 인스턴스변수 순서대로 값을 할당해준다

```cpp
#include <iostream>
#include <string>
#include <vector>
#include <algorithm>
using namespace std;

class Student{
public:
    string name;
    int p1;
    int p2;
    int p3;
};

int main(void){
    vector<Student> v;
    string name;
    int p1;
    int p2;
    int p3;
  
    cin >> name >> p1 >> p2 >> p3;
    v.push_back({name, p1, p2, p3}); // 중괄호로 바로 대입 가능
}
```

## for문으로 객체를 받을 때 &사용해서 메모리 효율화


```cpp
for(auto &s : input)
        sout << s.name << endl;
```