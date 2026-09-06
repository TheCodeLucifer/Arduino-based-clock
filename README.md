# Arduino-based Clock

**Language / Мова:** English / Українська  
**Platform / Платформа:** Arduino  
**RTC:** DS3231  
**Display / Дисплей:** 20×4 LCD with I2C backpack  
**Storage / Пам'ять:** Arduino EEPROM  
**Alarm / Будильник:** Yes / Так

---

<!-- QUICK NAVIGATION -->
<p align="center">
  <a href="#-english">🇬🇧 English</a> •
  <a href="#-українська">🇺🇦 Українська</a> •
  <a href="#hardware">🔧 Hardware</a> •
  <a href="#wiring-schematics">🔌 Schematics</a> •
  <a href="#required-libraries">📚 Libraries</a> •
  <a href="#installing-the-arduino-ide">💻 Installation</a> •
  <a href="#setting-the-clock-in-the-code">🕐 Clock Setup</a> •
  <a href="#setting-the-clock-using-buttons">🎛️ Buttons</a> •
  <a href="#alarm">⏰ Alarm</a> •
  <a href="#troubleshooting">🛠️ Troubleshooting</a>
</p>

<p align="center">
  <a href="#-english">🇬🇧 English Guide</a>
  &nbsp;&nbsp;|&nbsp;&nbsp;
  <a href="#-українська">🇺🇦 Українська Інструкція</a>
</p>

---

## 🇬🇧 English

### Overview

**Arduino-based Clock** is a digital clock built around an Arduino board, a DS3231 real-time clock (RTC), and a 20×4 I2C LCD.

The project provides:

- 24-hour time display with seconds.
- Date display in `DD-MM-YYYY` format.
- Day of the week display.
- DS3231 RTC for keeping accurate time.
- Alarm clock with configurable hour and minute.
- Alarm settings stored in Arduino EEPROM.
- Three-button control:
  - `PLUS` — increase a value.
  - `MINUS` — decrease a value.
  - `SELECT` — move to the next setting / confirm with a long press.
- Automatic LCD backlight shutdown from **00:00 to 05:59**.
- Buzzer alarm with an intermittent 2700 Hz tone.
- Custom large digits rendered using LCD character cells.

---

<a id="hardware"></a>
## Hardware

The exact component-to-pin wiring should be documented in the project schematics.

### Required components

- Arduino board compatible with the sketch.
- DS3231 RTC module.
- 20×4 LCD with I2C backpack.
- 3 push buttons.
- Buzzer / piezo speaker.
- Connecting wires.
- Breadboard or suitable permanent wiring.
- USB cable for programming the Arduino.

> **Important:** The sketch uses the following Arduino pins:
>
> | Component | Arduino pin |
> |---|---:|
> | PLUS button | D2 |
> | MINUS button | D3 |
> | SELECT button | D4 |
> | Buzzer | D5 |
> | I2C devices | SDA / SCL |

The I2C pins depend on the Arduino board. On common Arduino boards such as the Uno/Nano, I2C is normally connected to **A4 (SDA)** and **A5 (SCL)**.

---

<a id="wiring-schematics"></a>
# Wiring Schematics

## Schematic 1 — Main Clock Circuit

> **Insert the first wiring diagram here.**

```text
[ IMAGE: docs/schematic-main.png ]
```

### Component connection description

Write the complete connection table here.

| Component | Pin | Arduino |
|---|---|---|
| DS3231 | VCC | 5V |
| DS3231 | GND | GND |
| DS3231 | SDA | SDA |
| DS3231 | SCL | SCL |
| LCD I2C | VCC | 5V |
| LCD I2C | GND | GND |
| LCD I2C | SDA | SDA |
| LCD I2C | SCL | SCL |
| PLUS button | Signal | D2 |
| MINUS button | Signal | D3 |
| SELECT button | Signal | D4 |
| Buzzer | Signal | D5 |
| Buzzer | GND | GND |

**Note:** The buttons use Arduino's internal pull-up resistors (`INPUT_PULLUP`). The button should therefore connect the corresponding input pin to **GND when pressed**.

