# Автоматизация UI-тестирования сервиса «Яндекс.Самокат»

![Java](https://img.shields.io/badge/Java-11-orange?logo=java)
![Maven](https://img.shields.io/badge/Maven-Build-blue?logo=apache-maven)
![JUnit4](https://img.shields.io/badge/JUnit-4.13.2-red)
![Selenium](https://img.shields.io/badge/Selenium-WebDriver-43B02A?logo=selenium)
![Chrome](https://img.shields.io/badge/Browser-Chrome-blue?logo=googlechrome)
![Firefox](https://img.shields.io/badge/Browser-Firefox-orange?logo=firefoxbrowser)

Проект по автоматизации регрессионного UI-тестирования веб-сервиса «Яндекс.Самокат» (`qa-scooter.praktikum-services.ru`) с использованием Selenium WebDriver, JUnit 4 и паттерна Page Object Model (POM).

---

## 📌 Архитектура и технические особенности

- **Page Object Model (POM):**
  - `BasePage` — базовый класс с инкапсуляцией общих методов ожидания (`waitForElementClickable`, `waitForElementVisible`, `clickWithWait`) и скролла через JavaScript (`JavascriptExecutor`).
  - `MainPage` — работа с элементами главной страницы (аккордеон FAQ, верхняя и нижняя кнопки «Заказать»).
  - `OrderPage` — логика двухшаговой формы заказа («Для кого самокат», «Про аренду») и подтверждения в модальном окне.
- **PageFactory:** Инициализация веб-элементов страниц через `@FindBy` и `PageFactory.initElements()`.
- **Параметризация JUnit 4 (`@Parameterized`):**
  - Тестовые данные полностью отделены от логики проверок и вынесены в класс `TestData`.
  - Тест аккордеона проверяет все 8 вопросов и ответов по индексам.
  - Тесты оформления заказа запускаются на двух независимых наборах данных клиентов (с разными сроками аренды, адресами и цветами).
- **Кроссбраузерный запуск (`BrowserSetting`):**
  - Поддержка запуска в Google Chrome и Mozilla Firefox через системное свойство `-Dbrowser`.
  - Автоматическое управление бинарниками браузеров с помощью `WebDriverManager`.
  - В тесты заложена обработка известного дефекта верстки в Google Chrome при выборе станции метро.

---

## 🧪 Реализованные тесты

### 1. `FAQTests` (Аккордеон «Вопросы о важном»)
* Параметризованный прогон по 8 вопросам (`TestData.FAQ_DATA`).
* Скролл до вопроса по динамическому идентификатору `accordion__heading-{index}`.
* Клик по вопросу и проверка соответствия текста открывшегося ответа в `accordion__panel-{index}`.

### 2. `OrderTests` (Флоу оформления заказа)
* Параметризованный прогон на 2 наборах данных (`TestData.ORDER_DATA`).
* **Точка входа 1 (`createOrderFromTopButton`):** старт оформления через верхнюю кнопку «Заказать» в шапке сайта (`Header_Nav`).
* **Точка входа 2 (`createOrderFromBottomButton`):** старт оформления через нижнюю кнопку «Заказать» в теле страницы (`Home_FinishButton`) со скроллом.
* **Сквозной сценарий:**
  1. Заполнение первой формы: имя, фамилия, адрес, выбор станции метро из выпадающего списка, телефон.
  2. Заполнение второй формы: дата доставки, срок аренды, выбор цвета самоката (`Чёрный жемчуг` / `Серая безысходность`), комментарий.
  3. Подтверждение заказа в модальном окне и проверка отображения заголовка об успешном создании заказа.

---

## 🛠 Стек технологий

* **Язык:** Java 11
* **Система сборки:** [Apache Maven](https://maven.apache.org/)
* **Тестовый фреймворк:** [JUnit 4](https://junit.org/junit4/)
* **Фреймворк автоматизации:** [Selenium WebDriver](https://www.selenium.dev/)
* **Управление драйверами:** [WebDriverManager](https://github.com/bonigarcia/webdrivermanager)
