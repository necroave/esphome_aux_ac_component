# Кастомный компонент для ESPHome для управления кондиционером по wifi #
Readme in english [is here](README-EN.md#esphome-aux-air-conditioner-custom-component-aux_ac). 

Управляет кондиционерами на базе AUX по wifi.<br />
По тексту ниже для компонента используется сокращение `aux_ac`.

Обсудить проект можно [в чате Телеграм](https://t.me/aux_ac).<br /> 
Отзывы о багах и ошибках, а так же запросы на дополнительный функционал оставляйте [в соответствующем разделе](https://github.com/GrKoR/esphome_aux_ac_component/issues).
Будет просто отлично, если к своему сообщению вы добавите лог и подробное описание. Для сбора логов я написал [специальный скрипт на Python](https://github.com/GrKoR/ac_python_logger). С его помощью вы сможете сохранить в csv-файл все пакеты, которыми обменивается wifi-модуль и сплит-система. Если такой лог дополнить описанием, в какое время и что именно вы пытались включить, то это сильно ускорит исправление багов.


## ДИСКЛЭЙМЕР (ОТМАЗКИ) ##
1. Все материалы этого проекта (программы, прошивки, схемы, 3D модели и т.п.) предоставляются "КАК ЕСТЬ". Всё, что вы делаете с вашим оборудованием, вы делаете на свой страх и риск. Автор не несет ответственности за результат и ничего не гарантирует. Если вы с абсолютной четкостью не понимаете, что именно вы делаете и для чего, лучше просто купите wifi-модуль у производителя вашего кондиционера. 
2. Я ~~не настоящий сварщик~~ не программер. Поэтому код наверняка не оптимален и плохо оформлен (зато комментариев по коду я разместил от души), местами может быть написан небезопасно. И хоть я и старался протестировать всё, но уверен, что какие-то моменты упустил. Так что отнеситесь к коду с подозрением, ожидайте от него подвоха и если что-то увидели - [пишите в багрепорт](https://github.com/GrKoR/esphome_aux_ac_component/issues).


## Общее описание ##
Этот кастомный компонент для ESPHome позволяет управлять по wifi кондиционером, сделанным на фабриках AUX.<br />
Прошивка тестировалась с ESPHome 1.15.3 и сплит-системой Rovex серии ALS1. Скорее всего многие другие кондиционеры разных брендов, так же произведенные на фабриках AUX, могут управляться `aux_ac` без переделок. Но это не точно :)<br />
По понятным причинам протестирован ограниченный перечень кондиционеров. Полный перечень протестированных кондиционеров приведен в списке ниже.


## Поддерживаемые кондиционеры ## 
### Список совместимых (протестированных) кондиционеров ###
Приведенные ниже в списке кондиционеры были протестированы автором `aux_ac` или пользователями. И у нас все функции работали.<br />
Отсутствие вашего кондиционера в списке не говорит о том, что `aux_ac` с ним не работает. Но и присутствие названия в списке протестированных тоже не даёт никакой гарантии, так как тест проводится такими же пользователями компонента, как и вы.<br />
Проведенное автором или пользователями тестирование может не включать какие-то функции по причине их отсутствия в кондиционере тестировщика. Но как минимум присутствие вашего кондиционера в списке протестированных позволяет говорить, что у кого-то из пользователей компонента своим кондиционером этого бренда управлять получилось. Так что с должной осмотрительностью можно пробовать запускать у себя.

Протестированы:
+ Rovex (models: RS-07ALS1, RS-09ALS1, RS-12ALS1)
+ AUX (models: ASW-H09A4/LK-700R1, ASW-H09B4/LK-700R1, AMWM-xxx multysplit)
+ IGC (models: RAK-07NH multysplit)
+ Centek (models: CT-65Q09)
+ Roda (models: RS-AL09F)

 
### Список потенциально совместимых кондиционеров ###
**НЕ ТЕСТИРОВАЛИСЬ! ИСПОЛЬЗУЙТЕ КОМПОНЕНТ НА СВОЙ СТРАХ И РИСК!**<br />
AUX - это один из нескольких OEM-производителей кондиционеров. AUX производят кондиционеры как под собственным брендом, так и для внешних заказчиков. Поэтому есть шанс, что произведенный на их фабрике кондиционер неизвестного бренда с `aux_ac` так же заработает.

В интернете есть такой перечень производившихся на фабриках AUX брендов:
+ Abion
+ AC ELECTRIC
+ Almacom
+ Ballu 
+ CENTEK
+ Climer
+ DAX
+ Energolux
+ ERISSON
+ Green Energy
+ Hyundai
+ Kentatsu (некоторые серии; Kentatsu KSGMA26HFAN1 протестирован и **точно не поддерживается**)
+ Klimaire
+ KOMANCHI
+ LANZKRAFT
+ LEBERG
+ LGen
+ Monroe
+ Neoclima
+ NEOLINE
+ One Air
+ Pioneer (до 2016 года)
+ Roda
+ Royal Clima
+ SAKATA
+ SATURN
+ Scarlett
+ SmartWay
+ Soling
+ SUBTROPIC
+ Supra
+ Timberk
+ Vertex
+ Zanussi

Если производитель вашего кондиционера есть в списке выше, то стоит изучить вопрос. Возможно, вам тоже подойдет `aux_ac` для управления по wifi.<br />
Если в инструкции пользователя вашего кондиционера что-то написано про возможность управления по wifi (особенно с помощью мобильного приложения ACFreedom), то есть весьма существенные шансы, что `aux_ac` сможет управлять и вашим кондиционером. Но будьте осмотрительны: ваш кондиционер никем не тестировался и важно четко понимать, что вы делаете. Иначе можете поломать кондиционер.<br />
Если вы не уверены в своих силах, лучше дождитесь, пока другие более опытные пользователи протестируют вашу модель кондиционера (правда, это может не случиться никогда). Или приходите с вопросами [в телеграм-чат](https://t.me/aux_ac). Возможно, там вам помогут.

Если вы протестировали ваш кондиционер и он работает, напишите мне, пожалуйста. Я внесу вашу модель в список протестированных. Возможно, это упростит кому-то жизнь =)<br />
Лучший способ сообщить о протестированном кондиционере - написать [в разделе багрепортов и заказа фич](https://github.com/GrKoR/esphome_aux_ac_component/issues). [В телеграм](https://t.me/aux_ac) тоже можно, но есть шанс, что в ворохе сообщений ваше потеряется.

## Как использовать компонент ##
### Железо ###
Я тестировал проект на esp8266 (esp-12e). Минимальная обвязка традиционная и выглядит так:<br /> 
![scheme](https://github.com/GrKoR/esphome_aux_ac_component/blob/master/images/scheme.png?raw=true)

Для прошивки esp8266 в первый раз нужно в дополнение к обвязке, показанной на схеме выше, притянуть к Земле пин IO0 (GPIO0). После этого ESPHome может быть загружена в esp8266 по UART0. Если при этом вы указали OTA в конфигурации ESPHome, то в дальнейшем пин IO0 можно подтянуть к питанию или оставить висеть в воздухе. Он никак не будет влиять на загрузку новых прошивок, потому что все апдейты можно будет делать "по воздуху" (то есть по wifi). Я никуда IO0 не подтягивал и ничего к нему не паял, потому что не вижу смысла это делать ради одного раза. Первую прошивку делал в самодельном переходнике на макетке.

Плата esp-12e перед подключением кондиционера и модуля питания:<br /> 
![esp-12e minimal photo](https://github.com/GrKoR/esphome_aux_ac_component/blob/master/images/esp-12e.jpg?raw=true)

Внутренний блок сплит-системы имеет 5-проводное подключение к модулю wifi. Коннектор [JST SM](https://www.jst-mfg.com/product/pdf/eng/eSM.pdf).
 
Перечень проводников:
1. Желтый: +14В постоянного тока. Осциллограф показал от +13.70В до +14.70В. В сервисном мануале встречалось, что питание возможно до +16В.
2. Черный: земля.
3. Белый: +5В постоянного тока (измерено от +4.43В до +5.63В). Для чего нужна эта линия - не понятно. У меня нет версий. Эксперименты с родным wifi-модулем сплит-системы показали, что эта линия в работе wifi не участвует. Линия идет напрямую на ножку контроллера в сплите через резистор 1 кОм.
4. Синий: TX кондиционера. Высокий уровень +5В.
5. Red: RX кондиционера. Высокий уровень +5В.

Для питания ESP8266 можно использовать любой подходящий DC-DC преобразователь. Я использовал такой:<br /> 
![power module](https://github.com/GrKoR/esphome_aux_ac_component/blob/master/images/DD4012SA.jpg?raw=true).

Подключение:<br />
Черный провод (земля) подключается к земле DC-DC преобразователя и к пину GND модуля ESP8266.<br />
Желтый провод подключается ко входу DC-DC преобразователя (в моём случае контакт Vin).<br />
Синий провод подключается к пину RXD модуля esp-12e.<br />
Красный провод подключается к пину TXD модуля esp-12e.<br />

Вот схема всех соединений:<br />
![connections](https://github.com/GrKoR/esphome_aux_ac_component/blob/master/images/connections.png?raw=true)
 
Вот так это выглядит внутри самодельного корпуса:<br /> 
![module assembled](https://github.com/GrKoR/esphome_aux_ac_component/blob/master/images/assembled.JPG?raw=true)
 
Поскольку у меня не было под рукой коннекторов JST SM, а ехать искать их не хотелось, я сделал свой собственный из стандартных пинов с шагом 2,54 мм и нескольких напечатанных на 3D-принтере деталей:<br /> 
![JST SM connector replica](https://github.com/GrKoR/esphome_aux_ac_component/blob/master/images/connector.JPG?raw=true).
 
Все относящиеся к проекту модели для 3D-принтера также доступны: [STL-файлы коннектора](https://github.com/GrKoR/esphome_aux_ac_component/tree/master/enclosure/JST%20SM%20connector), [модельки частей корпуса](https://github.com/GrKoR/esphome_aux_ac_component/tree/master/enclosure/case). 
 
Конечный результат:<br /> 
![photo 1](https://github.com/GrKoR/esphome_aux_ac_component/blob/master/images/real-1.JPG?raw=true)<br />
![photo 2](https://github.com/GrKoR/esphome_aux_ac_component/blob/master/images/real-2.JPG?raw=true)<br />
![photo 3](https://github.com/GrKoR/esphome_aux_ac_component/blob/master/images/real-3.JPG?raw=true)



### Прошивка: интеграция aux_ac в вашу конфигурацию ESPHome ###
1. Скопируйте файл `aux_ac_custom_component.h` в папку с вашими YAML-файлами ESPHome.
2. В заголовочной части вашего YAML-файла пропишите инструкцию `include`. Например:
```yaml
esphome:
  name: $devicename
  platform: ESP8266
  board: esp12e
  includes:
    - aux_ac_custom_component.h
```
3. Настройте UART для коммуникации с вашим кондиционером:
```yaml
uart:
  id: ac_uart_bus
  tx_pin: GPIO1
  rx_pin: GPIO3
  baud_rate: 4800
  data_bits: 8
  parity: EVEN
  stop_bits: 1
```
4. У ESP8266 два аппаратных UART: UART0 и UART1. Нам подходит только UART0, поскольку только он имеет и TX и RX. Поэтому в секции **uart:** выше мы настроили UART0 для нужд `aux_ac`. Но на том же УАРТе сидит и **logger**. Чтобы не было коллизий, настраиваем логгер на работу с UART1, у которого есть только TX, чего для нужд логгера более чем достаточно:
```yaml
logger:
    level: DEBUG
    # important: for avoiding collisions logger works with UART1 (for esp8266 tx = GPIO2, rx = None)
    hardware_uart: UART1
```
5. Последний шаг - объявление кастомного компонента:
```yaml
climate:
- platform: custom
  lambda: |-
    extern AirCon acAirCon;
    if (!acAirCon.get_initialized()) acAirCon.initAC(id(ac_uart_bus));
    App.register_component(&acAirCon);
    return {&acAirCon};
  climates:
    - name: "My awesome air conditioner"
```

## Простейший пример ##
Исходный код простейшего примера можно найти в файле [aux_ac_simple.yaml](https://github.com/GrKoR/esphome_aux_ac_component/blob/master/examples/simple/aux_ac_simple.yaml).

Все настройки в нем тривиальны и подробно описаны [в официальной документации на ESPHome](https://esphome.io/index.html) и дополнены [в разделе об интеграции компонента](https://github.com/GrKoR/esphome_aux_ac_component#%D0%BF%D1%80%D0%BE%D1%88%D0%B8%D0%B2%D0%BA%D0%B0-%D0%B8%D0%BD%D1%82%D0%B5%D0%B3%D1%80%D0%B0%D1%86%D0%B8%D1%8F-aux_ac-%D0%B2-%D0%B2%D0%B0%D1%88%D1%83-%D0%BA%D0%BE%D0%BD%D1%84%D0%B8%D0%B3%D1%83%D1%80%D0%B0%D1%86%D0%B8%D1%8E-esphome) в ваш девайс.<br />
Просто скопируйте yaml-файл примера и `aux_ac_custom_component.h` в локальную папку у себя на компьютере, пропишите настройки вашей сети WiFi и откомпилируйте YAML с использованием ESPHome. 


## Продвинутый пример ##
Все исходники продвинутого примера лежат [в соответствующей папке](https://github.com/GrKoR/esphome_aux_ac_component/tree/master/examples/advanced).

В это примере мы конфигурируем два относительно одинаковых кондиционера на работу с `aux_ac`.<br />
Вводные: представим, что у нас есть два кондея, расположенных в кухне и в гостиной. Эти кондиционеры могут и не быть одного бренда. Главное, чтобы они были совместимы с `aux_ac`.<br />  
Поскольку мы ленивы, мы пропишем все общие настройки обоих кондиционеров в общем конфигурационном файле `ac_common.yaml`.<br />
А все параметры, специфичные для каждого конкретного устройства, вынесем в отдельные файлы. Это файлы `ac_kitchen.yaml` и `ac_livingroom.yaml`. В них мы установим значения для подстановок `devicename` и `upper_devicename`, чтобы у устройств в сети были корректные имена самого компонента и его сенсоров. И здесь же мы указываем уникальные для каждого устройства IP-адреса, спрятанные в `secrets.yaml`.<br />
Кстати да! **Не забудьте** присвоить корректные значения `wifi_ip_kitchen`, `wifi_ota_ip_kitchen`, `wifi_ip_livingroom` и `wifi_ota_ip_livingroom` в файле `secrets.yaml` наряду с остальной "секретной" информацией (например пароли, токены и т.п.). Файл `secrets.yaml` по понятным причинам на гитхаб не выложен.

Если попытаться компилировать файл `ac_common.yaml`, то ESPHome выдаст ошибку. Для корректной прошивки необходимо компилировать `ac_kitchen.yaml` или `ac_livingroom.yaml`.
 
## Дополнительная функциональность ##
Компонент `aux_ac` предоставляет три дополнительных сенсора: два значения температуры и один номер версии прошивки.

### Комнатная температура ###
Этот сенсор отдает значения комнатной температуры воздуха с внутреннего блока кондиционера. Если значение этого датчика вам нужно, пропишите подобную конфигурацию сенсора в вашем YAML-файле:
```yaml
sensor:
  - platform: custom
    lambda: |-
      extern AirCon acAirCon;
      if (!acAirCon.get_initialized()) acAirCon.initAC(id(ac_uart_bus));
      App.register_component(&acAirCon);
      return {acAirCon.sensor_ambient_temperature};
    sensors:
    - name: AC ambient temperature
      unit_of_measurement: "°C"
      accuracy_decimals: 1
```
 
### Уличная температура ###
К сожалению, пока этот сенсор показывает погоду на Марсе =) Значение, обрабатываемое `aux_ac` для нужд этого сенсора точно как-то связано с уличной температурой, но полностью расшифровка значения не известна. Есть предположение, что это температура испарителя во внешнем блоке, потому что при переключении кондиционера с обогрева на охлаждение или обратно эта температура стремительно меняется. А при выключенном кондиционере в течение суток меняется похожим на уличную температуру образом. Однако всё это при теплой погоде на улице. При отрицательной температуре показывает одно и то же значение. По крайней мере при температурах в диапазоне -25..-19 градусов Цельсия.<br />
В общем, для расшифровки надо собрать больше статистики и коллективно подумать в чатике.

Если несмотря на сказанное вам нужно это значение в ESPHome, пропишите следующий сенсор в конфигурации:
```yaml
sensor:
  - platform: custom
    lambda: |-
      extern AirCon acAirCon;
      if (!acAirCon.get_initialized()) acAirCon.initAC(id(ac_uart_bus));
      App.register_component(&acAirCon);
      return {acAirCon.sensor_outdoor_temperature};
    sensors:
    - name: AC outdoor temperature
      unit_of_measurement: "°C"
      accuracy_decimals: 1
```
 
### Обе температуры одновременно ###
Возможно прописать конфигурацию обоих сенсоров в одном определении:
```yaml
sensor:
  - platform: custom
    lambda: |-
      extern AirCon acAirCon;
      if (!acAirCon.get_initialized()) acAirCon.initAC(id(ac_uart_bus));
      App.register_component(&acAirCon);
      return {acAirCon.sensor_outdoor_temperature, acAirCon.sensor_ambient_temperature};
    sensors:
    - name: AC outdoor temperature
      unit_of_measurement: "°C"
      accuracy_decimals: 1
    - name: AC ambient temperature
      unit_of_measurement: "°C"
      accuracy_decimals: 1
```
 
### Версия прошивки ###
Компонент `aux_ac` предоставляет информацию о своей версии в виде текстового сенсора. Соответствующая конфигурация показана ниже:
```yaml
text_sensor:
- platform: custom
  lambda: |-
    auto aircon_firmware_version = new AirConFirmwareVersion();
    App.register_component(aircon_firmware_version);
    return {aircon_firmware_version};
  text_sensors:
    name: AC firmware version
    icon: "mdi:chip"
```
