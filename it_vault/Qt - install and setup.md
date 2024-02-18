___
Как установить и настроить Qt:

1. Скачать онлайн-инсталлер: [www.qt.io](https://www.qt.io/download-qt-installer-oss?hsCtaTracking=99d9dd4f-5681-48d2-b096-470725510d34%7C074ddad0-fdef-4e53-8aa8-5e8a876d6ab4)
2. При установке Qt с помощью этого инсталлера использовать прокси, как описано здесь: https://quterussia.ru/download/
3. Установить переменную окружения `QT_QPA_PLATFORM=wayland` в `/etc/environment`. (Без этой переменной QtCreator не запускается)