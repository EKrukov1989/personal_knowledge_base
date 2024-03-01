___
Паттерн pimpl позволяет эффективно скрыть имплементаци класса.

Класс имплементацию которого нужно скрыть:
```cpp
#pragma once
#include <memory>
class Face {
	class Impl;
	std::unique_ptr<Impl> m_Impl;
public:
	Face(int i);
	~Face();  // деструктор необходим!
	int exec();
};
```

FaceImpl.h
```cpp
#pragma once
class Face::Impl {
	int i_ = 0;
public:
	Impl(int i);
	int exec();
};
```

FaceImpl.cpp
```cpp
#include "Face.h"
#include "FaceImpl.h"
Face::Impl::Impl(int i) : i_(i) {}
int Face::Impl::exec() { return i_ + 1; }

Face::Face(int i) : m_Impl(new Impl(i)) {}
Face::~Face() {}
int Face::exec() {return m_Impl->exec(); }
```