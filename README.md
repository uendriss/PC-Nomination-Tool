# PC-Nomination-Tool

This repository hosts a [Django](https://www.djangoproject.com/) project that implements a
program committee (PC) nomination tool. This website allows users to nominate PC members, checking in
the background that the nominated persons have not already been nominated to avoid the usual 
overhead of finding duplicates in long lists of nominations.

## Presentation of the Tool

### Nominating Potential PC Members

This tool is accessible at the address `BASE_URL/nomination` where `BASE_URL` is the url of your
server. It is used to nominate PC members. After entering their name, users arrive to this page:

<img src="https://github.com/uendriss/PC-Nomination-Tool/tree/main/readme_imgs/nom_nomination.png" alt="Nomination page" height="50vh"/>

Using this form, users can enter the details of a person. The details they are entering are matched
against the database in the background, warning the user if they are potentially entering the details
of someone who has already been nominated.

<img src="https://github.com/uendriss/PC-Nomination-Tool/tree/main/readme_imgs/nom_duplicates.png" alt="Nomination page" height="50vh"/>

The identity of a person is linked to their DBLP page URL (as the other details needs not be unique).
When entering the name of someone, the DBLP API is queried to suggest pre-filled information:

<img src="https://github.com/uendriss/PC-Nomination-Tool/tree/main/readme_imgs/nom_DBLP.png" alt="Nomination page" height="50vh"/>

### Administrating the Nominations

On the page `BASE_URL/nomination/manage`, several admin tools are proposed.

- You can import nominations as a CSV file.
- You can export the nomination database as a CSV file.
- You can check if there are potential duplicates: database entries representing the same person
but with different email addresses.
- You can explore in a big table all the nominated persons.

## Development/Deployment

This project is ready to be deployed on a server by anyone who has basic knowledge of how to deploy
Django applications. In the following we present few steps to get started.

### Local Settings

A local setting file `confutils\local_settings.py` is required for the website to work. It should
look like this:

```
import os

# Build paths inside the project like this: os.path.join(BASE_DIR, ...)
BASE_DIR = os.path.dirname(os.path.dirname(os.path.abspath(__file__)))

# SECURITY WARNING: keep the secret key used in production secret! And update this fake one!
SECRET_KEY = 'secret'

# SECURITY WARNING: don't run with debug turned on in production!
DEBUG = True

# For production, update with the actual URL host
# ALLOWED_HOSTS = [""]

DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.sqlite3',
        'NAME': os.path.join(BASE_DIR, 'confutils.db'),
    },
}

STATIC_ROOT = "static/"

FILE_UPLOAD_MAX_MEMORY_SIZE = 40 * 1024 * 1024
````

### Python Setup

Next, install the required python libraries. Additional libraries may be needed depending on your
database engine.

```
pip install -r requirements.txt
```

### Database Setup

To create the databases, run the following:

```
python manage.py makemigrations
python manage.py migrate
python manage.py initialise_db
```

### Super Admin

Create a super admin to have access to the admin pages:

```
python manage.py createsuperuser
```

### Serve the Website Locally

To serve the website on your local machine run: 

```
python manage.py runserver
```

### Server Deployment

Follow any good Django deployment tutorial to deploy this project on a server.