---

## Schematic 2 — Alternative / Detailed Wiring

> **Insert the second wiring diagram here.**

```text
[ IMAGE: docs/schematic-detailed.png ]
```

### Detailed connection description

Add the complete explanation for the second schematic here.

Describe:

1. Power connections.
2. I2C connections.
3. Button connections.
4. Buzzer connection.
5. Any resistors or additional components.
6. Ground connections.
7. Any board-specific differences.

---

<a id="required-libraries"></a>
# Required Libraries

The sketch includes the following libraries:

```cpp
#include <Wire.h>
#include <LiquidCrystal_I2C.h>
#include <DS3231.h>
#include <EEPROM.h>
```

### 1. Wire

`Wire` is the standard Arduino I2C communication library.

It is normally included with the Arduino IDE and does **not** need to be installed separately.

```cpp
#include <Wire.h>
```

### 2. LiquidCrystal_I2C

This library controls the 20×4 LCD through its I2C backpack.

The sketch uses:

```cpp
LiquidCrystal_I2C lcd(0x27, 20, 4);
```

This means:

- I2C address: `0x27`
- LCD columns: `20`
- LCD rows: `4`

If your LCD uses another I2C address, change `0x27` in the sketch.

Common LCD addresses are `0x27` and `0x3F`.

### 3. DS3231

The project uses the **DS3231 Arduino library by jarzebski**.

The original sketch references:

```text
https://github.com/jarzebski/Arduino-DS3231
```

The sketch uses the following DS3231 objects:

```cpp
DS3231 clock;
RTCDateTime DateTime;
```

### 4. EEPROM

`EEPROM` is used to save alarm settings so they remain available after the Arduino is powered off.

It is normally included with the Arduino AVR core and does not require a separate installation on compatible AVR Arduino boards.

---

<a id="installing-the-arduino-ide"></a>
# Installing the Arduino IDE

1. Install the Arduino IDE.
2. Connect the Arduino board to the computer using USB.
3. Open the Arduino IDE.
4. Open the file:

```text
sbudilnikom.ino
```

5. Select the correct board:

```text
Tools → Board
```

6. Select the correct USB/serial port:

```text
Tools → Port
```

7. Install the required libraries.
8. Verify/compile the sketch.
9. Upload it to the Arduino.

---

# Installing Libraries

### LiquidCrystal_I2C

Open:

```text
Sketch → Include Library → Manage Libraries
```

Search for:

```text
LiquidCrystal I2C
```

Install a compatible `LiquidCrystal_I2C` library.

Make sure the installed library provides:

```cpp
#include <LiquidCrystal_I2C.h>
```

### DS3231

Install the DS3231 library by **jarzebski**.

Repository:

```text
https://github.com/jarzebski/Arduino-DS3231
```

Download the repository and install the library using:

```text
Sketch → Include Library → Add .ZIP Library...
```

Then select the downloaded ZIP file.

### Built-in libraries

The following are normally already available:

```cpp
Wire
EEPROM
```

---

# Uploading the Sketch

Before uploading:

1. Connect the Arduino via USB.
2. Select the correct board.
3. Select the correct processor if your board requires it.
4. Select the correct COM/serial port.
5. Open `sbudilnikom.ino`.
6. Click **Verify**.
7. If compilation succeeds, click **Upload**.

After uploading, the LCD should initialize and the clock should start working.

---

# LCD I2C Address

The current sketch uses:

```cpp
LiquidCrystal_I2C lcd(0x27, 20, 4);
```

If the LCD is blank, but the backlight works, the I2C address may be different.

For example:

```cpp
LiquidCrystal_I2C lcd(0x3F, 20, 4);
```

Change the address only if your LCD module uses a different I2C address.

---

<a id="setting-the-clock-in-the-code"></a>
# Setting the Clock in the Code

The clock is normally set using the buttons. However, the DS3231 can also be initialized directly from the sketch.

The RTC is controlled through:

```cpp
DS3231 clock;
RTCDateTime DateTime;
```

