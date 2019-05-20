Настройка cron для импорта данных из CSV
========================

## 1. Создание и настройка профиля импорта данных

* По пути `Рабочий стол -> Магазин -> Торговый каталог -> Импорт данных` добавить профиль `import CSV (new)` 
	* Но сперва определим доступные поля для импорта.
	* перейти `Рабочий стол->Настройки->Настройки продукта->Настройки модулей->Торговый каталог` далее вкладка `Экспорт / Импорт` далее блок `Экспорт / импорт из CSV` настроить как на картинке: 

	![joxi_screenshot_1558294369609](/media/29342.png)

* Вкладка `Файл данных` 

![joxi_screenshot_1558295545379]/media/2917.png)

* Далее 	`Формат` 

![joxi_screenshot_1558295657243](/media/2407.png)

* Далее `Поля`

![joxi_screenshot_1558295721382](/media/7646.png)

![joxi_screenshot_1558295767266](/media/28306.png)

* **Сохранить .**

## 2. Тест импорта данных 

![2019-05-19_23-59-44](/media/22530.png)

## 3. Привязать профиль импорта к cron 

![2019-05-20_00-03-31](/media/101.png)

![20.05.2019 09-40-57-193](/media/0001.png)

Проверить путь до php можно введя на сервере команду ` whereis php`

![20.05.2019 09-40-57-193](/media/0002.png)

## 4. Подключаемся к серверу Битрикс под root пользователем

В папке сайта переходим по пути `/bitrix/php_interface/include/catalog_import` 

## 5. Редактируем `cron_frame.php`

![20.05.2019 09-40-57-193](/media/0003.png)

1. проверть путь до пхп
2. полный путь до папки сайта на сервере
3. ИД сайта в битриксе

## 6. Проверяем и настраеваем сам крон

1. На всякий случай поправим время запуска в /home/bitrix/ext_www/**niime.bitdev.ru/bitrix/crontab/crontab.cfg**
   
   `0 1 * * * /usr/bin/php -f /home/bitrix/ext_www/niime.bitdev.ru/bitrix/php_interface/include/catalog_import/cron_frame.php 1 >/home/bitrix/ext_www/niime.bitdev.ru/bitrix/php_interface/include/catalog_import/logs/1.txt`

   каждый день в час ночи

2. Настройка крона

	в папке `/var/spool/cron`  на сервере найти файл с именем пользователя `bitrix`

	прописать 
	```
	0 1 * * * /usr/bin/php -f /home/bitrix/ext_www/niime.bitdev.ru/bitrix/php_interface/include/catalog_import/cron_frame.php 1 >/home/bitrix/ext_www/niime.bitdev.ru/bitrix/php_interface/include/catalog_import/logs/1.txt

	5 0 * * * /usr/bin/php /home/bitrix/ext_www/niime.bitdev.ru/local/forImportCSV/convert_In_XML_to_CSV.php >/home/bitrix/ext_www/niime.bitdev.ru/upload/import-1c/log.txt
	```
	в строке `/usr/bin/php -f /home/bitrix/ext_www/niime.bitdev.ru/bitrix/php_interface/include/catalog_import/cron_frame.php 1`  
	"1" - это ИД профиля импорта (эта строка должна уже быть в файле)
	
	Вторая строка это запуск php скрипта на конвертацию данных в csv файл
