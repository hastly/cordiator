Артефакт:

  ≡ Каркасная бибилиотека (python пакет) CorDio

Определение:

  ≡ Единообразно используемый каркас компонента микросервиса, реализованного на Python


Служит следующей цели:

  ≡ Создавать компоненты микросервиса на основе отработанной на практике единой платформы


Решает следующие задачи:
===================================

  ≡ Избежать ручного переноса "boilerplate" кода, повторяющегося от приложения к приложению
  ≡ Добавлять в приложение "из коробки" функции журналирования и мониторинга, отвечающие стандартным требованиям
  ≡ Внести единообразие в поведение различных приложений в области настройки и обработки исключений
  ≡ Внести единообразие во внутреннюю организацию структуры проекта
  ≡ Позволить гибкое управление применяемой стандартной функциональностью


Темы предоставляемой функциональности:
===================================
 
 ≡ Каркас
 ≡ Поток исполнения
 ≡ Внедрение сервисов
 ≡ Обработка нежелательных состояний
 ≡ Конфигурирование
 ≡ Мониторинг
 ≡ Утилиты


Предоставляет следующую функицональность:
===================================

 ≡ [Каркас] Основа микросервиса
 Независимое от конкретных каркасных и интерфейсных пакетов приложение, управляемое событиями
   ≡ Каркасный объект "приложения"
   ≡ Запуск как демона, ASGI приложения, обычного приложения
   ≡ Абстрагирование от (1) каркасных бибилиотек WEB приложений и (2) интерфейсов к хранению и обмену (postgres, rabbitmq, ...)
   ≡ Установка дополнительных зависимостей (основные фреймворки, дополнительная функциональность) по выбору
   ≡ Опциональное приведение данных запросов интерфейса бибилиотеки к однообразному интерфейсу данных приложения
   ≡ Модели данных и валидация
   ≡ Создание основы микросервиснового компонента на основе шаблона
   ≡ Подключение широко применяемых на практике инструментов контроля качества кода (линтинг, тестирование, типизация)
   ≡ Поддержка версионирования и сборки приложения (semver, datestamp, hashtag, migration)
 
 ≡ [Поток исполнения]
 Конструкция события-запроса с цепью асинхронных обработчиков
   ≡ Запуск асинхронных задач в рамках процесса
   ≡ Асинхронный запуск корневого компонента приложения
   ≡ Явное (и по выбору - неявное) подключение обработчиков запросов через декораторы с обеспечением контекста
   ≡ "Мягкое" завершение процесса с учётом запущенных асинхронных задач

 ≡ [Внедрение сервисов] Обеспечение обработчиков вспомогательными компонентами
 Единый механизм подключения сервисов разного типа: обмена, хранения, мониторинга
   ≡ Сервисы подключаются при использовании (использованием декоратора или передачей параметра с наименованием, используемым по соглашению)
   ≡ Подключаются только используемые сервисы
   ≡ Сервисы инициализируются и уничтожаются с учётом взаимозависимости
   ≡ Конфигурирование сервисов происходит с помощью единого сервиса среды исполнения
   ≡ Позволяет управлять постоянством подключения сервиса к используемому ресурсу (отключение по тайм-ауту, попытки переподключения)
 
 ≡ [Обработка нежелательных состояний] Работа над ошибками
 Структурированная обработка исключений: реакция, назначения, ответы, коды и тексты сообщений
   ≡ Какие исключения приводят к повтору операции
     Повторные попытки обращения к ресурсам в случае ошибок по настраиваемой схеме
   ≡ Какие исключения игнорируются
   ≡ Какие исключения фиксиируются в общих логах, логах исключений, трассировке
   ≡ Какие ответы на запросы выдаются при исключениях
   ≡ Какие коды и сообщения по какому назначению используются
 
 ≡ [Конфигурирование] Управление конфигурацией приложения
 Формирование настроек среды исполнения приложения
   ≡ Компонует единое множество конфигурационных значений из нескольких источников разных типов, согласно установленному приоритету
   ≡ Предоставляет значения компоненту приложения в необходимом ему срезе через механизм внедрения
   ≡ Опционально валидирует, преобразует полученные значения и обеспечивает их значениями по-умолчанию
   ≡ Специальным образом поддерживает определённые шаблонные конфигурации
 
 ≡ [Мониторинг] Средства контроля за исполнением
 Мониторинг и журналирование с единым описанием входных данных и разными назначениями
   ≡ Встроенное логирование как назначение: в файл, консоль, БД согласно настройкам
   ≡ Стороннее логирование: исключений (Sentry), трасс (Jaeger/Zipkin) и метрик (Statsd)
   ≡ Неявное подключение по стандартным точкам и настраиваемые пользовательские точки
   ≡ Маскирование конфиденциальных данных
   ≡ Минимальное вмешательство в процесс обработки до выполнения основного запроса
   ≡ Основная работа с журналируемыми данными после завершения основного процесса
   ≡ Запуск обработчика диагностических запросов: проверки работоспособности (heart-beat) и состояния и конфигураций приложения (какие асинхронные процессы запущены, какие обработчики)
   ≡ Возможность подавлять создание (или задавать наполнение) участков (или управлять используемыми типами логирования) по контексту
   ≡ Разбивать исходящие участки на группы, соответствующий этапам прохождения по бизнес-процессу
   ≡ Фиксировать и логировать место в коде, где была закрыта входящая точка и сформирован ответ

 ≡ [Утилиты] Общие механизмы
 Абстрактные от предметной области задачи, выделенные в сервисы или функции
   ≡ Сборник небольших утилитарных методов
   ≡ Авторизация / сертификаты
   ≡ Маскирование чувствительных данных