The time is written using:

```cpp
clock.setDateTime(
    year,
    month,
    day,
    hour,
    minute,
    second
);
```

For example:

```cpp
clock.setDateTime(2026, 9, 6, 15, 30, 0);
```

This sets:

```text
Year:   2026
Month:  09
Day:    06
Time:   15:30:00
```

### Important

The current version of the sketch does **not** automatically call `setDateTime()` during startup. This is intentional: the DS3231 keeps the previously stored time.

If you add a `clock.setDateTime(...)` call to `setup()`, remember that the RTC will be reset to that value every time the Arduino starts.

For normal operation, it is recommended to set the time through the button menu.

---

<a id="setting-the-clock-using-buttons"></a>
# Setting the Clock Using Buttons

The clock has three buttons:

| Button | Function |
|---|---|
| `PLUS` | Increase value |
| `MINUS` | Decrease value |
| `SELECT` | Next item |
| Long `SELECT` | Enter / save settings |

## Entering the settings menu

On the normal clock screen:

1. Press and hold **SELECT** for approximately **2 seconds**.
2. The settings menu will open.
3. The first item is:

```text
Set Hour:
```

## Setting the hour

Press:

- `PLUS` → increase hour.
- `MINUS` → decrease hour.

Range:

```text
00–23
```

Press `SELECT` briefly to move to minutes.

## Setting the minutes

Range:

```text
00–59
```

Use `PLUS` and `MINUS`.

Press `SELECT` to continue.

## Setting the day

Range implemented by the sketch:

```text
01–31
```

Press `SELECT` to continue.

> The current sketch does not calculate the actual number of days in each month. For example, it is possible to select day `31` for a month that has only 30 days. Enter a valid calendar date manually.

## Setting the month

Range:

```text
01–12
```

Press `SELECT` to continue.

## Setting the year

The menu displays the full year, while internally the sketch stores the last two digits.

Example:

```text
Set Year: 2026
```

The selectable internal range is:

```text
2000–2099
```

## Setting the alarm hour

Range:

```text
00–23
```

## Setting the alarm minute

Range:

```text
00–59
```

## Enabling or disabling the alarm

The final setting is:

```text
Alarm: ON
```

or:

```text
Alarm: OFF
```

Use `PLUS` or `MINUS` to switch between the two states.

---

# Saving Settings

After configuring all settings:

1. Hold **SELECT** for approximately **2 seconds**.
2. The settings will be saved.
3. The clock will return to the main screen.
4. The display will show:

```text
Settings Saved
```

The alarm hour, alarm minute, and alarm enabled/disabled state are stored in EEPROM.

The clock date and time are stored directly in the DS3231 RTC.

---

<a id="alarm"></a>
# Alarm

The alarm can be enabled or disabled from the settings menu.

The alarm time is displayed on the main screen when enabled:

```text
-- Alarm: 07:00 --
```

When the current time reaches the configured alarm time and the seconds equal `00`, the alarm starts.

Example:

```text
07:00:00
```

The LCD displays:

```text
*** WAKE UP! ***
```

The buzzer alternates on/off every 500 ms.

The tone frequency is:

```text
2700 Hz
```

---

# Stopping the Alarm

Press **any of the three buttons**:

- `PLUS`
- `MINUS`
- `SELECT`

The buzzer will stop and the alarm screen will close.

---

# EEPROM Alarm Storage

The project uses three EEPROM addresses:

```cpp
const int EEPROM_ADDR_ALARM_HOUR = 0;
const int EEPROM_ADDR_ALARM_MIN  = 1;
const int EEPROM_ADDR_ALARM_EN   = 2;
```

They store:

| Address | Value |
|---:|---|
| `0` | Alarm hour |
| `1` | Alarm minute |
| `2` | Alarm status |

`EEPROM.update()` is used so the EEPROM is not rewritten when the value has not changed.

---

# Automatic Backlight

The LCD backlight is automatically disabled between:

```text
00:00
```

and

```text
05:59
```

At other times the backlight is enabled.

