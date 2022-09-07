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

## Map

---

개념

[[C++] map container 정리 및 사용법](https://blockdmask.tistory.com/87)

코드트리문제

[코드트리](https://www.codetree.ai/missions/5/problems/more-than-one-alphabet/description)

내코드

```cpp
#include <iostream>
#include <map>
#include <string>
using namespace std;

int main(void) {
	map<char, int> m;
	string s;

	cin >> s;
	for(char c : s) {
		if (m.find(c) != m.end()) {
			m[c] += 1;
		}
		else
			m.insert({ c, 1 });
	}

	if (m.size() >= 2)
		cout << "Yes" << endl;
	else
		cout << "No" << endl;
}
```

특징

- R-B트리

시간복잡도

- 검색 : `O(logN)`
- 삽입 : `O(logN)`
- 삭제 : `O(logN)`

## Unordered-map

---

[[C++] 해시맵(Hashmap)을 이해해보자 | std::unordered_map | 기술면접](https://woo-dev.tistory.com/106)

## Unordered-set

---

```cpp
#include <iostream>
#include <unordered_set>
#include <string>
using namespace std;

int main(void) {
	unordered_set<char> set;
	string s;

	cin >> s;
	for(char c : s) {
		set.insert(c);
		if (set.size() >= 2)
			break;
	}

	if (set.size() >= 2)
		cout << "Yes" << endl;
	else
		cout << "No" << endl;
}
```

## Tuple

---

활용 문제

[코드트리](https://www.codetree.ai/missions/5/problems/correlation-between-shaking-hands-and-infectious-diseases2/description)

```cpp
#include <iostream>
#include <algorithm>
#include <tuple>
using namespace std;

int main(void) {
	int N, K, P, T;
	int t, a, b;
	cin >> N >> K >> P >> T;
	pair<int, int> virus[101] = { { 0, 0 } };
	tuple<int, int, int> tuple[250] = { { 0, 0, 0 } };
	virus[P] = { 1, 0 };

	for (int i = 0; i<T; i++) {
		cin >> t >> a >> b;
		tuple[i] = { t, a, b };
	}
	sort(tuple, tuple + T);
	for (int i = 0; i < T; i++) {
		tie(t, a, b) = tuple[i];
		if (virus[a].first == 1 && virus[b].first == 0) {
			if (virus[a].second < K) {
				virus[a].second += 1;
				virus[b].first = 1;
			}
		}
		else if (virus[a].first == 0 && virus[b].first == 1) {
			if (virus[b].second < K) {
				virus[b].second += 1;
				virus[a].first = 1;
			}
		}
		else if (virus[a].first == 1 && virus[b].first == 1) {
			virus[a].second += 1;
			virus[b].second += 1;
		}
	}
	for (int i = 1; i < N + 1; i++) {
		cout << virus[i].first;
	}
	cout << endl;
}
```