---
layout: post

title: "C++ 연습"

categories: [dev, c++]

date: 2020-04-17
---

C++ 클래스를 다뤄보자

---

C++은 내가 주로 사용하던 언어는 아니다. 그냥 알고리즘 문제를 풀기 위해 문법과 몇가지 라이브러리를 아는 정도? 그러다 이번 평가에서 C++ 자체 클래스나, 클래스를 선언하고 다루는 것이 나와서 꽤나 고생을 했다. (공부좀 미리 해놓을걸^^....) 아무튼 잊어버리지 않기 위해 간단히 정리해보기.

## 1. `string` 클래스

C와는 달리 C++은 다양한 자체 라이브러리를 가지고 있다. queue나 vector 같은 자료구조를 구현한 것이나 sort 같은 것들은 알고리즘을 풀면서도 유용히 사용했던 라이브러리다. 이번 평가에서는 string 라이브러리를 사용하는 문제가 나왔는데, 알고리즘 문제를 풀면서도 종종 다뤘기 때문에 어렵지 않게 풀 수 있었다.

### `.find(const string& str, size_t index = 0)` 함수

첫번째 인자로 string을, 두 번째 인자로(optional) 인덱스 번호를 받는다. 현재 문자열 내에 매개변수로 보낸 string을 포함하고 있는지 반환하는 함수이다.

포함하고 있으면 포함된 문자가 시작하는 인덱스를 반환하고, 그렇지 않으면 `string::npos` 라는 상수를 반환한다. 두번째 인자로 인덱스 번호를 넘겨주면, 그 인덱스 번호 이후부터 탐색해 결과를 반환한다.

다음은 문자열 두 개를 입력받아, 첫번째 문자열 내에 두번째 문자열이 몇 번 등장하는지, 각 시작 인덱스 번호는 몇인지를 출력하는 코드다.

```c++
#include <iostream>
#include <string>
#define MAX 300010

using namespace std;

int N, M;
int foundlocs[MAX];
string original, pattern;

int main() {
    int i = 0, found, cnt = 0;

    scanf("%d", &N);
    cin >> original;
    scanf("%d", &M);
    cin >> pattern;

    while (true) {
        found = original.find(pattern, i);
        if (found == string::npos) break;
        foundlocs[cnt++] = found+1;
        i = found + 1;
    }

    if (!cnt) cout << -1;
    else {
        cout << cnt << "\n";
        for (int i = 0; i < cnt; i++) {
            cout << foundlocs[i] << " ";
        }
    }

    return 0;
}
```

## 2. 연산자 오버로딩

함수 오버로딩은 봤어도.. 연산자 오버로딩은 지금까지는 접해보지 못한 개념이다. 한마디로 정의하면 클래스를 선언하고 그 클래스에 대해 `+`, `-`, `<`, `==` 등 **기본적으로 C++이 가지고 있는 연산자를 오버로딩, 재정의 해 사용할 수 있다**는 것이다. 왜 이렇게 하는걸까 처음엔 잘 와닿지 않았는데, 평가 문제와 여러 예시를 보며 이해할 수 있었다.

> 후보의 최종 점수는 학생들로부터 받은 자신의 선호도 점수를 모두 더한 값이 된다. 그러면 3명의 후보 중 가장 큰 점수를 받은 후보가 회장으로 결정된다. 단, 점수가 가장 큰 후보가 여러 명인 경우에는 3점을 더 많이 받은 후보를 회장으로 결정하고, 3점을 받은 횟수가 같은 경우에는 2점을 더 많이 받은 후보를 회장으로 결정한다. 그러나 3점과 2점을 받은 횟수가 모두 동일하면, 1점을 받은 횟수도 같을 수밖에 없어 회장을 결정하지 못하게 된다.

평가 문제의 조건이다. 최고 점수를 받은 후보를 결정해야 하는데, **단순히 점수비교로 끝나지 않고 3점의 횟수, 2점의 횟수 등을 고려해야 한다**. 따라서 후보의 점수를 비교하는 데 복잡한 연산이 들어가게 된다.