The relevant condition is:

```cpp
if (DateTime.hour >= 0 && DateTime.hour < 6) {
    lcd.noBacklight();
} else {
    lcd.backlight();
}
```

---

# Day of the Week

The sketch calculates the day of the week using the **Zeller algorithm**.

The displayed names are transliterated Ukrainian names:

```text
Subota
Nedilya
Ponedilok
Vivtorok
Sereda
Chetver
Pyatnytsya
```

The function is:

```cpp
getDayOfWeek(year, month, day)
```

---

# Configuration Summary

The main hardware configuration is defined near the beginning of the sketch:

```cpp
const int BTN_PLUS = 2;
const int BTN_MINUS = 3;
const int BTN_SELECT = 4;
const int BUZZER_PIN = 5;

LiquidCrystal_I2C lcd(0x27, 20, 4);
```

Change these values if your wiring is different.

---

<a id="troubleshooting"></a>
# Troubleshooting

### LCD does not display text

Check:

1. LCD power.
2. GND connection.
3. SDA/SCL connections.
4. I2C address.
5. LCD contrast potentiometer.
6. Installed `LiquidCrystal_I2C` library.

Try:

```cpp
0x27
```

or:

```cpp
0x3F
```

### DS3231 is not working

Check:

- VCC.
- GND.
- SDA.
- SCL.
- RTC module battery.
- Correct DS3231 library.

### Buttons do not work

The buttons use:

```cpp
INPUT_PULLUP
```

Therefore each button should connect its Arduino input pin to **GND when pressed**.

The assigned pins are:

```text
PLUS   → D2
MINUS  → D3
SELECT → D4
```

### Alarm does not sound

Check:

- Buzzer wiring.
- Buzzer polarity if applicable.
- `BUZZER_PIN`.
- Alarm status is `ON`.
- Alarm time is correct.
- The DS3231 time is correct.

---

# Project Structure

Recommended repository structure:

```text
Arduino-based-clock/
├── sbudilnikom.ino
├── README.md
└── docs/
    ├── schematic-main.png
    └── schematic-detailed.png
```

---

# License

Add your preferred license here.

Example:

```text
MIT License
```

---

---

# 🇺🇦 Українська

## Опис проєкту

**Arduino-based Clock** — цифровий годинник на базі Arduino, модуля реального часу **DS3231** та LCD-дисплея **20×4 з I2C**.

Проєкт підтримує:

- відображення часу у форматі 24 години;
- відображення секунд;
- відображення дати у форматі `DD-MM-YYYY`;
- відображення дня тижня;
- збереження точного часу за допомогою DS3231;
- будильник із налаштуванням годин і хвилин;
- збереження налаштувань будильника в EEPROM;
- керування трьома кнопками;
- автоматичне вимкнення підсвічування LCD з **00:00 до 05:59**;
- звуковий сигнал будильника;
- великі кастомні цифри на LCD.

---

# Необхідні компоненти

- Arduino-сумісна плата.
- Модуль RTC DS3231.
- LCD 20×4 з I2C-перехідником.
- 3 кнопки.
- Пищалка / бузер.
- Проводи.
- Макетна плата або постійне з'єднання.
- USB-кабель для прошивання Arduino.

## Використані піни Arduino

| Компонент | Пін Arduino |
|---|---:|
| Кнопка PLUS | D2 |
| Кнопка MINUS | D3 |
| Кнопка SELECT | D4 |
| Buzzer | D5 |
| I2C | SDA / SCL |

Для Arduino Uno/Nano I2C зазвичай використовується:

```text
A4 → SDA
A5 → SCL
```

---

# Схеми підключення

## Схема 1 — Основна схема годинника

> **Вставити першу схему підключення сюди.**

```text
[ IMAGE: docs/schematic-main.png ]
```

### Опис підключення

Тут буде розміщено повний опис того, який компонент до якого піна підключається.

