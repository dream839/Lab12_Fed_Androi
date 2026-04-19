# Лабораторная работа №12. Жизненный цикл Activity (Activity Lifecycle)
Цель работы: изучить жизненный цикл Activity в Android и научиться правильно
обрабатывать переходы между состояниями. В результате лабораторной работы будет
создано интерактивное приложение "Grade Clicker" (Кликер оценок), которое
демонстрирует работу lifecycle callbacks и сохранение состояния при изменении
конфигурации устройства.

- Поесть
- Поспать
- [x] Выполнить работу по Android St

## Что мы изучили?
### Создали картинки с баллами)
[!Скриншот 1](<img width="881" height="845" alt="step7_grade_0_Fedorov" src="https://github.com/user-attachments/assets/db3d58b2-d067-4b7e-89dd-cda0dd95a3a1" />)
[!Скриншот 2](<img width="879" height="820" alt="step7_grade_50_Fedorov" src="https://github.com/user-attachments/assets/29e2ebee-d3d2-4629-872e-b26e112fb7f2" />)
[!Скриншот 3](<img width="909" height="919" alt="step7_grade_70_Fedorov" src="https://github.com/user-attachments/assets/6c7011b5-0fe6-427d-a05c-35b4f9668bc3" />)
[!Скриншот 4](<img width="878" height="911" alt="step7_grade_90_Fedorov" src="https://github.com/user-attachments/assets/51203965-c77b-4b6e-ad59-32b5bacfc42d" />)
[!Скриншот 5](<img width="901" height="893" alt="step7_grade_100_Fedorov" src="https://github.com/user-attachments/assets/50aa5525-c64e-453a-b163-a5832ce4b756" />)
### Создали источники данных в dimens.xml и string.xml
```xml
<resources>
    <dimen name="padding_medium">16dp</dimen>
    <dimen name="image_size">200dp</dimen>
</resources>
```
---
```xml
<string name="app_name">Grade_Clicker</string>
    <!-- Заголовки -->
    <string name="points_earned">Набрано баллов</string>
    <string name="total_clicks">Всего кликов</string>
    <string name="current_grade">Текущая оценка</string>
    <!-- Оценки -->
    <string name="grade_fail">Неудовлетворительно</string>
    <string name="grade_pass">Удовлетворительно</string>
    <string name="grade_good">Хорошо</string>
    <string name="grade_excellent">Отлично</string>
    <!-- Сообщение -->
    <string name="message_fail">Продолжайте работать!</string>
    <string name="message_pass">Вы получили зачёт!</string>
    <string name="message_good">Отличная работа!</string>
    <string name="message_excellent">Вы успешно сдали курс!</string>
    <!-- Поделиться -->
    <string name="share">Поделиться</string>
    <string name="share_text">Я набрал %1$d баллов по курсу\"Разработка мобильных приложений\"!
    #GradeClicker</string>
    <string name="sharing_not_available">Функция недоступна</string>
```
---
### Настроили Color.kt и Theme.kt
```kotlin
val md_theme_light_primary = Color(0xFF1976D2)
val md_theme_light_onPrimary = Color(0xFFFFFFFF)
val md_theme_light_secondaryContainer = Color(0xFFE3F2FD)
val md_theme_light_onSecondaryContainer = Color(0xFF0D47A1)
val md_theme_light_background = Color(0xFFFAFAFA)

val md_theme_dark_primary = Color(0xFF64B5F6)
val md_theme_dark_onPrimary = Color(0xFF0D47A1)
val md_theme_dark_secondaryContainer = Color(0xFF1565C0)
val md_theme_dark_onSecondaryContainer = Color(0xFFE3F2FD)
val md_theme_dark_background = Color(0xFF121212)
```
---
```kotlin
private val DarkColorScheme = darkColorScheme(
    primary = md_theme_dark_primary,
    onPrimary = md_theme_dark_onPrimary,
    secondaryContainer = md_theme_dark_secondaryContainer,
    onSecondaryContainer = md_theme_dark_onSecondaryContainer,
    background = md_theme_dark_background
)
private val LightColorScheme = lightColorScheme(
    primary = md_theme_light_primary,
    onPrimary = md_theme_light_onPrimary,
    secondaryContainer = md_theme_light_secondaryContainer,
    onSecondaryContainer = md_theme_light_onSecondaryContainer,
    background = md_theme_light_background
)
```
---
### Создали модели данных
```kotlin
data class Grade(
    @DrawableRes val imageId: Int,
    val pointsPerClick: Int,
    val threshold: Int
)
```
**...**

## Ответ на контрольные вопросы:
- *Какие три метода вызываются при запуске приложения?*
**Activity: onCreate(), onStart() и onResume().**

- *Чем отличается onPause() от onStop()?*
**onPause() вызывается когда Activity частично теряет видимость - сохраняйте данные и останавливайте анимации**
**onStop() вызывается когда Activity полностью скрыта от пользователя - освобождайте ресурсы.**

- *Когда вызывается onRestart()?*
**`onRestart()` вызывается когда Activity из состояния `STOPPED` возвращается в видимое состояние.**

- *Что происходит при повороте экрана?*
**При повороте экрана Activity пересоздается, вызывается `onDestroy()` -> `onCreate()`. Состояние теряется, если не сохранено.**

- *В чём разница между remember и rememberSaveable?*
**remember - Сбрасывается (сохраняется только в памяти Compose)**
**rememberSaveable - Сохраняется (использует `Bundle` как `onSaveInstanceState`)**

- *Почему onCreate() должен вызывать super.onCreate()?*
  **`super.onCreate()` выполняет системную инициализацию Activity: создает view hierarchy, восстанавливает состояние из `savedInstanceState`, настраивает системные callbacks. Без него Activity не заработает.**

- *Для чего используется класс Log?*
**`Log.d(TAG, "message")` — выводит отладочные сообщения в Logcat (Android Studio).**
**`TAG` = `"MainActivity"` идентифицирует логи конкретного класса. Используется для debugging жизненного цикла.**

- *Какой метод вызывается только один раз за время жизни Activity?*
**`onCreate()` вызывается только один раз за жизненный цикл Activity**

## *Федоров Михаил Дмитриевич ИСП-233*