이때 다음과 같이 **`<` 와 `==` 연산자를 클래스 내부에서 재정의**해줌으로써 main 함수 내에서는 간단히 표현할 수 있다. `operator` 키워드를 이용해 연산자 오버로딩을 암시한다. 이 때 사용된 연산자는 기존의 연산자와는 다른 역할을 하게 되는 것이다.

```c++
class Candidator() {
			//  ...
public:
		bool operator < (Candidator& a) {
        if (totalScore < a.totalScore) return true;
        if (totalScore > a.totalScore) return false;

        if (threes < a.threes) return true;
        if (threes > a.threes) return false;
        if (twos < a.twos) return true;

        return false;
    }

    bool operator == (Candidator& a) {
        if (totalScore == a.totalScore && threes == a.threes && twos == a.twos) return true;
        return false;
    }
}

int main() {
    Candidator cand1 = Candidator(1);
    Candidator cand2 = Candidator(2);
    Candidator cand3 = Candidator(3);

		// ...

    if (cand2 < cand1 && cand3 < cand1) cand1.printResult();
    else if (cand1 < cand2 && cand3 < cand2) cand2.printResult();
    else if (cand1 < cand3 && cand2 < cand3) cand3.printResult();
    else {
        printf("0 %d", max({cand1.getTotal(), cand2.getTotal(), cand3.getTotal()}));
    }

    return 0;
}
```

## 3. Class 로 Double Linked List 구현하기

왜 문제 이름을 class 생성자와 소멸자로 지었는지는 모르는.. 생성자와 소멸자보다는 Linked List 구현에서 훨씬 애를 먹은 문제다😭

```c++
#include <cstdio>
using namespace std;

class Node {
    friend class List;
private:
    char value;
    Node* next;
    Node* prev;
public:
    Node(char v) {
        next = nullptr;
        prev = nullptr;
        value = v;
    }
    Node(char v, Node* n, Node* p) {
        value = v;
        next = n;
        prev = p;
    }
    ~Node() {}
};

class List {
private:
    Node* head;
    Node* tail;
    Node* cur;
    int size;
public:
    List() {
        head = new Node(NULL);
        tail = new Node(NULL);
        cur = tail;
    }

    void pushBack(char value) {
        Node* newNode = new Node(value);
        if (head->next == nullptr) {
            head = newNode;
            tail = head;
            head->next = tail;
            tail->prev = head;
        } else {
            tail->next = newNode;
            newNode->prev = tail;
            tail = newNode;
        }
        cur = tail;
        size ++;
    }

    void moveCurLeft() {
        if (cur == head) return;
        cur = cur->prev;
    }

    void moveCurRight() {
        if (cur == tail) return;
        cur = cur->next;
    }

    void insertChar(char value) {
        Node* newNode = new Node(value);
        cur->prev->next = newNode;
        newNode->next = cur;
        newNode->prev = cur->prev;
        cur->prev = newNode;
    }

    void deleteChar() {
        Node* deleteTarget = cur->prev;
        cur->prev->prev->next = cur;
        cur->prev = cur->prev->prev;
        delete deleteTarget;
    }

    void printString() {
        Node* temp = head;
        while (temp) {
            printf("%c", temp->value);
            temp = temp->next;
        }
    }
};

int main() {
    int N; char temp;
    char order, value;
    List* linkedlist = new List();
    while (true) {
        scanf("%c", &temp);
        linkedlist->pushBack(temp);
        if (temp == '\n') break;
    }

    scanf("%d", &N);
    for (int i = 0; i < N; i++) {
        scanf(" %c", &order);
        if (order == 'L') linkedlist->moveCurLeft();
        if (order == 'D') linkedlist->moveCurRight();
        if (order == 'B') linkedlist->deleteChar();
        if (order == 'P') {
            scanf(" %c", &value);
            linkedlist->insertChar(value);
        }
    }
    linkedlist->printString();


    return 0;
}
```