| Компонент | Пін | Arduino |
|---|---|---|
| DS3231 | VCC | 5V |
| DS3231 | GND | GND |
| DS3231 | SDA | SDA |
| DS3231 | SCL | SCL |
| LCD I2C | VCC | 5V |
| LCD I2C | GND | GND |
| LCD I2C | SDA | SDA |
| LCD I2C | SCL | SCL |
| PLUS | Signal | D2 |
| MINUS | Signal | D3 |
| SELECT | Signal | D4 |
| Buzzer | Signal | D5 |
| Buzzer | GND | GND |

Кнопки використовують внутрішні підтягувальні резистори Arduino:

```cpp
INPUT_PULLUP
```

Тому кнопка повинна з'єднувати відповідний цифровий пін Arduino із **GND при натисканні**.

---

## Схема 2 — Детальна / альтернативна схема

> **Вставити другу схему підключення сюди.**

```text
[ IMAGE: docs/schematic-detailed.png ]
```

### Опис схеми

Тут буде розміщено повний опис другої схеми.

Необхідно описати:

1. Підключення живлення.
2. Підключення SDA/SCL.
3. Підключення кнопок.
4. Підключення бузера.
5. Додаткові резистори, якщо вони використовуються.
6. Спільні GND.
7. Відмінності для конкретної плати Arduino.

---

# Необхідні бібліотеки

У скрипті використовуються:

```cpp
#include <Wire.h>
#include <LiquidCrystal_I2C.h>
#include <DS3231.h>
#include <EEPROM.h>
```

## Wire

Стандартна бібліотека Arduino для роботи з I2C.

Окремо встановлювати її зазвичай не потрібно.

```cpp
#include <Wire.h>
```

## LiquidCrystal_I2C

Використовується для керування LCD через I2C.

У коді:

```cpp
LiquidCrystal_I2C lcd(0x27, 20, 4);
```

Тут:

- `0x27` — I2C-адреса дисплея;
- `20` — кількість колонок;
- `4` — кількість рядків.

Якщо ваш дисплей має іншу адресу, наприклад `0x3F`, змініть:

```cpp
LiquidCrystal_I2C lcd(0x27, 20, 4);
```

на:

```cpp
LiquidCrystal_I2C lcd(0x3F, 20, 4);
```

## DS3231

Використовується бібліотека **DS3231 by jarzebski**.

Репозиторій:

```text
https://github.com/jarzebski/Arduino-DS3231
```

У проєкті використовуються:

```cpp
DS3231 clock;
RTCDateTime DateTime;
```

## EEPROM

EEPROM використовується для збереження налаштувань будильника після вимкнення живлення.

На сумісних Arduino AVR ця бібліотека зазвичай вже входить до Arduino core.

---

# Встановлення Arduino IDE

1. Встановіть Arduino IDE.
2. Підключіть Arduino через USB.
3. Відкрийте Arduino IDE.
4. Відкрийте:

```text
sbudilnikom.ino
```

5. Виберіть плату:

```text
Tools → Board
```

6. Виберіть правильний COM-порт:

```text
Tools → Port
```

7. Встановіть необхідні бібліотеки.
8. Виконайте перевірку компіляції.
9. Завантажте скетч на Arduino.

---

# Встановлення бібліотек

Для `LiquidCrystal_I2C` відкрийте:

```text
Sketch → Include Library → Manage Libraries
```

Знайдіть:

```text
LiquidCrystal I2C
```

та встановіть сумісну бібліотеку.

Для DS3231 встановіть бібліотеку **jarzebski/Arduino-DS3231**.

ZIP-архів можна встановити через:

```text
Sketch → Include Library → Add .ZIP Library...
```

`Wire` та `EEPROM` зазвичай уже доступні в Arduino IDE.

---

# Завантаження прошивки

Перед завантаженням:

1. Підключіть Arduino до USB.
2. Виберіть правильну плату.
3. Виберіть процесор, якщо це потрібно для вашої плати.
4. Виберіть COM-порт.
5. Відкрийте `sbudilnikom.ino`.
6. Натисніть **Verify**.
7. Після успішної компіляції натисніть **Upload**.

---

# Налаштування часу через код

DS3231 встановлюється за допомогою:

