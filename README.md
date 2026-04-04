# nisejjy
ESP32/Arduino fake radio clock station

emulates the following radio clock stations:

* JJY (Japan): https://ja.wikipedia.org/wiki/JJY
* WWVB (US): https://en.wikipedia.org/wiki/WWVB
	* set UT for WWVB.
* DCF77 (Germany) : https://www.eecis.udel.edu/~mills/ntp/dcf77.html
* (HBG (Switzerland) discon: https://msys.ch/decoding-time-signal-stations )
* BSF (Taiwan): https://en.wikipedia.org/wiki/BSF_(time_service)
* MSF (UK): https://en.wikipedia.org/wiki/Time_from_NPL_(MSF)
* BPC (China): https://harmonyos.51cto.com/posts/1731

## Build Notes

### Partition Scheme

Due to the large code size, you must change the partition scheme to **Huge APP (3MB No OTA/1MB SPIFFS)** in Arduino IDE, otherwise compilation will fail.

Arduino IDE menu: Tools → Partition Scheme → Huge APP (3MB No OTA/1MB SPIFFS)

### Arduino Core v3 (ESP-IDF v5) Compatibility

The hardware timer API changed in Arduino Core v3. This code has been updated accordingly:

| | Arduino Core v2 | Arduino Core v3 |
|---|---|---|
| Timer init | `timerBegin(0, prescaler, true)` | `timerBegin(frequency_hz)` |
| Attach interrupt | `timerAttachInterrupt(tm0, &onTimer, true)` | `timerAttachInterrupt(tm0, &onTimer)` |
| Alarm/start | `timerAlarmWrite()` + `timerAlarmEnable()` | `timerAlarm(tm0, count, true, 0)` |
| MAC address API | not needed | `#include "esp_mac.h"` required |