Каркас:
===================================

 ≡ Сценарии:
   ≡ Компоновки конфигурации
   ≡ Неявных умолчаний приложения
   ≡ Повторов
   ≡ Работы над ошибками
   ≡ Мониторинга

Поток исполнения:
===================================
  pass

Внедрение сервисов:
===================================
  pass

Действия по нежелательным состояниям:
===================================

 ≡ 3 действия
   ≡ [break] прерывание потока: остановка, повтор (retryFM) или переход
   ≡ [respond] формирование ответа
   ≡ [commit] фиксация факта
     ≡ в трассировщике (tracoon-zipkin)
     ≡ в метриках (tracoon-stats)
     ≡ в файлах (py logging)
     ≡ в реляционной базе (py logging with custom logger)
     ≡ в очереди задач (py logging with custom logger)

 ≡ слои действий: (reerrection herded layers)
   ≡ обработчика - основной метод и общие инициализирующие и финализирующие методы в пределах объекта которому принадлежит обработчик, последовательные вызовы методов этого объекта из основного (родственные методы)
     * под обработчиком понимается метод, выполняющийся по событиям запросам разного рода: HTTP, AMQP, внутрение и т.д.
     * обработчик может быть несвязанной функцией, тогда методы жиненного цикла и родственные будут отсутствовать, но логика работы слоя останется той же самой
     * декораторы, применённые к обработчику, находятся внутри слоя, то есть способны обрабтывать всплывшие исключения, до того как они попадут на слой
   ≡ сервиса - любые вызовы из обработчика методов сторонних сервисов или независимых функций
     * на уровне сервиса можно выбирать вариант с передачей исключений вверх до уровня обработчика, за исключением того случая, когда сам сервис является верхним уровнем
   ≡ произвольный - вложенная последовательность вызовов, оформленная специальным декоратором
     * предназначено для указания места перехода

     Каждый вызов внутри cordio приложения происходит внутри того или иного управляемого слоя (herded layer). В случае возникновения необработанных исключений, они перехватываются на уровне ближайшего слоя


