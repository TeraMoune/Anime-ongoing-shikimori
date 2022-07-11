# Anime-ongoing-shikimori
Календарь выхода Anime (API Shikimori)

Плагин добавит раздел на сайте календаря выхода аниме произведений в точности как у [Shikimori](https://shikimori.one/api/doc), API которого и предоставляет информацию.

<p align="center">
<img src="https://user-images.githubusercontent.com/44625352/178162946-5e767cf3-313e-4ec1-bcb6-617ffd3aa7f6.jpg" alt="Anime-ongoing-shikimori">
</p>

## Установка
1. Скачать архив репозитория и распаковать
2. Установить `anime-ongoing.xml` в систему
3. Подключить или перенести стили из `ongoing.css`
4. Скачать [magnific-popup](https://dimsemenov.com/plugins/magnific-popup/) и так же установить на сайт

## Раздел на сайте
После установки модуля раздел будет доступен по адресу `/?do=ongoing`, либо можете добавить ЧПУ правило.

В файле `.htaccess` Добавить ниже строки `RewriteEngine On`
```
RewriteRule ^ongoing(/?)$ index.php?do=ongoing [L]
```

## Изменение языка
В плагине в редактировании файла `engine/modules/ongoing.php` находим `data-name="{$animes['anime']['name']}"` и заменяем '**name**' на '**russian**'.

## Касательно прочей разметки раздела
В плагине есть функция `shiki_cals()` в ней в переменных `$buffer` прописаны HTML объекты. Вся разметка описывается в них.

> У блоков есть наличие рандомных `class` имен цветов, для окрашивания к соответствующие цвета добавьте правила в CSS с соответствующими именами.
> Представлено 11 названий.
```php
	$arr_color = array(
		'red',
		'pink',
		'blue',
		'green',
		'orange',
 		'brown',
		'powderblue',
		'skyblue',
		'purple',
		'magenta',
		'brown'
	);
  ```

## JavaScript функция отображения окна с поиском похожих новостей на сайте.
Вызов модального окна реализован на примере плагина [magnific-popup](https://dimsemenov.com/plugins/magnific-popup/)
```js
function ongoing_find(obj) {
    
    var name = $(obj).data('name');
    
    $.post(dle_root + "engine/ajax/controller.php?mod=find_relates", {title: name, mode: 1, accuracy_find: 1, user_hash: dle_login_hash}, function(data) {
        
        $.magnificPopup.open({
            items: {
                src: '<div style="width:700px;background:#FFF;position:relative;margin:0 auto;padding:25px 10px 10px;" class="clrfix">'+data+'</div>'
            },
            type: 'inline',
            mainClass: 'mfp-fade',
            removalDelay: 0,
            overflowY: 'hide',
            closeOnBgClick: true,
            callbacks: {
                open: function() {},
                afterClose: function() {},         
                beforeClose: function() {}
            }        
        });
            
    });

    return false;

};
```

## Об изменении домена API
Адрес **API** сервиса **shikimori** может меняться. Если плагин перестаёт получать данные то стоит проверить верно ли указан адрес.

В плагине стоит найти следующую строчку в которой указывается адрес.
```php
file_get_contents('https://shikimori.one/api/calendar');
```
