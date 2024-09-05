# Integración de React en Django

### Autor

- Ariel Duarte

1. _Creamos el entorno (envs) Django, para eso vamos a crear una carpeta donde tengamos todo el entorno junto con el proyecto:_

```bash
 mkdir django-react
```

2. _Crear el entorno virtual_

```bash
cd django-react
```

```bash
python \-m venv envs/djangoreact
```

> Nota: Si clonamos el proyecto por primera vez tenemos que instalar las librerias que se usan en el proyecto

```bash
pip freeze > requirements.txt
```

```bash
pip install -r requirements.txt
```

3. _Activamos el entorno virtual_

- Para Mac:

```bash
source ./envs/djangoreact/bin/activate
```

- Para Windows:

```bash
cd /envs/djangoreact/
```

```bash
Scripts\activate
```

4. _Ahora con el entorno virtual activado asi (djangoreact), creamos el proyecto django_

```bash
django-admin startproject djangoproj
```

5. _Ya tenemos el proyecto creado y entramos en esa carpeta y corremos el runserver para probar que todo funcione normalmente:_

```bash
cd djangoproj
```

```bash
python manage.py runserver
```

> Nota La url local es http://127.0.0.1:8000/

6. _Creamos el proyecto de react_

Vamos crear el proyecto de React de la forma clasica con el npx create-react-app donde noticiasweb-app es el nombre que le ponemos al proyecto

```bash
npx create-react-app noticiasweb-app
```

Si está desactualizado alguna librería va a pedir ? Need to install the following packages: (y): y

Todo listo nos sale unos mensajes con comandos, el más importante acá es npm run build, porque con ese vamos a ir actualizando nuestros archivos estáticos cada vez que se desarrolla una nueva feature en en la webapp.

El otro comando para tener un entorno de desarrollo de react con el hot reloading activado, es npm start, el cual levanta la aplicación en el puerto [https//localhost:3000](https://3000) para un experiencia de desarrollo mas rapida y al finalizar corremos el npm run build para que impacte los cambios en la url de Django.

> Si clonamos el proyecto no necesitamos hacer el create react app lo que vamos hacer para levatar de forma local nuestro entorno de react es:

```bash
cd noticiasweb-app
```

```bash
npm install
```

```bash
npm start
```

7. _Ahora vamos a ver la configuración para integrar los dos proyectos;_
   1. Vamos a tomar la carpeta de noticiasweb-app y la metemos dentro de la carpeta de djangoproj
   2. Ahora abrimos con VS Code la carpeta desde djangoproj y que ver los dos proyectos con este formato en el directorio de carpetas:

![][image1]

3. Ahora vamos a ir a la carpeta de noticiasweb-app y hacer un build

```bash
cd noticiasweb-app
```

```bash
npm run build
```

Si vemos dentro de la carpeta de noticiasweb-app hay una nueva carpeta llamada build y dentro de build hay una carpeta static

Ese directorio build/static es lo que necesitamos para conectar con nuestro proyecto de Django.

8. _Para esto vamos a ir a la carpeta de djangoproj y vamos abrir el archivo settings.py y importamos la biblioteca de OS_

from pathlib import Path  
import os

9. _También vamos a la sección de TEMPLATES y en DIRS metemos esos directorios así;_

```python
TEMPLATES \= \[
 {
 'BACKEND': 'django.template.backends.django.DjangoTemplates',
 'DIRS': \[os.path.join(BASE_DIR, 'noticiasweb-app/build')\],
 'APP_DIRS': True,
 'OPTIONS': {
 'context_processors': \[
 'django.template.context_processors.debug',
 'django.template.context_processors.request',
 'django.contrib.auth.context_processors.auth',
 'django.contrib.messages.context_processors.messages',
 \],
 },
 },
\]
```

10. _Este mismo directorio noticiasweb-app/build tiene una carpeta static, a esa carpeta la tenemos que definir como un STATICFILES al final del archivo así;_

```python
\# Static files (CSS, JavaScript, Images)
\# https://docs.djangoproject.com/en/5.1/howto/static-files/

STATIC_URL \= 'static/'
STATICFILES_DIRS \= \[
 os.path.join(BASE_DIR, 'noticiasweb-app/build/static')
\]

```

11. _Hasta aca cerramos el archivo settings.py y nos vamos a crear un archivo views.py en directorio de djangoproj y creamos un método render para apuntar al index.html_

```python
from django.shortcuts import render

def index*(request)*:
 return render(request, 'index.html')

12. _Ahora vamos al archivo urls.py y importamos la views_

from django.contrib import admin
from django.urls import path
from . import views

urlpatterns \= \[
 path('admin/', admin.site.urls),
 path('', views.index, name\='index'),
\]

```

13. _Con esto ya tenemos todo integrado el frontend de react en el proyecto de django, solo queda el testearlo, entonces ejecutamos el comando de runserver_

```bash
python manage.py runserver
```

> entonces vamos a la URL: [http://127.0.0.1:8000/](http://127.0.0.1:8000/) ya debemos ver la página de presentación de react:
