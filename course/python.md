# Data extraction, cleaning, and formatting

Now that we've spent some time experimenting with tools designed for an interpretive, hand-crafted mode of GIS work, let's move to the other end of the spectrum and work with some larger data sets. As a test project, we're going to be extracting "toponyms" - place references - from novels. It would take weeks or months to do this by hand - we'd have to read the entire novel, highlight each of the place references by hand, and then manually everything into some kind of database at the end.

Instead, we're going to use a piece of software called the Stanford Named Entity Recognizer to automatically pick out words or phrases in a text that are proper names. Then, we'll write a simple Python script that passes the extracted toponyms through a geocoding API, which takes the raw location names and converts them into latitude / longitude points that can be plotted on a map.

At the end of the process, we'll have a nicely-formatted CSV file that can be imported into visualization tools like Google Fusion Tables and CartoDB.

## Set up a Python development environment

First, we'll install the latest release of Python 3.

---

**MAC**

1. On Mac, the easiest way to do this is with a program called Homebrew, a package manager that automatically installs software on OSX. Go to http://brew.sh, and copy-and-paste the command on the front page into your terminal.

1. Install Python 3 with: `brew install python3`

1. Change back into the `Projects` directory: `cd ~/Projects`

**WINDOWS**

1. Go to http://python.org/downloads and click **Download Python 3.5.1**.

1. Once the installer finishes, open a command prompt and run these commands:

  ```sh
  setx PATH "%PATH%;C:\Users\<username>\AppData\Local\Programs\Python\Python35-32"
  setx PATH "%PATH%;C:\Users\<username>\AppData\Local\Programs\Python\Python35-32\Scripts"
  ```

  This will make the `python` and `pip` (the Python package manager) executables available from the command line.

1. Change back into the `Projects` directory: `cd C:\Users\<username>\Projects`

---

1. Create a new directory called `geotext`, which is what we'll call our program: `mkdir geotext`

1. Change down into the new directory with `cd geotext`.

Now, we'll create a "virtual environment," a set of files that wraps up a copy of Python and a set of dependencies that will be specific to this individual project.

---

**MAC**

1. Run: `pyvenv env` This will create a directory called `env` under `geotext`.

1. Activate the environment with: `. env/bin/activate`

1. Create a `requirements.txt` file, which will define our dependencies: `touch requirements.txt`

**WINDOWS**

1. Run: `python -m venv env`

1. Activate the environment with: `env\Scripts\activate.bat`

1. Open Atom with `atom .` and create a new file called `requirements.txt`.

---

1. Open up `requirements.txt` in Atom and enter `ipython` on the first line.

1. Then, run `pip install -r requirements.txt` to install the dependencies.

1. Now, if you type `ipython`, you should get dropped into an interactive Python shell.

****TODO: stanford NER

## Create a simple command-line program

Next, we'll scaffold out the files needed to create a command-line utility called `geotext`, which we'll use as a point entry for a set of scripts to extract place names, geocode them, and wrangle the data into different formats.

1. In the `geotext` directory, add a file called `setup.py`: `touch setup.py`

1. Open the file in Atom, and enter in some basic metadata about the project:

  ```python
  from setuptools import setup, find_packages

  setup(

      name='geotext',
      version='0.1.0',
      description='Extract toponyms from plain text.',
      license='MIT',
      author='David McClure',
      author_email='dclure@stanford.edu',
      packages=find_packages(),
      scripts=['bin/geotext'],

  )
  ```

  The important part here is the last two lines - `packages` and `scripts` - which tell Python where to find our code.

1. Back on the command line, create a new directory called `bin` - `mkdir bin`

1. Create a new file in that directory called `geotext` - `touch bin/geotext`

1. Set the permissions on the file to make it executable - `chmod 755 bin/geotext`

1. Open the new `geotext` file in Atom, and enter a basic hello world program as a test:

  ```python
  print('Hello world!')
  ```

1. Back on the command line, install the program with: `python setup.py develop`

1. Now, if you just run the command `geotext`, you should see - `Hello world!`

## Toponym extraction

Now, we're ready to sketch in the logic for the entity extraction and geocoding. We'll walk through this together, but here's the complete code for reference:

https://github.com/davidmcclure/hilt-2016/blob/master/geotext/bin/geotext