#CPP
___
### 스마트 포인터
___
C++은 다른 언어들과 달리 GC([[Garbage Collection]])이 없어 [[Heap 메모리]] 영역을 직접 프로그래머가 관리해야 하기 때문에 `new`로 메모리를 할당해 주었다면 `delete`로 해제해줘야 한다. 만약 실수로 `delete`를 하지 않는다면 메모리 누수([[Memory Leak]])가 발생해 쓸데없이 메모리를 많이 먹는 프로그램이 될 것이다.

C++에서는 이를 방지하기 위해서 [[RAII]] 구조의 `스마트 포인터`라는 특수한 제네릭 클래스가 있다.
따라서 스코프 안에 메모리 할당을 하고 스코프를 벗어나면 할당이 해제되는 원리이다. 하지만 바로 해제되어 버리기만 하면 사용할 곳이 별로 없을 것이다. 그래서 해제되는 조건이 다른 여러 종류가 있다.

### 스마트 포인터 종류
___
- `unique_ptr<T>`
- `shared_ptr<T>`
- `weak_ptr<T>`

#### unique_ptr
`unique_ptr`은 가리키는 객체에 대한 소유권을 가지고 있다. 소유권이 자기 자신한테 있기 때문에 다른 객체에서 참조할 수 없다. 다른 객체에게 소유권을 넘겨주려면 `std::move`를 통해 이동해줘야 한다. 그리고 스코프를 벗어나거나 멤버로 있는 클래스가 소멸되면 해제된다.

스코프에서의 소멸 타이밍
```cpp
int sum(int cnt) {
	auto arr = make_unique<int[]>(cnt);
	iota(arr.get(), arr.get() + cnt, 1);
	return accumulate(arr.get(), arr.get()+cnt, 0);
}

// arr는 함수를 호출하고 바로 해제된다.
```
스코프를 벗어나면 할당 해제되는 원리를 이용한 예이다.

멤버 변수에서의 소멸 타이밍
```cpp
class TestA
{
public:
	TestA()
	{
		cout << "TestA()\n";
	}
	~TestA()
	{
		cout << "~TestA()\n";
		}
};

class TestB
{
public:
	TestB()
	{
		obj = make_unique<TestA>();
		cout << "TestB()\n";
	}
	~TestB()
	{
		cout << "~TestB()\n";
	}
private:
	unique_ptr<TestA> obj;
}

int main()
{
	TestB* test = new TestB;
}

/*
결과:
TestB()
TestA()
~TestB()
~TestA()
*/
```
소속된 클래스가 소멸되면 함께 소멸된다.
#### shared_ptr
`shared_ptr`은 참조 카운팅을 통해 소멸되는 타이밍이 결정된다. `unique_ptr`과는 달리 참조된 계수에 의해 소멸이 결정된다. 스코프를 벗어나더라도 나 말고도 다른 누군가가 참조하고 있다면 참조 계수가 하나 늘어난다. 객체의 소멸되는 조건은 참조 계수가 0이 되면 소멸된다.

참조 카운팅과 소멸 타이밍의 예
```cpp
#include <iostream>  
  
using namespace std;  
  
class Knight  
{  
public:  
    Knight() { cout << "Knight()\n"; }  
    ~Knight() { cout << "~Knight()\n"; }  
    shared_ptr<class Inventory> _inventory;  
};
  
class Inventory  
{  
public:  
    Inventory() { cout << "Inventory()\n"; }  
    ~Inventory() { cout << "~Inventory()\n"; }  
};  

int main()  
{  
    auto k = make_shared<Knight>();  
	auto inven = make_shared<Inventory>();  
	k->_inventory = inven;
}

/*
결과:
Knight()
Inventory()
~Knight()
~Inventory()
*/
```
위 코드는 Knight객체를 생성하고 Inventory객체를 생성해 Knight에 있는 `_inventory`로 참조시켰다.  
#### weak_ptr
