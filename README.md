

Создание Teamcity ВМ:
![alt text](image-11.png)

Srv:
![alt text](image-4.png)

Agent:
![alt text](image-5.png)

Создание Nexus:
![alt text](image-1.png)

![alt text](image-2.png)

![alt text](image-3.png)

Выполняем тестовый прогон кода:
![alt text](image-6.png)

Выполняем play:
![alt text](image-7.png)

![alt text](image-8.png)

![alt text](image-9.png)

![alt text](image-10.png)

Инициализируем внутреннюю БД для хранения ДАН Team City

![alt text](image-12.png)
![alt text](image-13.png)
![alt text](image-14.png)

Шаг 1: Создание проекта:
подключение конфигурации
![alt text](image-15.png)
![alt text](image-16.png)

Шаг 2: Подключаем в автоматическом режиме:
![alt text](image-17.png)

![alt text](image-18.png)
Статус подключения:   Connection to VCS repository verified.

Auto-Detect build step

![alt text](image-19.png)
Найден pom.xml

Подключим:
![alt text](image-20.png)

![alt text](image-21.png)

Шаг 3: Запустим первую сборку

Шаг 3.1:  Настроим агента.
Берем агента с сервера.
Помещаем в mkdir -p /opt/teamcity-agent
wget http://62.84.118.135:8111/update/buildAgent.zip
![alt text](image-22.png)

/opt/teamcity-agent
cd conf   
cp buildAgent.dist.properties buildAgent.properties
vim buildAgent.properties

![alt text](image-23.png)
![alt text](image-24.png)
![alt text](image-25.png)
![alt text](image-26.png)

Немного пришлось подумать по поводуподключения агентов, проверить логи со стороны агента и сервера.
Увидел что со стороны агента к серверу идут подключения с ID=2, однако на сервере агенты не отображались, пока не было исправлено публикация сервера по внешнему IP (раньше localhost был указан),
и произведен reboot.

![alt text](image-27.png)

Авторизуем:

![alt text](image-28.png)

Очередь, которая висела ранее = пуста )

![alt text](image-29.png)

![alt text](image-30.png)

Тесты прошли SUCESS.
![alt text](image-32.png)
![alt text](image-33.png)
Логи выгружены сюда:  [text](netology_example-teamcity-build_2.log)

Проект создан:
![alt text](image-31.png)

Задача 4:
"Поменяйте условия сборки: если сборка по ветке master, то должен происходит mvn clean deploy, иначе mvn clean test."

Переходим в настройки Build конфигурации:
![alt text](image-34.png)

Смотрю Buils Steps
![alt text](image-35.png)

Переходим к редактированию первого шага:
![alt text](image-36.png)
![alt text](image-37.png)
![alt text](image-38.png)

Добавляем условие:
![alt text](image-39.png)   teamcity.build.branch

![alt text](image-40.png)

Теперь этот шаг будет выполняться не для master

Сделаем шаг для Master'a
![alt text](image-41.png)

Выберем тип шага Maven
![alt text](image-42.png)

Создан шаг:
![alt text](image-43.png)

Найдены параметры Execution step:
![alt text](image-44.png)

Установки обновлены:
![alt text](image-45.png)

Указываем предустановки проекта:
![alt text](image-46.png)

Прописываем параметры execution step 2
![alt text](image-47.png)

Готов запускать deploy 
![alt text](image-48.png)

![alt text](image-49.png)

Логи:
![alt text](image-50.png)

Видно что step1 выполнился, второй нет. По параметрам сборки, заметил что параметр condition не существует. Укажу вместо teamcity.build.branch используем vcsroot.branch
 ![alt text](image-51.png)

Перезапуск:
![alt text](image-52.png)

Не указан правильно nexus. Fixit.
![alt text](image-53.png)
[text](netology_example-teamcity-build_5.log)

Не указан правильно пароль после смены. Fixit.
![alt text](image-54.png)
[text](netology_example-teamcity-build_6.log)

Сборка собралась без ошибок:
![alt text](image-55.png)
SUCCESS
[text](netology_example-teamcity-build_7.log)

Шаг 7:
Артефакт добыт =)
![alt text](image-56.png)

