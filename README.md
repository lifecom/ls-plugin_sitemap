[![Build Status](https://secure.travis-ci.org/stfalcon-studio/ls-plugin_sitemap.png?branch=master)](https://travis-ci.org/stfalcon-studio/ls-plugin_sitemap)

ОПИСАНИЕ
--------

Плагин предназначен для генерации Sitemaps в LiveStreet CMS.

С помощью файла Sitemap веб-мастеры могут сообщать поисковым системам о
веб-страницах, которые доступны для сканирования. Файл Sitemap представляет
собой XML-файл, в котором перечислены URL-адреса веб-сайта в сочетании с
метаданными, связанными с каждым URL-адресом (дата его последнего изменения;
частота изменений; его приоритетность на уровне сайта), чтобы поисковые системы
могли более грамотно сканировать этот сайт.

Более подробную информацию вы можете найти на странице http://sitemaps.org/ru/.

ЛИЦЕНЗИИ
-------

Файлы в этом архиве распостраняются по лицензии GNU GPL. Вы можете найти копию
этой лицензии в файле LICENSE.txt.


ИСТОРИЯ ВЕРСИЙ
--------------

v0.4.0
- Плагин совместим с LiveStreet v1.0
- Добавлены BDD тесты и конфиг для Travis CI

v0.3.0
- Добавлен вывод персональных блогов
- Улучшена производительность при генерации сайтмапа пользователей

v0.2.1
- Для совместимости с плагином L10n добавлена возможность переопределять метод
  генерирующий префикс для кеша наборов.
- Реализована возможность добавлять наборы ссылок в sitemap index.

v0.2.0 (анонс http://livestreet.ru/blog/addons/5591.html)
- Основательный рефакторинг кода плагина. Теперь все действия которые производят
  с наборами сущностей или свойствами сущностей другие плагины отображаются в
  генерируемых sitemap'ах.
  Для примера плагин NiceUrl изменяет url записей и в sitemap топиков 
  выводятся url измененные плагином NiceUrl.
- Изменены ссылки в sitemap.xml в соответсвии с рекомандациями опубликованными
  на странице http://sitemaps.org/ru/protocol.php#location. Теперь они
  выглядят так как будто файлы sitemap'ов расположены в корне сайта.
- Добавлены XSLT шаблоны для удобного просмотра sitemap в окне браузера.
- Все основные настройки вынесены в конфиг плагина. Это время жизни кеша для 
  наборов записей, приоритеты страниц, вероятная частота изменений страниц.
- Добавлена возможность интеграции для сторонних плагинов.

v0.1.1
- Плагин переработан в соответсвии с новыми стандартами наименований в LS 0.4.1.

v0.1.0 (анонс http://livestreet.ru/blog/addons/4262.html)
- Модуль Sitemap-generator Дмитрия Гадеева был оформлен в виде plugin'а Sitemap
  для LS 4.0

Пример конфигурации server для корректной работы с nginx
-------

    server {
        # ... другие настройки
    
        location / {
            index       index.html index.htm index.php;
            try_files   $uri $uri/ @ls; # для несуществующих файлов
        }
    
        location ~ \.php$ {
            fastcgi_pass        unix:/var/run/php5-fpm.sock;
            fastcgi_index       index.php;
            fastcgi_param       SCRIPT_FILENAME /var/www/example.com/www/$fastcgi_script_name;
            include             fastcgi_params;
        }
    
        location @ls {
            fastcgi_pass        unix:/var/run/php5-fpm.sock;
            fastcgi_param       SCRIPT_FILENAME  /var/www/example.com/www/index.php;
            fastcgi_param       QUERY_STRING     $uri;
            include             fastcgi_params;
        }
    
    }
