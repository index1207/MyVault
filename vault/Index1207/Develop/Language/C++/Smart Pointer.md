#CPP
___
## 스마트 포인터
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

EX) 스코프에서의 소멸 타이밍
```cpp
int sum(int cnt) {
	auto arr = make_unique<int[]>(cnt);
	
}
```

EX) 멤버에서의 소멸 타이밍
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

#### shared_ptr


#### weak_ptr