Конфигурирование:
===================================

 ≡ Используемые средства
   ≡ Click Arguments Parse library https://github.com/pallets/click + https://github.com/click-contrib
     для определения, парсинга и проверки опций

 ≡ Предоставляемая функциональность:
   ≡ Основная:
     ≡ Задание возможных опций и их атрибутов с помощью декораторов основной функции приложения
     ≡ Создание общей композиции опций из множества источников
     ≡ Валидация полученных значений по типу и наличию с заданной реакцией в случае непрохождения
     ≡ Приведение значения к указанному типу
     ≡ Предоставление полученного множества опций потребителю в нужном срезе
   ≡ Сервисная:
     ≡ Отображение актуальных значений
     ≡ Создание шаблонов конфигурационных файлов

 ≡ Конфигурирование опций
   ≡ Возможные параметры задаются декораторами библиотеки Click
   ≡ Помимо этого возможно подключение стандартных блоков опций методом Cordio
   ≡ Запуск приложения со специальным параметром генерирует шаблон конфигурационного файла по вобору в одном из двух вариантах: со всеми параметрами, только с обязательными параметрами, при этом можно выбрать формат выводимого файла (из тех же, что поддерживаются для разбора) плюс указать заполнители значений: пустые, кавычки и скобки, где нужно, примерные, кавычки для шаблона
   ≡ Запуск приложения со специальным параметром валидирует все обнаруженные конфигурационные файлы или только указанные, и показывает текущие полученные значения и их источник

 ≡ Источники конфигураций
   ≡ Приоритет источников конфигурации может быть изменён через программный интерфейс
   ≡ Конфигурационные значения могут быть записаны в едином файле с секциями в соответствующем формате, либо разбиты по файлам в соответствии с секциями
   ≡ Источники значений в порядке возрастания приоритета по умолчанию
     ≡ Множество конфигурационных файлов
     ≡ Переменные среды
     ≡ Переменные среды из файла .env
     ≡ Параметры коммандной строки

 ≡ Форматы конфигурационных файлов и обнаружение
   ≡ TOML или как его вырожденный вариант INI
   ≡ YAML или как его вырожденный вариант JSON, поддержка JSON с комментариями
   ≡ При инициализации приложения в коде задаётся список относительных путей внутри проекта и/или абсолютных по которым происходит поиск конфигурационных фалов по заданному шаблону (regex или gitignore подобные globs), если это явно не задано, используется вариант по-умолчанию.
   ≡ Обнаруженные файлы упорядочиваются по алфавиту по своему неквалифицировнному (без пути) имени и их содержимое преобразуется в словарь python. При этом, если встречаются повторяющиеся ключи, то побеждает последнее значение. Также опционально можно задать вывод ошибки валидации конфигурации и остановку приложения при обнаружении дублирующих значений
   ≡ При невозможности отпарсить конфигурационный файл в формате по указанному расширению или при отсутствии расширения, парсинг пробует все возможные форматы по очереди, невозхможность загрузить конфигураицонный файл отображается в STDERR и, опционально, приложение завершает работу

 ≡ Специальная поддержка определённых шаблонных конфигураций
   ≡ Ресурсов хранения и обмена
   ≡ Средств журналирования и мониторинга

 ≡ Основное, что опции могут задавать:
   ≡ Адреса, реквизиты и настройки адаптера сторонних ресрусов
   ≡ Собственные адреса и реквизиты
   ≡ Включение / отключение определённой функциональности или режима работы
   ≡ Настройки журналирования различного рода
   ≡ Поведение и сообщения при исключениях
   ≡ Настройки поведения при взаимодейтсвии с другими ресурсами

 ≡ Настройки управления по-умолчанию:
   ≡ Пути поиска конфигурационных файлов: ./config/**/main.{json,yaml}
   ≡ Файл переменных среды: ./.env
   ≡ Поведение при обнаружении перекрывающихся значений: остановка выполнения
   ≡ Поведение при ошибках в парсере файлов конфигурации: остановка выполнения


Средства контроля за исполнением:
===================================
  pass




