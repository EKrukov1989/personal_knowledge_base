___
Официальная страница: http://www.curlpp.org/
Проект на github:  https://github.com/jpbarrette/curlpp

Чтобы собрать curlpp следует выполнить следующий довольно стандартный набор команд:
```
sudo apt-get install libcurl4-openssl-dev
git clone https://github.com/jpbarrette/curlpp.git
cd curlpp
mkdir build
cd build
cmake ../
cmake --build .
make
make install
```

Пример get-запроса с использованием curlpp:

Структура проекта:
```
Makefile
main.cpp
libcurl.so.1
[app]
```

Makefile:
```
curlpp_dir = "/home/gene/Desktop/hubreps/curlpp"

app : main.cpp
	g++ -o app \
	-I "${curlpp_dir}/include" \
	-Wl,-rpath='$$ORIGIN' \
	main.cpp \
	-lcurlpp \
	-lcurl \
```

main.cpp:
```cpp
#include "curlpp/cURLpp.hpp"
#include "curlpp/Easy.hpp"
#include "curlpp/Options.hpp"

using namespace curlpp::options;

int main(int, char **)
{
	try
	{
		// That's all that is needed to do cleanup of used resources (RAII style).
		curlpp::Cleanup myCleanup;
		// Our request to be sent.
		curlpp::Easy myRequest;
		// Set the URL.
		myRequest.setOpt<curlpp::options::Url>("http://example.com");
		// Send request and get a result.
		// By default the result goes to standard output.
		myRequest.perform();
	}
	catch(curlpp::RuntimeError & e)
	{
		std::cout << e.what() << std::endl;
	}
	catch(curlpp::LogicError & e)
	{
		std::cout << e.what() << std::endl;
	}
	return 0;
}
```
