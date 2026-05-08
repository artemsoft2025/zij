\# \*\*POLYGLOT TOOL zij 1.0\*\*





НАЗНАЧЕНИЕ:

&nbsp; Сокрытие ZIP-архива внутри файлов различных форматов (изображения, видео,

&nbsp; аудио, документы) с полной маскировкой сигнатур PK.



ИСПОЛЬЗОВАНИЕ:

&nbsp; zij.exe {hide|extract} \[аргументы]



КОМАНДЫ:

&nbsp; hide    - спрятать ZIP-архив внутри файла-носителя

&nbsp; extract - извлечь ZIP-архив из файла-носителя



---

КОМАНДА HIDE - Сокрытие архива

---



СИНТАКСИС:

&nbsp; zij.exe hide <носитель> <архив> \[-o <выходной\_файл>]



ПАРАМЕТРЫ:

&nbsp; <носитель>        - файл, внутри которого будет спрятан архив

&nbsp; <архив>           - ZIP-архив, который нужно спрятать

&nbsp; -o <выходной>     - имя выходного файла (по умолчанию: output.bin)



ПРИМЕРЫ:

```

&nbsp; # Спрятать в PNG

&nbsp; zij.exe hide image.png secret.zip -o hidden.png



&nbsp; # Спрятать в JPEG

&nbsp; zij.exe hide photo.jpg archive.zip -o output.jpg



&nbsp; # Спрятать в PDF

&nbsp; zij.exe hide document.pdf data.zip -o hidden.pdf



&nbsp; # Спрятать в AVI

&nbsp; zij.exe hide video.avi backup.zip -o hidden.avi



&nbsp; # Спрятать в MP3

&nbsp; zij.exe hide song.mp3 files.zip -o hidden.mp3

&nbsp; ```



---

КОМАНДА EXTRACT - Извлечение архива

---

```

СИНТАКСИС:

&nbsp; zij.exe extract <файл> \[-o <выходной\_архив>]



ПАРАМЕТРЫ:

&nbsp; <файл>            - файл со скрытым архивом

&nbsp; -o <выходной>     - имя выходного ZIP-архива (по умолчанию: restored.zip)



ПРИМЕРЫ:

&nbsp; # Извлечь с именем по умолчанию

&nbsp; zij.exe extract hidden.png



&nbsp; # Извлечь с указанием имени

&nbsp; zij.exe extract hidden.png -o my\_archive.zip



&nbsp; # Извлечь из PDF

&nbsp; zij.exe extract hidden.pdf -o extracted.zip



&nbsp; # Извлечь из AVI

&nbsp; zij.exe extract hidden.avi -o backup.zip

```

---

ПОДДЕРЖИВАЕМЫЕ ФОРМАТЫ НОСИТЕЛЕЙ

---

```

&nbsp; ИЗОБРАЖЕНИЯ:  JPEG (.jpg, .jpeg)

&nbsp;               PNG (.png)

&nbsp;               GIF (.gif)

&nbsp;               BMP (.bmp)



&nbsp; ДОКУМЕНТЫ:    PDF (.pdf)



&nbsp; ВИДЕО:        AVI (.avi)



&nbsp; АУДИО:        WAV (.wav)

&nbsp;               MP3 (.mp3)



&nbsp; ДРУГИЕ:       WEBP (.webp)

```

---

ОСОБЕННОСТИ

---



&nbsp; 1. ПОЛНАЯ МАСКИРОВКА СИГНАТУР PK

&nbsp;    - Все сигнатуры PK (50 4B 03 04, 50 4B 01 02, 50 4B 05 06) заменяются

&nbsp;      на замаскированные байты

&nbsp;    - Команда "strings file.png | grep PK" НЕ НАЙДЁТ сигнатур

&nbsp;    - Работает для ВСЕХ форматов без исключения



&nbsp; 2. ПОЛНОЕ ВОССТАНОВЛЕНИЕ

&nbsp;    - При извлечении ZIP восстанавливается бит-в-бит

&nbsp;    - MD5 сумма совпадает с оригиналом

&nbsp;    - Все файлы в архиве распаковываются без ошибок



&nbsp; 3. СОХРАНЕНИЕ ФУНКЦИОНАЛЬНОСТИ

&nbsp;    - Файл-носитель после сокрытия остаётся работоспособным

&nbsp;    - Изображения открываются в просмотрщиках

&nbsp;    - Видео проигрывается в плеерах

&nbsp;    - PDF читается в документах



&nbsp; 4. КОНТРОЛЬ ЦЕЛОСТНОСТИ

&nbsp;    - Встроенная проверка MD5

&nbsp;    - При несовпадении MD5 выводится предупреждение



---

ПРИМЕР ПОЛНОГО ЦИКЛА РАБОТЫ

---

```

&nbsp; # 1. Создаём тестовый архив

&nbsp; echo "Секретный текст" > secret.txt

&nbsp; zip secret.zip secret.txt



&nbsp; # 2. Берём изображение (например, photo.png)

&nbsp; # 3. Прячем архив

&nbsp; zij.exe hide photo.png secret.zip -o hidden.png



&nbsp; # 4. Проверяем скрытность (не должно быть PK)

&nbsp; strings hidden.png | grep PK

&nbsp; # (пустой вывод)



&nbsp; # 5. Изображение正常но открывается

&nbsp; eog hidden.png



&nbsp; # 6. Извлекаем архив

&nbsp; zij.exe extract hidden.png -o restored.zip



&nbsp; # 7. Проверяем целостность

&nbsp; md5sum secret.zip restored.zip

&nbsp; # (суммы совпадают)



&nbsp; # 8. Распаковываем

&nbsp; unzip restored.zip

&nbsp; cat secret.txt

&nbsp; # Вывод: Секретный текст

```

---

СРАВНЕНИЕ С ОБЫЧНЫМ ПОЛИГЛОТОМ

---



&nbsp; ХАРАКТЕРИСТИКА           | ОБЫЧНЫЙ ПОЛИГЛОТ | ДАННАЯ УТИЛИТА

&nbsp; -------------------------|-----------------|------------------

&nbsp; Сигнатуры PK видны       | ДА              | НЕТ (маскируются)

&nbsp; strings найдет PK        | ДА              | НЕТ

&nbsp; file покажет ZIP         | ДА              | НЕТ

&nbsp; Восстановление 1:1       | ДА              | ДА

&nbsp; Поддержка многих форматов| НЕТ             | ДА (9 форматов)



---

ВОЗМОЖНЫЕ ОШИБКИ

---



&nbsp; ОШИБКА: "Неподдерживаемый формат"

&nbsp;   → Используйте один из поддерживаемых форматов



&nbsp; ОШИБКА: "Магическое число не найдено"

&nbsp;   → Файл не содержит данных, созданных этой утилитой



&nbsp; ОШИБКА: "Файл не найден"

&nbsp;   → Проверьте правильность пути и имя файла



&nbsp; ОШИБКА: "ZIP начинается с ..."

&nbsp;   → Возможно, файл повреждён или создан другой версией утилиты



---

ДОПОЛНИТЕЛЬНЫЕ КОМАНДЫ

---

```

&nbsp; # Показать эту справку

&nbsp; zij.exe -h



&nbsp; # Справка по конкретной команде

&nbsp; zij.exe hide -h

&nbsp; zij.exe extract -h



&nbsp; # Проверить версию (если добавлена)

&nbsp; zij.exe --version

```

================================================================================

&nbsp; (c) AKSoft, 2026, artemsoft@yahoo.com , https://t.me/avhelpnew ,

&nbsp; https://github.com/artemsoft2025/

