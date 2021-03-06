Heroku
======

- Sign up and log in in heroku.com and create a new app, pick a name (eg. my-fuelwatch-app)and any country, and no pipeline.
- Install the CLI tool as specified under the 'Deploy' tab.
- Once you have the heroku installed, make sure you launch a new `cmd` terminal again, so that the `heroku` command is recognised.
- `cd` into your local django project folder and run `heroku login` to login.
- Follow the rest of the instructions under "Deploy using Heroku Git", you can `git init` but do NOT just do `git add. ` because that will add the .pyc and .sqlite files which is not what you want in the Git repository. So `git add` the .py and .html files one by one and commit.
- then do `heroku git:remote -a my-fuelwatch-app` in your Django project folder.
- and finally `git push heroku master`.

ERROR!
------

Your push failed! Saying something about:
    
    Enumerating objects: 50, done.
    Counting objects: 100% (50/50), done.
    Delta compression using up to 8 threads
    Compressing objects: 100% (36/36), done.
    Writing objects: 100% (50/50), 7.40 KiB | 841.00 KiB/s, done.
    Total 50 (delta 13), reused 0 (delta 0)
    remote: Compressing source files... done.
    remote: Building source:
    remote:
    remote:  !     No default language could be detected for this app.
    remote:                         HINT: This occurs when Heroku cannot detect the buildpack to use for this application automatically.
    remote:                         See https://devcenter.heroku.com/articles/buildpacks
    remote:
    remote:  !     Push failed
    remote: Verifying deploy...
    remote:
    remote: !       Push rejected to my-fuelwatch-app.
    remote:
    To https://git.heroku.com/my-fuelwatch-app.git
    ! [remote rejected] master -> master (pre-receive hook declined)
    error: failed to push some refs to 'https://git.heroku.com/my-fuelwatch-app.git'


You got to add extra things to your project to satisfy heroku.


requirements.txt
----------------

One file named `requirements.txt` is required in the project root directory and must at least contain:

    Django
    gunicorn

These are the libraries that heroku will install when you deploy. So libraries that your fuel app relies on, like `feedparser` needs to be included as well. Note that the newest version of the libraries will install.


Procfile
--------

Another which is a file named `Procfile` (Note capital letter) in your project root directory, and it must contain the following line BUT sustitute `<PROJECT_APP>` with your 'project app' name (Note the deliberate '-' at the end of the line):

    web: gunicorn <PROJECT_APP>.wsgi --log-file -

But what is the 'project app' name?

Your file structure could look something this:

    my_django_project
    ├── db.sqlite3
    ├── main
    │   ├── __init__.py
    │   ├── settings.py
    │   ├── templates
    │   │   └── prices.html
    │   ├── urls.py
    │   ├── views.py
    │   └── wsgi.py
    ├── manage.py
    ├── Procfile
    └── requirements.txt

With the above example, `my_django_project` is the 'project' name, while `main` is the 'project app' name. Our example shows the 'project' and 'project app' to have the same name but it is typical for the 'project' name and 'project app' name to be the same.

Almost there
------------

You got to now remember to `git add` the Procfile and requirements.txt, or else those files are not pushed to Heroku.

Woops! not quite there
----------------------

Heroku is complaining about collectstatic, just out of laziness, just run:

    heroku config:set DISABLE_COLLECTSTATIC=1

Push Success! But...
--------------------

A successful push should give you the public URL you can visit. It's going to give you new errors though, but they are Django errors now. Run `heroku log --tail` to see the Python/Django errors.