Набор инструментов:
 
 ≡ Постройка пакета, доставка и управление зависимостями и средой исполнения: poetry
 ≡ Совместимость конфигураций пакета с разными инструментами (pip и pipenv), версионирование semver, calver etc.: depHell+plugins
 ≡ Поддержка соглашений по форматированию кода: flake-8, black, isort
 ≡ Статический анализ кода при сборке: mypy
 ≡ Запуск тестов при сборке: tox
 ≡ Тестирование: pytest, coverage, hypothesis
 ≡ Поддержка создания производных приложений по шаблону через cookiecutter
 ≡ Создание документации: sphinx, swagger
 ≡ Работа с командной строкой, настрйоками и переменными среды: click, parseit
 ≡ Сериализация/десериализация с проверкой по схеме: typesystem (?marshmallow ?pydantic)
 ≡ Взаимодействие с ресурсом хранения (SQL DBMS): asyncpg, buildpg, gino(?)
 ≡ Взаимодействие с ресурсом обмена (AMQP): aio-pika
 ≡ Запуск по расписанию sched (stdlib)
 ≡ Межкомпонентное взаимодействие и клиентский интерфейс (HTTP): aiohttp
 ≡ Возможно добавятся tornado5/6
 ≡ Возможно добавятся uvicorn и его надстройки (starlette, fastapi), управляемые по пртооколу ASGI и созданные с помощью библиотек парсинга HTTP h11/h2 или httptools используя асинхронные циклы ввода-вывода uvloop или asyncio/epoll
 ≡ micheles / decorator
 

Подразумеваемые элементы конфигурации проекта:
 Могут быть замещены явными или работать вместе с ними в некоторых случаях
 ≡ Наименование приложения
   Наименование приложения совпадает с наименованием корневой директории проекта
 ≡ Расположение исходного кода
   В поддиректории, совпадающей по названию с наименованием приложения
 ≡ Расположение и наименование основного запускаемого файла
   В корне проекта, в файле up.py
 ≡ Расположение и шаблоны наименований конфигурационных файлов
   В корне проекта с наименованием config с одним из расширений поддерживаемых типов
   Все файлы в поддиректории config или etc с любыми наименованиями и с одним из расширений поддерживаемых типов
 ≡ Сервисы и обработчики
   Активируется сервис HTTP c обработчиками служебных функций опроса состояния
 ≡ Средства контроля за исполнением
   Журналируется в стандартный вывод консоли события потока приложения и сервиса HTTP. Исключения в вывод ошибок консоли. Уровень логирования полный.

Шаблон файловой структуры проекта:

my_app
├── home
│   └── исходный код и тесты
├── etc
│   ├── файл версии данных
│   └── файлы конфигурации
├── var
│   ├── log
│   │   └── файлы конфигурации
│   ├── target
│   │   ├── deploy
│   │   │   └── собранный пакет приложения
│   │   └── docs
│   │       └── собранная документация
│   ├── test
│   │   └── файлы результатов тестирования
│   ├── run
│   │   ├── файлы-сокеты
│   │   └── файлы-замки
│   ├── tmp
│   │   └── любые временные файлы
│   ├── venv
│   │   └── интерпретатор и его окружение
│   └── файлы-замки
├── usr
│   ├── bin
│   │   └── скрипты сборки и вспомогательные
│   ├── docs
│   │   └── исходные файлы документации
│   ├── deploy
│   │   └── настройки выкладки
│   ├── etc
│   │   └── шаблонные файлы конфигурации
│   ├── <ide>
│   │   └── подмножество настроек, разделяемое разработчиками (словарь, игнорирование правил)
│   └── certs
│       └── ключи и сертификаты
├── opt
│   └── дополнительные ресурсы
├── .git
│   └── куда деваться
├── .<ide>
│   └── куда деваться, настройки среды разработки
├── up.py
├── файлы конфигурации пакета
├── файлы конфигурации среды (версионирование, редактор и т.д.)
└── первичные файлы документации

