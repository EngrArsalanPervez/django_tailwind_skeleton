# Django Tailwind Integration 2023

### Create a new project

```bash
mkdir testsite
cd testsite
mkdir django_src
mkdir venv

cd venv
python3 -m venv .
source bin/activate

pip install django
pip freeze

cd ../django_src
django-admin startproject config .
ls

python3 manage.py makemigrations
python3 manage.py migrate
python3 manage.py runserver
```

### open testsite folder in pycharm

```bash
1. Configure python interpreter
	
	add a EXISTING interpreter 
	
	testsite/venv/bin/python

2. Add django launch server
		
	Edit Configurations
	
	Add new django server
	
	rename it to testsite
	
	set python interpreter
	
	set working directory: ./testsite/django_src/
	
	if error:
	
	Add django support
	
	Enable django support 
	
	Project root: ./testsite/django_src/
	
	Settings: ./testsite/django_src/config/settings.py
	
	Manage Script: ./testsite/django_src/manage.py
	
	Apply
	
	Ok

	run server
```

### Install package

``` python
python -m pip install django-compressor
```
	

### Create static folders	

```bash
cd ./testsite/django_src/
mkdir templates
mkdir static
mkdir media
```

### Update settings.py

```python
# Update Templates Dir
TEMPLATES = [ 
	{
		'DIRS': [BASE_DIR / 'templates'],
	}
]
	
# Register the compressor app
INSTALLED_APPS = [
	{
		'compressor',
	}
]

# Update Static urls
STATIC_URL = 'static/'

STATICFILES_DIRS = [BASE_DIR/'static']

# Add at the bottom of file
COMPRESS_ROOT = BASE_DIR / 'static'

COMPRESS_ENABLED = True

STATICFILES_FINDERS = ('compressor.finders.CompressorFinder',)

```

### Start a new app

```python
python3 manage.py startapp core
```

### Update core/views.py

```python
from django.shortcuts import render 

def index(request): 
	return render(request, 'core/index.html')
```

### Create index.html in templates/core folder

```python
{% extends "_base.html" %} 

{% block content %} 
	<h1 class="text-3xl text-green-800"> 
		GeeksforGeeks 
	</h1> 
	<h3 class="text-3x1 text-green-800"> 
		GeeksforGeeks 
	</h3> 
{% endblock content %}
```

### Create _base.html in templates folder

```python
{% load compress %} 
{% load static %} 

<!DOCTYPE html> 
<html lang="en"> 

<head> 
	<meta charset="UTF-8"> 
	<meta http-equiv="X-UA-Compatible" content="IE=edge"> 
	<meta name="viewport" content= 
		"width=device-width, initial-scale=1.0"> 
	<title>Django + Tailwind CSS</title> 

	{% compress css %} 
	<link rel="stylesheet"
		href="{% static 'output.css' %}"> 
	{% endcompress %} 

</head> 

<body class="bg-green-50"> 
	<div class="container mx-auto mt-4"> 
		{% block content %} 
		{% endblock content %} 
	</div> 
</body> 

</html>
```


### Update root urls.py

```python
from django_src.core.views import index 

urlpatterns = [ 
	path('admin/', admin.site.urls), 
	path('', index, name='index') 
]
```

### Install Tailwind using node

```bash
cd testsite/
sudo apt install npm
npm install -D tailwindcss
npx tailwindcss init
```

### Update tailwind.config.js

enable code assistance for Node.js in Pycharm

```javascript
module.exports = { 
	content: [ 
		'./django_src/templates/**/*.html'
	], 
	theme: { 
		extend: {}, 
	}, 
	plugins: [], 
}
```


### Create input.css in static folder

```css
@tailwind base; 
@tailwind components; 
@tailwind utilities;
```

### Update package.json

```json
{
 "scripts": {
  "dev": "npx tailwindcss -i ./django_src/static/input.css -o ./django_src/static/output.css --watch"
 },
 "devDependencies": {
  "tailwindcss": "^3.3.2"
 }
}
```

### Run Tailwind

```bash
npm run dev
```

### Happy development :-)
