# Runestone Interactive Server and API

## Installation

### Install python

First, make sure you have Python 2.7 installed.  Web2py has not yet been ported to Python3.  Even if you don't care about the web2py part of the install, the version of paverutils on pypi is still a Python 2.x package, although the development version is now at 3.x.

There are a couple of prerequisites you need to satisfy before you can build and use this
eBook. The easiest/recommended way is to use [pip](http://www.pip-installer.org/en/latest/).

If you're using a Mac, we highly recommend installing [Homebrew](http://brew.sh) and using it to install python (which also installs pip), rather than using the built-in installation.

You should set up a virtualenv for the app. To do that, [install virtualenv](http://docs.python-guide.org/en/latest/dev/virtualenvs/) per the linked instructions. If you've never used virtualenv before, consult the [documentation](https://virtualenv.pypa.io/en/stable/) as needed.

### Install web2py

The easiest way to do so is to download the source code distribution from the [web2py site](http://www.web2py.com/init/default/download). After you download it, extract the zip file to a folder on your hard drive (web2py requires no real "installation"). Avoid the web2py.app installation on OS X, as it messes with the Python path.

Clone this repository -- that is, the RunestoneSever -- into `web2py/applications`. Clone it into runestone rather than the default RunestoneServer: `git clone <repo_url> runestone`

All the web2py stuff is configured assuming that the application will be called runestone.

Clone the [thinkcspy](https://github.com/LaunchCodeEducation/thinkcspy) book into `web2py/applications/runestone/books` (this is a clone of the original, modified for LaunchCode classes). Other books are available from [Runestone Interactive](https://github.com/RunestoneInteractive).

### Install app dependencies

You can install all dependencies by running the following command in main runestone directory. Make sure you have activated your virtualenv: `pip install -r requirements.txt`. If this yields an error, make sure your path is set to a version of postgreSQL that is supported by pytz. To check this, run `pg_config --version`.

### Set up your local database

* Install postgreSQL

* Create a database

* Figure out your database connection string. It will be something like `postgresql://username:passwd@localhost/dbname`

#### Mac Installation

Install the [Postgres app](http://postgresapp.com).

In `~/.bash_profile` add:

    export PATH=$PATH:/Applications/Postgres.app/Contents/Versions/9.6/bin

To ensure that this version is provided in the Postgres installation, locate the listed folder and make sure the number used in the above PATH is there. If the listed versions differ, replace `9.6` with one such listing. At a command line:

    $ source ~/.bash_profile
    $ createdb runestone
    $ psql

In postgres (`psql`):

    > CREATE ROLE launchclass WITH LOGIN PASSWORD 'toinfinityandbeyond';
    > GRANT ALL PRIVILEGES ON DATABASE runestone TO launchclass;

*The following steps are unnecessary if you are installing the app locally using the same user/db settings above.*

* Tell web2py to use that database
    * Create a file `applications/runestone/models/1.py`, with the following line: `settings.database_uri = <your_connection_string>`
        * NOTE: Don't put this inside an if statement, like it shows in `models/1.prototype`
    * If you're running https, edit `settings.server_type` in `models/0.py`
    * You will need a customization to `runestone/modules/chapternames.py`
        * (Note: hopefully, this will be fixed in the future so that it automatically reads from `models/1.py`)
        * In `chapternames.py`, where it sets the value of `dburl` to a connection string, place the value of your connection string.

* Edit `/applications/runestone/books/<yourbook>/pavement.py`
    * set the `master_url` variable for your server, if not `localhost`

* Run web2py once, so that it will create all the tables (making sure you've activated your virtualenv)
    * `cd web2py/`
    * `python web2py.py`

### Build the book

* `cd web2py/applications/runestone/books/<your book>`

* `runestone build`

  * At the end, it should say `trying alternative database access due to No module named pydal` and then, if things are working correctly, start outputting the names of the chapters.

* Deploy the book
    * The following may not be necessary. Check the static directory first to see if the book contents were already moved there. From `runestone/books/<your book name>`:
        * `rm -r ../../static/<your book name>`
        * `mv build/<your book name> ../../static/`

* Create an account for yourself
    * restart web2py if it's not running
    * go to `runestone/appadmin`
    * create a course for the book
        * insert new courses
        * course_id can be blank
        * course name should be your book name, the directory name inside books/ (no spaces)
        * date is in format 2015-08-29
        * institution doesn't matter
        * base course should be same as course name
    * create an account for yourself
        * insert new auth_user
        * cohort id should be "id"
        * Course name should be the course name from above (not a number)
        * Do *not* make up a registration key or a reset password key; leave them blank
    * make yourself the instructor for the course
        * insert new course_instructor
        * Course is the *number* for the course (probably 5 if you just inserted one additional course)

# Documentation

There is an official [documentation site](http://runestoneinteractive.org/build/html/index.html) created by the original development team.