Пример минимального вида
my_app
├── home
│   ├── app.py
│   └── app.test.py
├── etc
│   └── main.json
├── var
│   ├── log
│   │   └── main.log
│   ├── test
│   │   └── файлы результатов тестирования
│   └── venv
│       └── интерпретатор и его окружение
├── .git
│   └── ..
├── .idea
│   └── ..
├── up.py
├── setup.py
├── .gitignore
└── requirements.txt

 ≡ Поддиректории var создаются автоматически приложением или инструментами
 ≡ Поддиректории var не версионируются, их можно сгенерировать заново программными средствами
 ≡ Содержимое поддиректории /etc не версионируется
 ≡ Поддиректория /<ide> не версионируется
 ≡ Тесты располагаются рядом с кодом или в отдельной поддиректории test внутри home
 ≡ Поддиректория home разбивается на поддиректории в зависимости от модульности пириложения


Project Conventions:
   ≡ project name is "cordio" or "cordiator" for development version
   ≡ python compatibility: ~3.6.3, 3.7.x, 3.8.x
   ≡ dependencies and tools are managed by poetry according to PEP-517 (pyproject.toml)
   ≡ coding style defined by black, flake8 and isort rules with string width set to 111 chars
   ≡ use setuptools entry points to start application
   ≡ semver, date-based (YYYYMMDD.xx, xx - order for the date) and git hash build versions tags
   ≡ numpy docsting format with doctest examples
   ≡ source code, docs (except for README), specs are located in <project>/src directory
   ≡ built applcation and docs are located in <project>/build directory
   ≡ run unit-tests for the whole python versions set, linting, styling, coverage, sloc on dev builds
   ≡ run the same as for dev plus auto doc render, changelog render, auto version set, deps conversion on prod build


Project management fallbacks:
   ≡ dependencies management fallback to requirements.txt with setup conversion tool
   ≡ "up" script in root, that detects this point and start it


Project setup:

 ✔ Stage One - Initialization @done

   ✔ Compose this to-do list @done
   ✔ Create venv (3.7.4) via pyenv @done
   ✔ Activate venv @done
   ✔ Do venv-install poetry @done
   ✔ Create new poetry project and recreate project file with init @done
   ✔ Make local venv link @done
   ✔ Compose start readme file @done
   ✔ Add 3.6.3 and 3.8-dev environments @done
   ✔ Add tox via poetry and make setup @done
   ✔ Check tox tests in cli @done
   ✔ Create pycharm interpreter venv setup for 3.7 and tox run config and check @done
   ✔ Move tests into code directory @done
   ✔ Create pycharm test run configuration @done
   ✔ Create editors config @done
   ✔ Create git ignore @done
   ✔ Init and push repo @done
   ✔ setup coverage report @done

   As this step completed we will have the following basic things:
   - project setup and structure using virtual environment
   - unit test and cross-py-version tests configured with coverage to use from cli & pycharm
   - vcs and editor setup
   - general description


 ✔ Stage Two - Quality & Style

   ✔ flake8 config path, line width, ignore @done
   ✔ cli flake8 @done
   ✔ pycharm flake8 @done
   ✔ black config path, ignore, line width @done
   ✔ cli black @done
   ✔ pycharm black @done
   ✔ isort @done
   ✔ setup xdoctest @done

   As this step completed we will have the following basic things:
   - flake8 check current file in pycharm (with hot key) or the whole project or part of it from cli
   - flake8 whole project with pytest
   - prettify import in current file in pycharm by hot key or for the whole project or part of it from cli
   - prettify code with black in current file in pycharm by hot key or for the whole project or part of it from cli
   - isort
   - run doctest with pytest
   

