# propellerads-championship

Проект, занявший первое место

[Описание задания](https://hub.docker.com/repository/docker/qapropeller/qa-battle)

## Описание проекта ##

### Основные компоненты: ###

* TestNG
* Selenium WebDriver
* Atlas
* Allure
* Owner

### Описание слоёв ###

| Слой       | Путь/пакет     | Назначение |
| :------------- | :----------: | :----------: |
| test cases layer                 | /src/test/                              | Непосредственно тесты и фикстуры|
| domain layer                     | com.github.savkk.propeller.steps        | Шаги, описывающие бизнес действия и действия по работе с элементами|
| page layer                       | com.github.savkk.propeller.pages        | Описание страниц|
| elements layer                   | com.github.savkk.propeller.elements     | Описание кастомных элементов|
| layout layer                     | com.github.savkk.propeller.layout       | Описание компоновок элементов|
| configuration layer              | com.github.savkk.propeller.config       | Внешние параметры, используемые в тестах|

### Правила работы с проектом: ###

1. Тестовый слой должен вызвать методы только из слоя шагов
2. Слой шагов должен общаться только с уровнем описания страниц и непосредственно с WebDriver'ом
3. Страницы содержат только описание элементов и компоновки элементов, никаких действий страница не должна содержать
4. Нельзя непосредственно в тестах вызвать методы webElement'ов

### Конфигурация ### 
 
Проект содержит 4 файла конфигурации:
* aut.properties - содержит url и порт приложения. Также есть возможность запускать приложение в TestContainers
* credentials.properties - содержит явки/пароли
* timeouts.properties - настройка таймаутов
* webdriver.properties - настройки веб-драйвера (бинарный файл скачивается при помощи WebDriverManager)

Для доступа к конфигам используется библиотека Owner, часть настроек можно переопределить через System.env() и System.properties()

### Запуск тестов ###

```
mvn clean test
```

### Формирование отчета ###

```
mvn allure:serve
```

## Баги ##
### Страница логина ###
1. В поле ввода электронной почты, можно вводить любое значение. Нет валидации.
2. При вводе некоретных логина и пароля не происходит никаких действий, предупреждающих пользователя о вводе некорректных данных, форма логина просто исчезает.
3. Название страницы (title) содержит ошибку - "Welcom to Propeller Automated Testing Championship"

### Страница информации о существующих клиентах ###
1. При сохранении статья исчезает из раздела
2. Статьи можно сохранять сколько угодно раз, при это в куки добавляются каждый раз новые записи о сохранении
3. Отсутствует название страницы (title)

### Страница профиля пользователя ###
1. Отсутствует название страницы (title)
2. Если оставить пустыми поля First Name и Last Name и нажать на кнопку Save user info, отобразится предупреждение, что о необходимости заполнения только поля First Name
3. Не исчезает предупреждение под полем Last Name, если его заполнить, а потом очистить
4. Отсутствует валидация значения в поле Card Number
5. Если не заполнять информацию о платежной системе и нажать кнопку Save payment info, то появится ошибка только о необходимости заполнить поле с номером карты.
6. Не отображается начальное значение выбранного дня платежа
7. Не исчезает предупреждение под полем Choose your payment system, если его заполнить, а потом очистить
8. Какой-то подозрительный диапазон в дне платежа с 1 до 31, не учитывается длительность месяца :-\