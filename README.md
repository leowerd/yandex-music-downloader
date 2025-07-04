# yandex-music-downloader

> Внимание! В версии v3 был изменен способ авторизации и некоторые
> аргументы. Смотрите [MIGRATION.md](MIGRATION.md) для получения информации
> об изменениях

## Содержание
1. [О программе](#О-программе)
2. [Установка](#Установка)
3. [Получение данных для авторизации](#Получение-данных-для-авторизации)
4. [Примеры использования](#Примеры-использования)
5. [Использование](#Использование)
6. [Уровни совместимости](#Уровни-совместимости)
7. [Спасибо](#Спасибо)
8. [Дисклеймер](#Дисклеймер)

## О программе
Загрузчик, созданный вследствие наличия *фатального недостатка* в проекте [yandex-music-download](https://github.com/kaimi-io/yandex-music-download).

### Возможности
- Возможность загрузки:
    - Всех треков исполнителя
    - Всех треков из альбома
    - Всех треков из плейлиста
    - Отдельного трека
- Загрузка всех метаданных трека/альбома:
    - Номер трека
    - Номер диска
    - Название трека
    - Исполнитель
    - Дополнительные исполнители
    - Дата выпуска альбома
    - Обложка альбома
    - Название альбома
    - Текст песни (при использовании флага `--add-lyrics`)
- Загрузка треков в lossless качестве
- Поддержка паттерна для пути сохранения музыки

## Установка
Для запуска скрипта требуется Python 3.9+
```
pip install -U https://github.com/llistochek/yandex-music-downloader/archive/main.zip
yandex-music-downloader --help
```

## Получение данных для авторизации
https://yandex-music.readthedocs.io/en/main/token.html

## Примеры использования
Во всех примерах замените `<Токен>` на ваш токен.

### Скачать все треки [Arctic Monkeys](https://music.yandex.ru/artist/208167) в наилучшем качестве
```
yandex-music-downloader --token "<Токен>" --quality 2 --url "https://music.yandex.ru/artist/208167"
```

### Скачать альбом [Nevermind](https://music.yandex.ru/album/294912) в высоком качестве, загружая тексты песен в формате LRC (с временными метками)
```
yandex-music-downloader --token "<Токен>" --quality 1 --lyrics-format lrc --url "https://music.yandex.ru/album/294912"
```

### Скачать трек [Seven Nation Army](https://music.yandex.ru/album/11644078/track/6705392)
```
yandex-music-downloader --token "<Токен>" --url "https://music.yandex.ru/album/11644078/track/6705392"
```

## Использование
```
usage: yandex-music-downloader [-h] [--quality <Качество>] [--skip-existing]
                               [--lyrics-format {none,text,lrc}]
                               [--embed-cover]
                               [--cover-resolution <Разрешение обложки>]
                               [--delay <Задержка>] [--stick-to-artist]
                               [--only-music]
                               [--compatibility-level <Уровень совместимости>]
                               [--timeout <Время ожидания>]
                               [--tries <Количество попыток>]
                               [--retry-delay <Задержка>]
                               (--artist-id <ID исполнителя> | --album-id <ID альбома> | --track-id <ID трека> | --playlist-id <владелец плейлиста>/<тип плейлиста> | -u URL)
                               [--unsafe-path] [--dir <Папка>]
                               [--path-pattern <Паттерн>] --token <Токен>

Загрузчик музыки с сервиса Яндекс.Музыка

options:
  -h, --help            show this help message and exit

Общие параметры:
  --quality <Качество>  Качество трека:
                        0 - Низкое (AAC 64kbps)
                        1 - Оптимальное (AAC 192kbps)
                        2 - Лучшее (FLAC)
                        (по умолчанию: 0)
  --skip-existing       Пропускать уже загруженные треки
  --lyrics-format {none,text,lrc}
                        Формат текста песни (по умолчанию: none)
  --embed-cover         Встраивать обложку в аудиофайл
  --cover-resolution <Разрешение обложки>
                        Разрешение обложки (в пикселях). Передайте "original" для загрузки в оригинальном (наилучшем) разрешении (по умолчанию: 400)
  --delay <Задержка>    Задержка между запросами, в секундах (по умолчанию: 0)
  --stick-to-artist     Загружать альбомы, созданные только данным исполнителем
  --only-music          Загружать только музыкальные альбомы (пропускать подкасты и аудиокниги)
  --compatibility-level <Уровень совместимости>
                        Уровень совместимости, от 0 до 1. См. README для подробного описания (по умолчанию: 1)

Сетевые параметры:
  --timeout <Время ожидания>
                        Время ожидания ответа от сервера, в секундах. Увеличьте если возникают сетевые ошибки (по умолчанию: 20)
  --tries <Количество попыток>
                        Количество попыток при возникновении сетевых ошибок. 0 - бесконечное количество попыток (по умолчанию: 20)
  --retry-delay <Задержка>
                        Задержка между повторными запросами при сетевых ошибках (по умолчанию: 5)

ID:
  --artist-id <ID исполнителя>
  --album-id <ID альбома>
  --track-id <ID трека>
  --playlist-id <владелец плейлиста>/<тип плейлиста>
  -u URL, --url URL     URL исполнителя/альбома/трека/плейлиста

Указание пути:
  --unsafe-path         Не очищать путь от недопустимых символов
  --dir <Папка>         Папка для загрузки музыки (по умолчанию: .)
  --path-pattern <Паттерн>
                        Поддерживает следующие заполнители: #number, #track-artist, #album-artist, #title, #album, #year, #artist-id, #album-id, #track-id, #number-padded (по умолчанию: #album-artist/#album/#number - #title)

Авторизация:
  --token <Токен>       Токен для авторизации. См. README для способов получения
```

## Уровни совместимости
Уровень совместимости позволяет отойти от стандарта тегов, которого
придерживается библиотека mutagen. Сделано это для поддержки большего
количества музыкальных плееров. Ниже подробно описаны все уровни.

### 0
Стандартные теги mutagen.

### 1
Затрагиваемые форматы: `m4a`

- Теги с несколькими значениями (`\xa9ART` и `aART`) устанвливаются с
разделителем `;`. Пример: `Artist1; Artist2; Artist3`


## Спасибо
- Разработчикам проекта [yandex-music-api](https://github.com/MarshalX/yandex-music-api)
- @ArtemBay за [скрипт](https://github.com/MarshalX/yandex-music-api/issues/656#issuecomment-2306542725) получения ссылки на загрузку в lossless качестве
- @keltecc за [метод дешифрования файлов](https://github.com/llistochek/yandex-music-downloader/issues/112#issuecomment-2812535100)
- @leowerd за [корректные имена исполнителей](https://github.com/llistochek/yandex-music-downloader/issues/93#issuecomment-2960210879) при загрузке сборников


## Дисклеймер
Данный проект является независимой разработкой и никак не связан с компанией Яндекс.
