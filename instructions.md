# Wordle Project

## Project Setup

For this Project I will be using Poetry for dependency management, instead of the typical pip/venv/pipenv/virtualenv/requirements.txt file.

### Introduction to Poetry

Poetry is a tool for dependency management and packaging in Python.
- Allows you to declare the libraries you project depends on
    - Will install/update them for you
- Can build your project for distribution
- Offers a lockfile to ensure repeatable installs


### Install Poetry

Instructions:
1. Install pipx
- Download Scoop (via PowerShell)

```powershell
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
Invoke-RestMethod -Uri https://get.scoop.sh | Invoke-Expression
```

- Install pipx via Scoop

```powershell
scoop install pipx
pipx ensurepath
```

- Restart Powershell (so that pipx is ready to go)

- Check version of pipx

```powershell
pipx --version
```

For more help, here are the official pipx installation instructions: https://pipx.pypa.io/stable/installation/

Note: pipx is used to install Python CLI applications globally while still isolating them in virtual environments. pipx will manage upgrades and uninstalls when used to install Poetry.

2. Install Poetry
- Install Poetry in the terminal via pipx

```powershell
pipx install poetry
```

### Project Setup

Instructions:
1. Create our new project directory
- Name of project: wordle

```powershell
poetry new wordle
```

Note: This will create the `wordle` directory with the following content:

```
poetry-demo
├── pyproject.toml
├── README.md
├── poetry_demo
│   └── __init__.py
└── tests
    └── __init__.py
```

2. Operating modes

The pyproject.toml file is what is the most important here. This will orchestrate your project and its dependencies. For now, it looks like this:

```
[tool.poetry]
name = "poetry-demo"
version = "0.1.0"
description = ""
authors = ["Sébastien Eustace <sebastien@eustace.io>"]
readme = "README.md"
packages = [{include = "poetry_demo"}]

[tool.poetry.dependencies]
python = "^3.7"


[build-system]
requires = ["poetry-core"]
build-backend = "poetry.core.masonry.api"
```

Poetry can be operated in two different modes. The default mode is the package mode, which is the right mode if you want to package your project into an sdist or a wheel and perhaps publish it to a package index.

To use Poetry only for dependency management but not for packaging, you can use the non-package mode:

```
[tool.poetry]
package-mode = false
```

3. Specifying dependencies

If you want to add dependencies to your project, you can specify them in the tool.poetry.dependencies section.

```
[tool.poetry.dependencies]
django = "^5.0.4"
```

Also: instead of modifying the pyproject.toml file by hand, you can use the add command. (It will automatically find a suitable version constraint and install the package and sub-dependencies.)

```powershell
cd wordle

poetry add django
```

4. Using your virtual environment

By default, Poetry creates a virtual environment in {cache-dir}/virtualenvs. You can change the cache-dir value by editing the Poetry configuration. Additionally, you can use the virtualenvs.in-project configuration variable to create virtual environments within your project directory.

There are several ways to run commands within this virtual environment.
- poetry run: 

5. Activating the virtual environment

The easiest way to activate the virtual environment is to create a nested shell with `poetry shell`

To deactivate the virtual environment and exit this new shell type `exit`. To deactivate the virtual environment without leaving the shell use `deactivate`.

Note: I have been using `exit`

6. Installing dependencies

To install the defined dependencies for your project, just run the install command.

```powershell
poetry install
```

When you run this command, one of two things may happen:
- Installing without poetry.lock:
    - If you have never run the command before and there is also no poetry.lock file present, Poetry simply resolves all dependencies listed in your pyproject.toml file and downloads the latest version of their files.
    - When Poetry has finished installing, it writes all the packages and their exact versions that it downloaded to the poetry.lock file, locking the project to those specific versions. You should commit the poetry.lock file to your project repo so that all people working on the project are locked to the same versions of dependencies (more below).

- Installing with poetry.lock:
    - If there is already a poetry.lock file as well as a pyproject.toml file when you run poetry install, it means either you ran the install command before, or someone else on the project ran the install command and committed the poetry.lock file to the project (which is good).

Either way, running install when a poetry.lock file is present resolves and installs all dependencies that you listed in pyproject.toml, but Poetry uses the exact versions listed in poetry.lock to ensure that the package versions are consistent for everyone working on your project. As a result you will have all dependencies requested by your pyproject.toml file, but they may not all be at the very latest available versions (some dependencies listed in the poetry.lock file may have released newer versions since the file was created). This is by design, it ensures that your project does not break because of unexpected changes in dependencies.

7. Committing your poetry.lock file to version control

As an application developer: Application developers commit poetry.lock to get more reproducible builds.

Head to Github and create a new remote repository named `wordle`.

Following the creation of your new remote repository, create a new local repository on the command line:

```
cd wordle

echo "# wordle" >> README.md
git init
git add README.md
git commit -m "first commit"
git branch -M main
git remote add origin https://github.com/MylesThomas/wordle.git
git push -u origin main
```

Create a .gitignore file [with the following code](https://github.com/github/gitignore/blob/main/Python.gitignore) and save it in the root directory:

```
echo > .gitignore
```

Update the README.md file:

```
# Wordle

Creating a customized Wordle Game. More updates to come soon!

```

Push your entire project's changes to Github:

```
git status
git add .
git commit -m "Completing project setup"
git push -u origin main
```

Committing this file to VC is important because it will cause anyone who sets up the project to use the exact same versions of the dependencies that you are using. Your CI server, production machines, other developers in your team, everything and everyone runs on the same dependencies, which mitigates the potential for bugs affecting only some parts of the deployments. Even if you develop alone, in six months when reinstalling the project you can feel confident the dependencies installed are still working even if your dependencies released many new versions since then. (See note below about using the update command.)

8. Updating dependencies to their latest versions

As mentioned above, the poetry.lock file prevents you from automatically getting the latest versions of your dependencies.

To update to the latest versions, use the update command.

```
poetry update
```

This will fetch the latest matching versions (according to your pyproject.toml file) and update the lock file with the new versions. (This is equivalent to deleting the poetry.lock file and running install again.)

Now that our project directory/git environment is setup, we can begin with our project.

---







---

# References

1. [Using environment variables in `pyproject.toml` for versioning](https://stackoverflow.com/questions/74968585/using-environment-variables-in-pyproject-toml-for-versioning)
2. [Writing your pyproject.toml](https://packaging.python.org/en/latest/guides/writing-pyproject-toml/#writing-pyproject-toml)
3. [Poetry - Basic usage](https://python-poetry.org/docs/basic-usage/)
4. [PEP 517 – A build-system independent format for source trees](https://peps.python.org/pep-0517/)
5. []()