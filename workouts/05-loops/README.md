# Running commands

Теперь разберём как можно выполнять задачи в цикле.
За основу возьмём плейбук с прошлой главы.

```yaml
- hosts: all

  tasks:
    - name: Install nginx and update apt cache
      apt:
        name: nginx
        update_cache: yes
        state: latest

    - name: Sync site content
      synchronize:
        src: site/
        dest: /var/www/html
        delete: yes
        recursive: yes

    - name: Enable nginx service
      service:
        name: nginx
        state: started
        enabled: yes
```

Сделаем отдельную страничку для каждого изображения. На странице выведем всю доступную нам информацию о нём.

Сначала создадим шаблон странички для вывода информации про изображение

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>{{ filename }}</title>
</head>
<body>
<pre>
{{item | to_nice_json}}
</pre>
<p>
    <img src="{{ item.path | replace('/var/www/html', '') }}" alt="CodeX Logo">
</p>
</body>
</html>
```

Теперь с помощью ключевого слова loop проитерируемся по всем файлам и создадим для них html страницы.

```yaml
    - name: Render page for each image
      template:
        src: site/image-page.html
        dest: "/var/www/html/{{ filename }}.html"
      vars:
        filename: "{{ (item.path | basename).split('.') | first }}"
      loop: "{{ images.files }}"
```
