## Heroku DJANGO deployment

- install heroku cli
```bash
curl https://cli-assets.heroku.com/install-ubuntu.sh | sh
```

- install python module gunicorn
```bash
pipenv install gunicorn
```

- create a file called Procfile (process file) - heroku looks to start app
- inside the Procfile write the following
```yaml
web: gunicron appname.wsgi
```
- with this we are telling heroku
    - we need a web process (web) - a process used for web app
    - to start that process we need gunicorn (gunicorn is webserver)
    - appname.wsgi is the name of the module that gunicorn will use to start our application (app/wsgi.py)

### Preapre static file for deployment

- bring those static files to current folder to deploying it to heroku
- open settings.py in main app and add a variable STATIC_ROOT
- create folder static in the project dir  (do not hard the static path) - use os.path
```python
STATIC_ROOT = os.path.join(BASE_DIR,'static')
```
- run the below command to get all static files in the project
```bash
python manage.py collectstatic
```
- to serve static files to heroku will need a python module called whitenoise (need onyl for heroku deployment)
- install whitenoise and register it's middleware on settings.py
### EG
```python
MIDDLEWARE = [
    'django.middleware.security.SecurityMiddleware',
    'whitenoise.middleware.WhiteNoiseMiddleware'
]
```

## Deploying APP

- Behind Heroku Deploy
    - create a local repo for the application
    - setup a repo in heroku as mirror for the local
    - heroku has process which watches local repo which will fetch the changes to heroku

- Deployment
    - login to heroku cli using and create an app
    ```bash
    heroku login
    heroku create
    ```
    - the heroku will create an app with random name (unique) we can have a custom name (should be unique)
    ```bash
    heroku create customapp
    ```
    - a default page will displayed before deployment
    - Behind Deploy
        - first heroku create's app and it's address 
        - second it creates a git repository at it's address
        - third it will tell the local repo about remote heroku repo (the connection)
    - now deploy the app
    ```bash
    git init 
    git add .
    git commit -m "Initial Commit"
    git push heroku master
    ```
    - while pushing heroku will install python and it's modules on heroku side
    - now tell heroku to allocate one webserver to this app
    ```bash
    heroku ps:scale web=1 # allocating one webserver to this app
    ```
    - in future if we have more website we can allocate more by increasing the number of web
    - to open the site 
    ```bash
    heroku open
    ```

- Fixing Host
    - when first opening we will get DisallowedHost at / warining it can be fixed
    - add the heroku address to the list ALLOWED_HOSTS in settings.py
    ```python
    ALLOWED_HOSTS = [
    'shrouded-bayou-21968.herokuapp.com'
    ]
    ```
- Making changes
    - after making changes add and commit on you local repository and deploy via
    ```bash
    git push heroku master
    ```


