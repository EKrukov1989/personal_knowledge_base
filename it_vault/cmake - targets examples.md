___
### 1. Подключение импортной библиотеки

Структура проекта:
```
build
	libcurl.so
	[app]
	...
curl
	curl.h
	...
CMakeLists.txt
main.cpp
```

>[!warning] Обратите внимание,
>что `libcurl.so` находится в папке `build`!

Файл CMakeLists.txt:
```
cmake_minimum_required(VERSION 3.25)
project(App LANGUAGES CXX)

add_library(curl SHARED IMPORTED)
target_include_directories(curl INTERFACE "curl")
set_target_properties(curl PROPERTIES IMPORTED_LOCATION "libcurl.so" )

add_executable( app main.cpp )
target_link_libraries(app PRIVATE curl)
```

Файл main.cpp:
```cpp
#include "curl.h"
int main()
{
	CURL *curl;
	CURLcode res;
	curl_global_init(CURL_GLOBAL_DEFAULT);
	curl = curl_easy_init();
	if(curl)
	{
		curl_easy_setopt(curl, CURLOPT_URL, "https://ya.ru/");
		curl_easy_setopt(curl, CURLOPT_CA_CACHE_TIMEOUT, 604800L);
		res = curl_easy_perform(curl);
		if(res != CURLE_OK)
			fprintf(stderr, "curl_easy_perform() failed: %s\n",
			curl_easy_strerror(res));
		curl_easy_cleanup(curl);
	}
	curl_global_cleanup();
	return 0;
}
```

___
### 2. Подключение header-only library

Структура проекта:
```
build
	[app]
	...
CMakeLists.txt
main.cpp
```

Файл CMakeLists.txt:
```
cmake_minimum_required(VERSION 3.25)
project(App LANGUAGES CXX)

add_library(gzip INTERFACE IMPORTED)
target_include_directories(gzip INTERFACE
	"/home/gene/Desktop/hubreps/gzip-hpp/include")

add_executable( app main.cpp )
target_link_libraries(app PRIVATE gzip)
```

Файл main.cpp:
```cpp
#include <gzip/compress.hpp>
#include <gzip/utils.hpp>
int main()
{
	std::string data = "hello";
	const char * pointer = data.data();
	std::size_t size = data.size();
	bool c = gzip::is_compressed(pointer, size);
	return 0;
}
```