```cpp
clock.setDateTime(
    year,
    month,
    day,
    hour,
    minute,
    second
);
```

Наприклад:

```cpp
clock.setDateTime(2026, 9, 6, 15, 30, 0);
```

Це встановить:

```text
Рік:       2026
Місяць:    09
День:      06
Час:       15:30:00
```

### Важливо

Поточна версія скрипта **не встановлює час автоматично під час кожного запуску Arduino**. DS3231 продовжує зберігати час самостійно.

Якщо додати `clock.setDateTime(...)` у `setup()`, цей час буде встановлюватися заново після кожного перезапуску Arduino.

Для звичайного використання рекомендується встановлювати час через меню кнопок.

---

# Налаштування годинника кнопками

Використовуються три кнопки:

| Кнопка | Функція |
|---|---|
| `PLUS` | Збільшити значення |
| `MINUS` | Зменшити значення |
| `SELECT` | Перейти далі |
| Довге `SELECT` | Увійти / зберегти |

## Вхід у меню

На головному екрані:

1. Натисніть і утримуйте **SELECT приблизно 2 секунди**.
2. Відкриється меню.
3. Перше поле:

```text
Set Hour:
```

## Година

Кнопки:

```text
PLUS  → +1
MINUS → -1
```

Діапазон:

```text
00–23
```

Коротке натискання `SELECT` переходить до хвилин.

## Хвилини

Діапазон:

```text
00–59
```

## День

Діапазон, реалізований у поточному скрипті:

```text
01–31
```

> Скрипт не перевіряє фактичну кількість днів у конкретному місяці. Тому користувач може вибрати, наприклад, 31-й день для місяця, у якому лише 30 днів. Необхідно вводити коректну календарну дату.

## Місяць

Діапазон:

```text
01–12
```

## Рік

У меню відображається повний рік:

```text
Set Year: 2026
```

Доступний діапазон:

```text
2000–2099
```

## Година будильника

Діапазон:

```text
00–23
```

## Хвилина будильника

Діапазон:

```text
00–59
```

## Увімкнення будильника

Останній пункт:

```text
Alarm: ON
```

або:

```text
Alarm: OFF
```

`PLUS` та `MINUS` перемикають стан.

---

# Збереження налаштувань

Після налаштування всіх параметрів:

1. Натисніть і утримуйте `SELECT` приблизно 2 секунди.
2. Налаштування буде збережено.
3. Годинник повернеться на головний екран.
4. З'явиться:

```text
Settings Saved
```

Налаштування будильника зберігаються в EEPROM:

- година;
- хвилина;
- увімкнений/вимкнений стан.

Дата і час зберігаються в модулі DS3231.

---

# Будильник

Якщо будильник увімкнений, на головному екрані відображається:

```text
-- Alarm: 07:00 --
```

Коли поточний час дорівнює встановленому часу будильника і секунди дорівнюють `00`, будильник запускається.

Наприклад:

```text
07:00:00
```

На дисплеї:

```text
*** WAKE UP! ***
```

Бузер працює переривчасто:

```text
500 ms ON
500 ms OFF
```

Частота:

```text
2700 Hz
```

---

# Вимкнення будильника

Для вимкнення будильника достатньо натиснути будь-яку кнопку:

```text
PLUS
MINUS
SELECT
```

---

# Автоматичне підсвічування

Підсвічування LCD автоматично вимикається з:

```text
00:00
```

до:

```text
05:59
```

В інший час підсвічування увімкнене.

Це реалізовано в:

```cpp
if (DateTime.hour >= 0 && DateTime.hour < 6) {
    lcd.noBacklight();
} else {
    lcd.backlight();
}
```

---

# Структура репозиторію

Рекомендована структура GitHub-репозиторію:

```text
Arduino-based-clock/
├── sbudilnikom.ino
├── README.md
└── docs/
    ├── schematic-main.png
    └── schematic-detailed.png
```

---

# License / Ліцензія

Додайте необхідну ліцензію для проєкту.

Наприклад:

```text
MIT License
```
