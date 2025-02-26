# On Python Environment Management
- [On Python Environment Management](#on-python-environment-management)
    - [Package Managers (General Purpose)](#package-managers-general-purpose)
    - [Python package and environment managers](#python-package-and-environment-managers)
    - [Docker](#docker)
    - [Simpler is better](#simpler-is-better)
    - [Project Structure](#project-structure)
    - [Virtual Environments](#virtual-environments)
      - [```flag -m```](#flag--m)
    - [Dependency Management](#dependency-management)
      - [```pyproject.toml```](#pyprojecttoml)
      - [```pip-tools```](#pip-tools)
    - [Version Control Management](#version-control-management)
      - [Git](#git)
    - [Environment variables management (```env```)](#environment-variables-management-env)
    - [Special python files](#special-python-files)
      - [```__init__.py```](#__init__py)
    - [Testing](#testing)
    - [Documentation](#documentation)


<details>
  <summary>concept disambiguation's litle corner</summary>

  >Python an "interpreted language" - ?? (interpreted/compiled is not a property of the language but a property of the implementation):
  1. Lexical analysis: python interpreter reads source code and breaks it down to tokens
  2. Parsing: tokens are organized into AST (abstract syntax tree)
  3. bytecode compilation -> AST is converted to bytecode
  4. PVM (Python Virtual Machine) - stack based machine -  executes the bytecode to give the final output
  ___
  - shims:
  - ```__init__.py```: a special Python file indicating the directory it's in is a Python package.
  - ```__main__```:
  - at runtime vs ahead of time compilation, just.in-time compiler
  >Packaging
  - **sdist**: source distribution (```package_name-version.tar.gz```, e.g., ```pip-23.3.1.tar.gz```)
  - **wheel**: binary distribution, a wheel contains exactly the files that need to be copied when installing the package
  - setuptools, 
  - setup.py: a configuration script used in Python projects to facilitate the distribution and installation of the package. setup.cfg and **pyproject.toml** are newer alternatives to it.
  - sys.path, sys.argv
  - environment variable:
  - package manager (key feature of homebrew, pip)
  - python version management (key feature of pyenv, asdf)
  - package manangement (key feature of conda, pipenv)
  - environment managemnet (key feature of conda, pipenv, venv)
  - containarization (key feature of docker)
  
</details>
<details>
  <summary>miscellaneous collections</summary>

### Package Managers (General Purpose)
- Homebrew (not the best option for python packages)
- Conda (Conda is a general-purpose package management system, designed to build and manage software of any type from any language. As such, it also works well with Python packages.)
__homebrew__  
Homebrew is a general purpose package manager, (which can also be used for python management) -> not recommended for python management because homebrew will try to update python version autonomously it may lead to unnecessary problems.

[reasons not to use homebrew for python management](https://justinmayer.com/posts/homebrew-python-is-not-for-you/)
- "assuming you use Homebrew to install other tools, deleting homebrew's python installation would be a fool’s errand — Homebrew Python will almost certainly be installed anyway when some other formula requires it. But that doesn’t mean we have to use it! Let the Homebrew Python sit there and do its job serving other formulae, and we can forget it exists."


__conda__  
[conda myths](https://jakevdp.github.io/blog/2016/08/25/conda-myths-and-misconceptions/) 

Conda is a package manager; Anaconda is a distribution. Although Conda is packaged with Anaconda, the two are distinct entities with distinct goals

### Python package and environment managers
[Python Env post](https://www.nijho.lt/post/python-environments/#-system-package-managers)

[how to set up a python project](https://cjolowicz.github.io/posts/hypermodern-python-01-setup/)

chat gpt:
1. version management
    - pyenv or asdf (asdf supports multiple language versions under a unified system. But for Python-only workflows, pyenv is simpler)
2. environment and dependencies management
    - micromamba + poetry

    > Micromamba creates fast environments and handles non-Python dependencies.

    >Poetry handles Python package management, lockfile management, and virtual environments seamlessly.
Combined, they offer a powerful, efficient, and reproducible setup for ML projects.
3. project configuration
    - VSCode integration 
    >powerful IDE with extensions that streamline Python development, and it works seamlessly with pyenv, micromamba, and poetry for environment management.
    - Install VSCode extensions:
      - Python (for linting, IntelliSense)
      - Docker (for container-based workflows)
      - Poetry (for managing dependencies within VSCode)  

4. Containerization: Use Docker to ensure consistency across different environments, especially for deployment.

5. Version control: Use Git and automate CI/CD pipelines with GitHub Actions for testing, training, and deployment.

6. Data management: Use DVC to version datasets and models.


### Docker
[reddit](https://www.reddit.com/r/datascience/comments/111djvu/what_do_you_use_to_manage_your_python_packages/)
- Docker, and I code in my container with VSCode remote-container. Basically changed my life.
- deploying a docker container on our custom SoC and remoting into it. No more cross compilation dogshit.
---

### Simpler is better
+ [Why not tell people to "simply" use pyenv, poetry, pipx or anaconda](https://www.bitecode.dev/p/why-not-tell-people-to-simply-use?ref=new.pythonforengineers.com)  
+ [simpler is better](https://www.bitecode.dev/p/relieving-your-python-packaging-pain?ref=new.pythonforengineers.com)

---
---
</details>
</details>

### Project Structure 
example:
<pre>
ML_Project/
├── data/              # Raw data or datasets (avoid version control)
├── notebooks/         # Jupyter notebooks for exploration
├── src/               # Source code (organized by modules)
│   ├── __init__.py
│   ├── data_preprocessing.py
│   ├── model_training.py
│   └── utils.py
├── tests/             # Unit tests and integration tests
├── venv/              # Virtual environment directory
├── pyproject.toml     # project metadata and high-level dependencies
├── requirements.txt   # pinned dependencies (generated by pip-tools or pip freeze > requirements.txt)
├── .env               # Environment variables (like API keys)
├── README.md          # Project documentation
└── .gitignore         # Ignore files like data/, venv/, etc.
</pre>



---
1. Don’t install the latest major version of Python
2. venv:
   ### Virtual Environments

    Don´t install or run anything outside a **virtual environment** and favor python.org installer (or official repositories on Linux).  
    When creating a virtual environment, be explicit about which Python you use.
    + to create a virtual env: ```python3.X -m venv .venv```  
    + to activate it: ```source .venv/bin/activate```
    + to deactivate it: ```deactivate```

   Limit yourself to the basics: ```pip``` and ```venv```  
   If you run a command, use the flag ```-m```
    <details> 
      <summary>-m flag</summary>

   #### ```flag -m```  
      [python -m <module>](https://snarky.ca/why-you-should-use-python-m-pip/)
      
      - ```-m``` is a flag on the “python” command that few users seem to know about. It lets you run any importable Python module, no matter where you are. 

      - ```python -m pip install``` ensures that you're using the pip module associated with the exact Python interpreter that you are calling (e.g., the one from a virtual environment or a specific Python version).

      - Running ```python3.9 -m pip install '<package'>``` ensures that the package gets installed in the environment tied to Python 3.9, rather than potentially installing it for a different Python version.

      e.g.:

      Don't do: ```pip install```
          
      Do: ```python -m pip install```

      Don't do: ```black```

      Do: ```python -m black```

      Don't do: ```jupyter notebook```
      
      Do: ```python -m jupyter notebook```
    
  </details>

3. Managing dependencies
### Dependency Management

  1. Define high-level (unpinned or without reference to versions) dependencies in: 
#### ```pyproject.toml```
  [writing pyproject.toml](https://packaging.python.org/en/latest/guides/writing-pyproject-toml/),
  [Python and TOML](https://realpython.com/python-toml/)  
  
  2. Generate requirements.txt using 
  
  #### ```pip-tools```

  <details>
      <summary>workflow details</summary>

  + Define unpinned dependencies and project metadata in ```pyproject.toml``` while keeping flexibility in development.
  + Use pip-tools for versioning: 
    ```bash
    python -m piptools compile \
      -o requirements.txt \
      pyproject.toml
    ``` 
    and to **pin the development dependencies** 
    from the pyproject.toml (in the ```[project.optional-dependencies]``` TOML table):

    ```zsh
    python -m piptools compile \
      --extra dev \
      -o dev-requirements.txt \
      pyproject.toml
    ```
    or 
    ```pip-compile --generate-hashes```  

    allows you to pin exact versions for deployment and ensures package integrity with hashes.  

      **or**  

    + Create a requirements.txt file for dependencies:  
```pip freeze > requirements.txt```
    + `pip-tools`: a set of command line tools to help you keep your pip-based packages fresh, even when you've pinned them. 
    </details>


    <details>
      <summary>about pip-tools</summary>

      When you pin a dependency, you specify an exact version of a package. 
      This ensures that your project always uses the same version, avoiding potential issues that may arise if newer versions introduce breaking changes.
      It consists of two main utilities:

      1. pip-compile:
      - Generates a requirements.txt file with pinned versions of all dependencies, including any transitive dependencies (dependencies of your dependencies).
      - You maintain a separate file called requirements.in where you list the top-level dependencies without worrying about their versions.
        - Default example workflow with pip-compile:

      Create a requirements.in file with unpinned dependencies:
      ```txt
      numpy
      pandas
      requests
      ```
      Run the pip-compile command to generate a requirements.txt with pinned versions:
      ```zsh
      python -m piptools compile
      ```
      or  
      
      ```pip-tools compile```

      This will create a requirements.txt file with exact, pinned versions, including dependencies of the packages listed in requirements.in:

      ```txt
      # Generated by pip-compile
      numpy==1.21.0
      pandas==1.3.0
      requests==2.25.1
      ```
      + **pip-tools** also supports pyproject.toml (not only the default requirements.in):  
      [pip-tools with pyproject.toml](https://hynek.me/til/pip-tools-and-pyproject-toml/)
  
      1. pip-sync:
      - ```pip-sync``` synchronizes your current Python environment with the pinned dependencies listed in requirements.txt. It ensures that the exact versions in the requirements.txt are installed and **removes any packages not listed**.  
      ```python -m piptools sync requirements.txt```
      **or** 
      - ```python -m pip install -r requirements.txt``` (standard pip functionality that adds missing packages abut **leaves extras**)
  </details>

### Version Control Management
  #### Git

  additional sources | link
  | --- | --- |
  book | [pro git](https://git-scm.com/book/en/v2) |
  mooc | [MIT](https://missing.csail.mit.edu/2020/version-control/#snapshots) |
  other | [git for CS](https://eagain.net/articles/git-for-computer-scientists/) |

  1. Initialize a Git repository for the project:
  ```bash
  git init
  git add .
  git commit -m "Initial commit"
  ```  
  2. Push to a remote repository like GitHub for versioning and collaboration.
  ```bash
  # Add the remote GitHub repository:
  git remote add origin https://github.com/your-username/your-repository.git
  # Push to GitHub:
  git push -u origin master
  ```

  >Basics:
  - `git help <command>`: get help for a git command
  - ```git init```: creates a new git repo, with data stored in the .git directory
  - ```git status```: tells you what’s going on
  - ```git add <filename>```: adds files to staging area
  - ```git commit```: creates a new commit
  - `git log`: shows a flattened log of history
  - `git log --all --graph --decorate`: visualizes history as a DAG
  - `git diff <filename>`: show changes you made relative to the staging area
  - `git diff <revision> <filename>`: shows differences in a file between snapshots
  - `git checkout <revision>`: updates HEAD and current branch

  >Branching and merging:
  - ```git branch```: shows branches
  - ```git branch <name>```: creates a branch
  - ```git checkout -b <name>```: creates a branch and switches to it
  (same as git branch \<name>; git checkout <name>)
  - ```git merge <revision>```: merges into current branch
  - `git mergetool`: use a fancy tool to help resolve merge conflicts
  - `git rebase`: rebase set of patches onto a new base
  
  >Remotes:
  - `git remote`: list remotes
  - `git remote add \<name> \<url>`: add a remote
  - `git push <remote> <local branch>:<remote branch>`: send objects to remote, and update remote reference
  - `git branch --set-upstream-to=<remote>/<remote branch>`: set up correspondence between local and remote branch
  - `git fetch`: retrieve objects/references from a remote
  - `git pull`: same as git fetch; git merge
  - `git clone`: download repository from remote
  
  >Undo:
  - `git commit --amend`: edit a commit’s contents/message
  - `git reset HEAD <file>`: unstage a file
  - `git checkout -- <file>`: discard changes

- **.gitignore** file:
  specify intentionally untracked files to ignore (e.g., virtual environments, data files):
  ```
  # .gitignore
  venv/
  __pycache__/
  .env
  data/
  ```
### Environment variables management (```env```)
  Store sensitive information like API keys in a .env file:
  ```touch .env```  
  Use the dotenv package to load environment variables:
  ```python
  from dotenv import load_dotenv
  load_dotenv()
  ```
### Special python files
  #### ```__init__.py```
  - The ```__init__.py``` file is a special Python file used to indicate that a directory is a **Python package**. It can be an empty file or contain initialization code for the package.
  
### Testing
  - Unit Tests: Write tests for critical parts of the codebase using unittest or pytest.
  - Organize your tests in a tests/ folder.
 
### Documentation
  - README.md: Use it to describe the project, how to set it up, and run it



---


