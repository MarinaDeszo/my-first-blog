## **Instalação do Django**

###  Ambiente virtual

1. Criando o diretório

```bash
$ pwd
/mnt/c/Users/marin/BioEnv
```



2. Criar o 'virtual enviroment': myvenv

```bash
$ cd /mnt/c/Users/marin/BioEnv
$ python3 -m venv myvenv
```



3. Trabalhando com o virtualenv

```bash
$ source myvenv/bin/activate
```



4. Instalando o Django

```bash
$ source myvenv/bin/activate

$ python -m pip install --upgrade pip # Atualizando o 'pip'

## Criando 'requirements.txt'
##  Abrir editor 'Atom'
##  Digite "Django~=2.2.4"
##  Salve como 'requirements.txt' em '/mnt/c/Users/marin/BioEnv'

$  pip install -r requirements.txt
Collecting Django~=2.2.4
  Using cached Django-2.2.17-py3-none-any.whl (7.5 MB)
Collecting pytz
  Using cached pytz-2020.4-py2.py3-none-any.whl (509 kB)
Collecting sqlparse>=0.2.2
  Using cached sqlparse-0.4.1-py3-none-any.whl (42 kB)
Installing collected packages: sqlparse, pytz, Django
Successfully installed Django-2.2.17 pytz-2020.4 sqlparse-0.4.1

```

------



## Criando o projeto

```bash
## Ativar myenv
$ source myvenv/bin/activate

## Criar projeto
$ django-admin startproject mysite .
```

1. Mudando as configurações

   mysite/settings.py

   ```python
   TIME_ZONE = 'America/Sao_Paulo'
   LANGUAGE_CODE = 'pt-BR'
   STATIC_URL = '/static/'
   STATIC_ROOT = os.path.join(BASE_DIR, 'static') ## adiconar esta linha
   ALLOWED_HOSTS = ['127.0.0.1', '.pythonanywhere.com']
   DATABASES = {
       'default': {
           'ENGINE': 'django.db.backends.sqlite3',
           'NAME': os.path.join(BASE_DIR, 'db.sqlite3'),
       }
   }
   ```

   

2. Criando o banco de dados

   ```bash
   $ source myvenv/bin/activate
   $ python manage.py migrate
   ```

   

3. Iniciando o servidor web

   ```bash
   $ cd /mnt/c/Users/marin/BioEnv
   $ source myvenv/bin/activate
   $ python manage.py runserver
   
   ## Abrir no navegador
   http://127.0.0.1:8000/
   ```

   ![image-20201204160338468](C:\Users\marin\AppData\Roaming\Typora\typora-user-images\image-20201204160338468.png)

------



## Criando uma aplicação

```bash
$ cd /mnt/c/Users/marin/BioEnv
$ source myvenv/bin/activate

$ python manage.py startapp blog
```

Antes

![image-20201204161819227](C:\Users\marin\AppData\Roaming\Typora\typora-user-images\image-20201204161819227.png)

Depois 

![image-20201204162953739](C:\Users\marin\AppData\Roaming\Typora\typora-user-images\image-20201204162953739.png)



Indicando ao Django para usar nosso app

```python
## mysite/settings.py
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'blog', # Incluir a linha 'blog'
]
```



Criando um modelo de postagem

```python
## Sobrescrever blog/models.py

from django.conf import settings
from django.db import models
from django.utils import timezone


class Post(models.Model):
    author = models.ForeignKey(settings.AUTH_USER_MODEL, on_delete=models.CASCADE)
    title = models.CharField(max_length=200)
    text = models.TextField()
    created_date = models.DateTimeField(default=timezone.now)
    published_date = models.DateTimeField(blank=True, null=True)

    def publish(self):
        self.published_date = timezone.now()
        self.save()

    def __str__(self):
        return self.title

```



- [x] ### Criando tabelas para nossos modelos no banco de dados

O último passo é adicionar nosso novo modelo ao banco de dados. Primeiramente, precisamos fazer com que o Django entenda que fizemos algumas alterações nos nossos modelos. (Acabamos de criar um modelo de Post!)

- [x] ```bash
  (myenv)~$ python manage.py makemigrations blog
  
  Migrations for 'blog':
    blog/migrations/0001_initial.py
      - Create model Post
  ```



O Django preparou um arquivo de migração que precisamos aplicar ao nosso banco de dados. Digite 

- [x] ```bash
  (myenv)~$ python manage.py migrate blog
  
  Operations to perform:
    Apply all migrations: blog
  Running migrations:
    Applying blog.0001_initial... OK
  ```

------

## Django Admin

```python
## Substituir blog/admin.py

from django.contrib import admin
from .models import Post

admin.site.register(Post) # tornar nosso modelo (Post) visível na página de administração
```



Ver o nosso modelo de post

```bash
## Criar supeuser
$ cd /mnt/c/Users/marin/BioEnv
$ source myvenv/bin/activate
(myenv) ~$ python manage.py createsuperuser
	Superuser created successfully.

## Rodar servidor
$ python manage.py runserver

## Abrir no navegador
http://127.0.0.1:8000/admin/
```