Stage Three - Documentation, Changes:

   ≡ compose 1st version of README well-formated in .rst
      ≡ title (name, description, target, author/owner)
      ≡ features - short read
   ≡ compose aux pages in .rst
      ≡ project structure and conventions
      ≡ toolkit & setup
      ≡ features - long read
   ≡ setup 3-type version bump with dephell
      ≡ semver
      ≡ date-based
      ≡ commit hash
   ≡ setup towncrier
   ≡ init setup sphinx
   ≡ setup source and destination and index.htm
   ≡ sphinx get version from pyproject
   ≡ include markdown into rst using m2r sphinx extension
   ≡ setup napoleon, numpydoc sphinx extensions
   ≡ build from index and readme
   ≡ add aux pages to doc build
   ≡ add changelog to doc build
   ≡ setup doctest and add autodoc with doctest to doc build
   ≡ setup viewcode extension
   ≡ setup and add todo pages
   ≡ setup intersphinx extension to link to other py projects
   ≡ push built doc to github pages with link in repo sub-title (add .nojekyll to ready doc root)
   ≡ try output to getsby
   ≡ try publish to gitlab pages

   As this step completed we will have the following basic things:
   - change-list from chunks of news files
   - doc from doc strings
   - build the complete doc with changes and readme
   - run tests from doc strings

   Doc Generation Refs:
   ≡ sphinx tldr: https://samnicholls.net/2016/06/15/how-to-sphinx-readthedocs/ 
   ≡ sphinx manual: https://pythonhosted.org/an_example_pypi_project/sphinx.html
   ≡ sphinx setup example: https://github.com/psf/requests/tree/master/docs
   ≡ numpydoc style official: https://numpydoc.readthedocs.io/en/latest/index.html
   ≡ numpydoc style example: https://sphinxcontrib-napoleon.readthedocs.io/en/latest/example_numpy.html#example-numpy
   ≡ comprehensive docstring style guide:  https://developer.lsst.io/python/numpydoc.html


 ≡ Stage Four - Release

   ≡ add up script
   ≡ add starting code and tests
   ≡ setup project management poetry commands
   ≡ setup git precommit hook
   ≡ publish

   As this step completed we will have the following basic things:
   ≡ script command to build all (deps/setup conversion, autoformat, check, docs generation, version bump, tags)
   ≡ script command to clean
   ≡ script command to push dev version
   ≡ script command to publish
   ≡ script command and shim to run project



   data checkers:
      https://github.com/alecthomas/voluptuous
      https://github.com/samuelcolvin/pydantic
      https://github.com/marshmallow-code/marshmallow
      https://github.com/schematics/schematics
      https://github.com/encode/typesystem

   optionall tools:
      https://pypi.org/project/pygount/ - to count line of codes
      https://github.com/baverman/flameprof - to profiling flamecharts


import os
import sys
sys.path.append(os.path.join(os.path.dirname(__name__), "../.."))


def get_version():
    import toml
    toml_path = os.path.join(os.path.dirname(__file__), "../../..", "pyproject.toml")

    with open(toml_path, "r") as fopen:
        pyproject = toml.load(fopen)

    return pyproject["tool"]["poetry"]["version"]


Sample for sphinx setup:

project = "cordiator"
copyright = "MMXIX, IPT"
author = "Tovva Kowalt"
version = get_version()
release = version
extensions = [
    "sphinx.ext.autodoc",
    "sphinx.ext.intersphinx",
    "sphinx.ext.todo",
    "sphinx.ext.mathjax",
    "sphinx.ext.ifconfig",
    "sphinx.ext.viewcode",
    "sphinx.ext.napoleon",
    "sphinx.ext.doctest",
    "numpydoc",
    "m2r",
]
autodoc_default_options = {"member-order": "bysource", "undoc-members": True}
source_suffix = ".rst"
templates_path = ["_templates"]
exclude_patterns = ["_build", "Thumbs.db", ".DS_Store"]
html_theme = "sphinx_rtd_theme"
html_theme_options = {"navigation_depth": -1}
html_static_path = ["_static"]
htmlhelp_basename = "cordiator"
master_doc = "index"


Cordiator minimal spec:
  ≡ pyenv/poetry driven project environment
  ≡ fastapi skeleton using asyncio, uvicorn
  ≡ '/healthcheck' http-handler depends on db-model, config services
  ≡ db-model service depends on db-adapter
  ≡ config service
