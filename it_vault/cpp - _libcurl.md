___

1. Клонировать репозиторий: https://github.com/curl/curl.git
2. Собрать libcurl с помощью cmake
3. Копировать файл `libcurl.so.4.8.0` и папку `include` в своей проект.

Пример реализации https-GET запроса с помощью licburl на основе: https://curl.se/libcurl/c/https.html

Структура файлов проекта:
```
curl
	curl.h
	...
libcurl.so
main.cpp
Makefile
[app]
```

Makefile:
```
app : main.cpp
	g++ -o app \
	-Wl,-rpath='$$ORIGIN' \
	main.cpp \
	-lcurl \
	-L/home/gene/Desktop/station/expl/netup/curl_linking \

# порядок файлов имеет значение
```

```cpp
#include <iostream>
#include "curl/curl.h"

int main()
{
	CURL *curl;
	CURLcode res;
	curl_global_init(CURL_GLOBAL_DEFAULT);
	curl = curl_easy_init();
	if(curl)
	{ 
		curl_easy_setopt(curl, CURLOPT_URL, "https://ya.ru/");
#ifdef SKIP_PEER_VERIFICATION
		curl_easy_setopt(curl, CURLOPT_SSL_VERIFYPEER, 0L);
#endif

#ifdef SKIP_HOSTNAME_VERIFICATION
		curl_easy_setopt(curl, CURLOPT_SL_VERIFYHOST, 0L);
#endif
		/* cache the CA cert bundle in memory for a week */	
		curl_easy_setopt(curl, CURLOPT_CA_CACHE_TIMEOUT, 604800L);
	
		/* Perform the request, res will get the return code */
		res = curl_easy_perform(curl);
	
		/* Check for errors */
		if(res != CURLE_OK)
			fprintf(stderr, "curl_easy_perform() failed: %s\n", curl_easy_strerror(res));
	
		/* always cleanup */
		curl_easy_cleanup(curl);
	}
	curl_global_cleanup();
	return 0;
}
```