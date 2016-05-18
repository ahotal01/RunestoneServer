# Runestone Interactive Server and API

## Installation

1. Install python.

First, make sure you have Python 2.7 installed.  Web2py has not yet been ported to Python3.  Even if you don't care about the web2py part of the install, the version of paverutils on pypi is still a Python 2.x package, although the development version is now at 3.x.

There are a couple of prerequisites you need to satisfy before you can build and use this
eBook. The easiest/recommended way is to use `[pip](http://www.pip-installer.org/en/latest/)`.

You can simply install all dependencies by running the following command in main runestone directory:

  pip install -r requirements.txt

It is recommended that you install python in a virtualenv. To do that, [install virtualenv and virtualenvwrapper](http://docs.python-guide.org/en/latest/dev/virtualenvs/) per the linked instructions.

2. Install web2py. The easiest way to do so is to download the **Source Code** distribution from http://www.web2py.com/init/default/download.
[Here](http://www.web2py.com/examples/static/web2py_src.zip) is a direct link to the zip archive.
After you download it, extract the zip file to some folder on your hard drive. (web2py requires no real "installation"). Avoid the web2py.app installation on OS X as it messes with the Python path.

3. Clone this repository **into the web2py/applications directory**. When you make the clone you should clone it into runestone rather than the default RunestoneComponents: `git clone <repo_url> runestone`

All the web2py stuff is configured assuming that the application will be called runestone.

4. Clone the [thinkcspy](https://github.com/chrisbay/thinkcspy.git) book **into the web2py/applications/runestone/books** directory (this is a clone of the original, for modifying for LaunchCode usage). Other books are available at https://github.com/RunestoneInteractive.

5. Set up your local database

* Install postgreSQL

* Create a database

* Figure out your database connection string. It will be something like postgres://username:passwd@localhost/dbname'

Mac Installation

In `.bash_profile`:

  export PATH=$PATH:/Applications/Postgres.app/Contents/Versions/9.4/bin

At a command line:

  $ source .bash_rc
  $ createdb habitual
  $ psql

In postgres (`psql`):

  > CREATE ROLE launchclass WITH LOGIN PASSWORD 'toinfinityandbeyond';
  > GRANT ALL PRIVILEGES ON DATABASE runestone TO launchclass;

*The following steps are unnecessary if you are installing the app locally using the same user/db settings above.*

* Tell web2py to use that database
    * Create a file applications/runestone/models/1.py, with the following line: `settings.database_uri = <your_connection_string>`
        * NOTE: Don't put this inside an if statement, like it shows in models/1.prototype
    * If you're running https, edit settings.server_type in models/0.py
    * You will need a customization to runestone/modules/chapternames.py
        * (Note: hopefully, this will be fixed in the future so that it automatically reads from models/1.py)
        * In chapternames.py, where it sets dburl = a connection string, put your connection string there.

* Edit /applications/runestone/books/<yourbook>/pavement.py
    * set the master_url variable for your server, if not localhost

* Run web2py once, so that it will create all the tables (making sure you've activated your virtualenv)
    * cd web2py/
    * python web2py.py

* Build the book

* `cd web2py/applications/runestone/books/<your book>`

* `runestone build`

  * At the end, it should say `trying alternative database access due to  No module named pydal` and then, if things are working correctly, start outputting the names of the chapters.

* runestone deploy
    * The following may not be necessary. Check the static directory first to see if the book contents were already moved there.
        * `rm -r applications/runestone/static/<your book name>`
        * `cd runestone/books/<your book name>`
        * `mv build/<your book name> ../static/`

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



Documentation
-------------

Documentation for the project is on our official [documentation site](http://docs.runestoneinteractive.org>) This includes the list of dependencies you need to install in order to build the books included in the repository, or to set up a complete server environment.

The Runestone Tools are not only good for authoring the textbooks contained in this site, but can also be used for:

* Making your own lecture materials
* Making online quizzes for use in class
* Creating online polls for your course

More Documentation
------------------

I have begun a project to document the [Runestone Interactive](http://docs.runestoneinteractive.org/build/html/index.html) tools

* All of the Runestone Interactive extensions to sphinx:

    * Activecode -- Interactive Python in the browser
    * Codelens  -- Step through code examples and see variables change
    * mchoicemf  -- multiple choice questions with feedback
    * mchoicema  -- multiple choice question with multiple answers and multiple feedback
    * fillintheblank  -- fill in the blank questiosn with regular expression matching answers
    * parsonsproblem  -- drag and drop blocks of code to complete a simple programming assignment
    * datafile -- create datafiles for activecode

* How to write your own extension for Runestone Interactive


Creating Your Own Textbook
--------------------------

To find instructions on using the Runestone Tools to create your own interactive textbook, see the file in this directory named README_new_book.rst.


Browser Notes
-------------

Note, because this interactive edition makes use of lots of HTML 5 and Javascript I highly recommend either Chrome, or Safari.  Firefox 6+ works too, but has proven to be less reliable than the first two.  I have no idea whether this works at all under later versions of Internet Explorer.
