# Mastering Python Package Development in 1 Day

**Focusing on Modern Essentials to Get You Publishing**

---

**Note:** This guide reflects Python packaging best practices as of early 2025, emphasizing `pyproject.toml` for configuration and standard tools like `build` and `twine`.

---

## Table of Contents

- [Mastering Python Package Development in 1 Day](#mastering-python-package-development-in-1-day)
  - [Table of Contents](#table-of-contents)
  - [Introduction: Why Package Your Python Code?](#introduction-why-package-your-python-code)
  - [Chapter 1: Pre-Course Preparation (Setting the Stage)](#chapter-1-pre-course-preparation-setting-the-stage)
    - [Install Tools \& Set Up Your Environment](#install-tools--set-up-your-environment)
    - [Quick Overview: Packaging Explained Simply](#quick-overview-packaging-explained-simply)
  - [Chapter 2: Morning Session: Fundamentals \& Setting Up Your Package](#chapter-2-morning-session-fundamentals--setting-up-your-package)
    - [2.1 Understanding Python Packaging (9:00 – 9:30 AM)](#21-understanding-python-packaging-900--930-am)
    - [2.2 Creating a Basic Package Structure (9:30 – 10:15 AM)](#22-creating-a-basic-package-structure-930--1015-am)
    - [2.3 Short Break (10:15 – 10:30 AM)](#23-short-break-1015--1030-am)
    - [2.4 Configuring Your Package (`pyproject.toml`) (10:30 – 11:15 AM)](#24-configuring-your-package-pyprojecttoml-1030--1115-am)
    - [2.5 Additional Files \& Documentation (11:15 – 12:00 PM)](#25-additional-files--documentation-1115--1200-pm)
  - [Chapter 3: Lunch Break (12:00 – 1:00 PM)](#chapter-3-lunch-break-1200--100-pm)
  - [Chapter 4: Afternoon Session: Building, Testing, and Publishing Your Package](#chapter-4-afternoon-session-building-testing-and-publishing-your-package)
    - [4.1 Building Your Package Distributions with `build` (1:00 – 1:45 PM)](#41-building-your-package-distributions-with-build-100--145-pm)
    - [4.2 Testing Your Package Locally (1:45 – 2:30 PM)](#42-testing-your-package-locally-145--230-pm)
    - [4.3 Short Break (2:30 – 2:45 PM)](#43-short-break-230--245-pm)
    - [4.4 Publishing to Test PyPI with `twine` (2:45 – 3:30 PM)](#44-publishing-to-test-pypi-with-twine-245--330-pm)
    - [4.5 Installing from Test PyPI (3:30 – 4:15 PM)](#45-installing-from-test-pypi-330--415-pm)
    - [4.6 Troubleshooting, Best Practices \& Next Steps (4:15 – 5:00 PM)](#46-troubleshooting-best-practices--next-steps-415--500-pm)
  - [Chapter 5: Wrap-Up (5:00 – 5:15 PM)](#chapter-5-wrap-up-500--515-pm)
    - [Recap: What You Achieved](#recap-what-you-achieved)
    - [Next Steps on Your Packaging Journey](#next-steps-on-your-packaging-journey)
    - [Final Thoughts](#final-thoughts)
  - [Conclusion: Happy Packaging!](#conclusion-happy-packaging)

---

## Introduction: Why Package Your Python Code?

Welcome to the updated "Mastering Python Package Development: The 80/20 Guide"! If you write Python code you want to reuse, share, or simply organize better, this guide is for you.

We follow the **Pareto Principle**: focus on the vital 20% of modern packaging practices that deliver 80% of the results. We'll use **layman's terms** and our trusty recipe binder analogy: your code is the recipe, packaging is organizing it with instructions and metadata (`pyproject.toml`), a cover (`README.md`), legal notes (`LICENSE`), and creating shareable copies (distributions) installable via `pip`.

This guide centers on **current best practices (early 2025)**, primarily using the `pyproject.toml` file for configuration and standard tools like `build` and `twine`. By the end, you'll have built, tested, and pseudo-published a simple, modern Python package.

---

## Chapter 1: Pre-Course Preparation (Setting the Stage)

Let's get your development kitchen ready with the latest tools.

### Install Tools & Set Up Your Environment

1.  **Python:** Ensure you have a recent Python version (3.7+ recommended for broad compatibility, check `python --version`). Get it from [python.org](https://www.python.org/). This includes `pip`.
2.  **Essential Packaging Tools:** Install the core tools using pip in your terminal:
    ```bash
    pip install build twine wheel # setuptools is usually included with pip/python
    ```
    *   `build`: The **standard tool** for building package distributions (`sdist` and `wheel`) from your source code, using configuration from `pyproject.toml`.
    *   `twine`: The **standard tool** for securely uploading your built packages to PyPI or Test PyPI.
    *   `wheel`: Enables the building of `.whl` files (though `build` handles invoking it).
    *   `setuptools`: The most common *build backend* (the library that does the actual work under `build`). It's often bundled, but explicit mention is useful. We'll configure it via `pyproject.toml`.

3.  **Test PyPI Account:** For practice uploads, sign up for a free account at [Test PyPI](https://test.pypi.org/). Keep your username and password (or ideally, an API token) handy.

### Quick Overview: Packaging Explained Simply

*   **Your Python Code:** Your valuable functions and classes (the recipes).
*   **Packaging:** Organizing code, defining metadata (ingredients, author info in `pyproject.toml`), adding documentation (`README.md`, `LICENSE`), and creating installable files (`.whl`, `.tar.gz`) using the `build` tool.
*   **`pip install your-package`:** Anyone grabbing your standardized package from the shelf (PyPI) and instantly adding it to their toolkit.

**Goals:** Organization, Reusability, Shareability, Dependency Management.

---

## Chapter 2: Morning Session: Fundamentals & Setting Up Your Package

Let's build our package structure and define its core configuration using modern standards.

### 2.1 Understanding Python Packaging (9:00 – 9:30 AM)

**Topics:**

*   **What is a Python Package?** A directory with Python modules (`.py` files) and an `__init__.py` file (can be empty) marking it as a package. It's bundled with metadata and supporting files for distribution.
*   **Why Package?** Organize, reuse code easily (`pip install`), share publicly (PyPI) or privately, manage dependencies automatically.

**Layman’s Terms:**

Like a recipe binder. `__init__.py` is the binder's spine. Packaging avoids scattered recipe notes, allowing easy sharing and reuse.

**Activity:** Think about packages you use (`requests`, `pandas`). You `import` them because they are packaged.

### 2.2 Creating a Basic Package Structure (9:30 – 10:15 AM)

**Topics:**

*   **Modern Directory Layout:**

    ```plaintext
    my_project_root_folder/
    ├── src/                  <-- Optional, but recommended: Source layout
    │   └── my_package/       <-- Your actual Python package source code
    │       ├── __init__.py   <-- Marks 'my_package' as a package
    │       └── greetings.py  <-- A module within your package
    ├── LICENSE               <-- Your package's license (e.g., MIT)
    ├── README.md             <-- Description, usage instructions
    ├── pyproject.toml        <-- **Primary Configuration File** (build system, metadata)
    └── (Optional) tests/     <-- Folder for automated tests
    ```
    *   **`src/` Layout (Recommended):** Placing your main package (`my_package`) inside a `src` directory helps prevent common import errors and clearly separates source code from project root files like `pyproject.toml`. We'll use this structure. If you prefer the flatter layout (without `src/`), adjust paths in `pyproject.toml` accordingly.
    *   **`pyproject.toml`:** The **central file** defined by PEP 518 and PEP 621. It tells tools like `pip` and `build` *how* to build your package and contains all the package metadata (name, version, dependencies, etc.). This largely replaces the need for `setup.py` for configuration, though a minimal `setup.py` might sometimes be needed for specific advanced `setuptools` features or editable installs with older tools (less common now).
    *   Other files (`LICENSE`, `README.md`, `__init__.py`, `greetings.py`) serve the same purpose as before.

**Hands-On Exercise:**

1.  Create a root folder: `my_package_project`.
2.  Inside `my_package_project`, create:
    *   A folder named `src`.
    *   Inside `src`, create a folder named `my_package`.
    *   Inside `src/my_package`, create an empty file `__init__.py`.
    *   Inside `src/my_package`, create `greetings.py`.
    *   Inside `my_package_project` (the root), create empty files `README.md`, `LICENSE`, and `pyproject.toml`.
3.  Add code to `greetings.py`:
    ```python
    # src/my_package/greetings.py
    def say_hello():
        """Returns a friendly greeting."""
        return "Hello, World from my_package!"
    ```
4.  Make the function accessible in `__init__.py`:
    ```python
    # src/my_package/__init__.py
    from .greetings import say_hello

    __version__ = "0.1.0" # Define version here or (preferably) in pyproject.toml
    __all__ = ['say_hello']
    ```
    *(Note: Defining `__version__` here is an option, but PEP 621 recommends defining it centrally in `pyproject.toml`)*.

**Layman’s Terms:**

Setting up the binder:
*   `src/my_package/`: The "Recipes" tab inside a dedicated "Source Code" section (`src`).
*   `__init__.py`: The tab divider.
*   `greetings.py`: First recipe card.
*   `README.md`, `LICENSE`: Cover page and legal notes.
*   `pyproject.toml`: The main index card slot at the front – we'll fill this next.

### 2.3 Short Break (10:15 – 10:30 AM)

Coffee time! Let the modern structure settle in.

### 2.4 Configuring Your Package (`pyproject.toml`) (10:30 – 11:15 AM)

**Topics:**

*   **`pyproject.toml`: The Standard:** This TOML file is the modern standard for configuring Python project builds and metadata. Tools like `build` and `pip` read this file directly.
*   **Key Sections:**
    *   **`[build-system]` (PEP 518):** Tells tools *what* software is needed to build your package. For most standard packages using `setuptools`, this is fairly standard.
        *   `requires`: A list of packages needed for the build (e.g., `setuptools`, `wheel`).
        *   `build-backend`: Specifies the Python object that build tools will use (usually provided by `setuptools`).
    *   **`[project]` (PEP 621):** Contains the core metadata about your package. This replaces most of the `setup()` arguments from the old `setup.py`.
        *   `name`: Distribution name (use hyphens), what users `pip install`.
        *   `version`: Package version (e.g., "0.1.0"). **Crucial: Define this here.**
        *   `authors`: List of authors (name and email).
        *   `description`: Short, one-line summary.
        *   `readme`: Path to your README file (e.g., "README.md"). Specifies content type automatically.
        *   `requires-python`: Minimum Python version (e.g., ">=3.7").
        *   `license`: Specify license, often linking to the file (e.g., `{ file="LICENSE" }`).
        *   `classifiers`: List of PyPI classifiers (same as before).
        *   `dependencies`: List of runtime dependencies (replaces `install_requires`). E.g., `["requests>=2.20.0", "importlib-metadata; python_version<'3.8'"]`.
    *   **`[project.urls]` (Optional):** Links like Homepage, Bug Tracker, Repository.
    *   **`[project.scripts]` (Optional):** Define command-line scripts (replaces `entry_points` `console_scripts`).

**Example `pyproject.toml`:**

Add the following content to your `pyproject.toml` file in the `my_package_project` root:

```toml
# pyproject.toml

[build-system]
requires = ["setuptools>=61.0"] # Specify minimum setuptools version
build-backend = "setuptools.build_meta"

[project]
name = "my-simple-package" # Distribution name on PyPI
version = "0.1.0"         # Authoritative version number
authors = [
  { name="Your Name", email="your.email@example.com" },
]
description = "A simple Python package that says hello (updated)"
readme = "README.md"
requires-python = ">=3.7"
license = { file="LICENSE" }
classifiers = [
    "Development Status :: 3 - Alpha",
    "Intended Audience :: Developers",
    "License :: OSI Approved :: MIT License",
    "Operating System :: OS Independent",
    "Programming Language :: Python :: 3",
    "Programming Language :: Python :: 3.7",
    "Programming Language :: Python :: 3.8",
    "Programming Language :: Python :: 3.9",
    "Programming Language :: Python :: 3.10",
    "Programming Language :: Python :: 3.11",
    "Programming Language :: Python :: 3.12", # Add newer versions
    "Topic :: Software Development :: Libraries :: Python Modules",
]
dependencies = [
    # Add runtime dependencies here, e.g.:
    # "requests>=2.25.0",
    # "tomli>=1.0; python_version<'3.11'", # Example conditional dependency
]

# Optional: Specify where setuptools should look for packages
# If using src layout, this might not be strictly needed as setuptools often finds it,
# but explicit is better than implicit.
[tool.setuptools.packages.find]
where = ["src"]  # Look for packages in the src directory

[project.urls]
"Homepage" = "https://github.com/yourusername/my_package_project"
"Bug Tracker" = "https://github.com/yourusername/my_package_project/issues"

# Optional: Define console scripts
# [project.scripts]
# my-hello-cmd = "my_package.cli:main_function"
```

**Note on `setup.py`:** For this simple package defined entirely with `pyproject.toml`, you **do not need a `setup.py` file at all!** The `build` tool and `pip` will use `pyproject.toml`. If you needed complex build logic or C extensions, you might still use a `setup.py` acting as the build script invoked by `setuptools`, but the metadata should stay in `pyproject.toml`.

**Layman’s Terms:**

`pyproject.toml` is now the official, standardized index card for your recipe binder.
*   `[build-system]`: Tells the librarian (build tools) *which machines* (setuptools) to use to copy your binder.
*   `[project]`: Contains all the key info: Title (`name`), Edition (`version`), Chef (`authors`), Summary (`description`), Full Intro (`readme`), Required Kitchen Version (`requires-python`), Legal Stuff (`license`), Categories (`classifiers`), and Other Required Recipes (`dependencies`).
*   `[tool.setuptools.packages.find]`: Explicitly tells the copying machine where to find the actual recipe pages (`src` folder).

### 2.5 Additional Files & Documentation (11:15 – 12:00 PM)

**Topics:**

*   **`README.md`:** Still vital. Explain what, install, usage. Ensure the `pip install` command uses the `name` from `pyproject.toml` (`my-simple-package`). Ensure the `import` statement uses the actual package directory name (`my_package`).
*   **`LICENSE`:** Still essential. Choose an OSI-approved license (e.g., MIT) and place the text in the `LICENSE` file. Reference it correctly in `pyproject.toml` (`license = { file="LICENSE" }`).
*   **`MANIFEST.in` (Less Common Now):** While `setuptools` (configured via `pyproject.toml`) has better auto-detection for data files *within* the package directory (using `[tool.setuptools] include-package-data = true` and potentially `package-data` settings in `pyproject.toml`), `MANIFEST.in` is still the mechanism used specifically for adding extra files (like global templates, data files outside the package dir, examples) to the **Source Distribution (sdist)**. For simple pure-Python packages, you often won't need it if your README and LICENSE are correctly specified in `pyproject.toml`.

**Hands-On Exercise:**

1.  **Update/Create `README.md`:** Fill it with explanation, installation (`pip install my-simple-package`), and usage (`import my_package`).
2.  **Add `LICENSE`:** Copy/paste MIT license text (or your choice) into the `LICENSE` file, updating year/name.

**Layman’s Terms:**

Same as before: Update the binder's cover page (`README.md`) and the legal disclaimer (`LICENSE`). `MANIFEST.in` is like a special note for the source-code-only copy machine (sdist builder) if it needs to grab extra flyers or notes kept outside the main recipe section.

---

## Chapter 3: Lunch Break (12:00 – 1:00 PM)

Break time! You've set up a modern Python package structure and configuration.

---

## Chapter 4: Afternoon Session: Building, Testing, and Publishing Your Package

Let's build using the standard tools, test locally, and publish to our practice repository.

### 4.1 Building Your Package Distributions with `build` (1:00 – 1:45 PM)

**Topics:**

*   **Distribution Formats:** Still `sdist` (`.tar.gz`) and `wheel` (`.whl`). Wheels are preferred for installation speed and ease.
*   **The `build` Tool:** This is the **standard, recommended** command-line tool for building packages. It reads `pyproject.toml`, sets up an isolated build environment, and invokes the specified build backend (`setuptools` in our case) to create the `sdist` and `wheel`. **Do not use `python setup.py sdist/bdist_wheel` anymore.**

**Hands-On Exercise:**

1.  Open your terminal in the `my_package_project` root directory (where `pyproject.toml` is).
2.  Run the `build` command:
    ```bash
    python -m build
    ```
    *(You might see `-m build --wheel` used sometimes if you only want the wheel, but the default builds both)*.
3.  Observe the output. It will show steps like setting up an isolated environment, getting build dependencies, and finally building the sdist and wheel.
4.  Check the `dist/` folder. It should contain:
    *   `my_simple_package-0.1.0.tar.gz` (sdist)
    *   `my_simple_package-0.1.0-py3-none-any.whl` (wheel)

**Layman’s Terms:**

You used the modern, official packaging machine (`python -m build`). It read the main index card (`pyproject.toml`), automatically gathered the right tools, and produced two packages in the shipping department (`dist/`):
*   The source kit (`.tar.gz`)
*   The pre-assembled, ready-to-use binder (`.whl`).

### 4.2 Testing Your Package Locally (1:45 – 2:30 PM)

**Topics:**

*   **Importance:** Crucial check before uploading.
*   **Virtual Environments (`venv`):** Essential for clean testing.
*   **Installation:** Use `pip install .` which now reads `pyproject.toml` to install your package correctly.

**Hands-On Exercise:**

1.  **Create and activate a new virtual environment:**
    ```bash
    python -m venv test_env
    source test_env/bin/activate  # Or .\test_env\Scripts\activate on Windows
    ```
2.  **Install your package from the current directory:** (Make sure you are in `my_package_project` root)
    ```bash
    pip install .
    ```
    *(Alternatively, you could install directly from the wheel: `pip install dist/my_simple_package-0.1.0-py3-none-any.whl`)*
3.  **Test the installed package in the Python interpreter:**
    ```bash
    python
    >>> import my_package
    >>> print(my_package.say_hello())
    # Should print: Hello, World from my_package!
    >>> exit()
    ```
4.  **Deactivate:**
    ```bash
    deactivate
    ```

**Layman’s Terms:**

Same process as before, but now `pip install .` reads your modern `pyproject.toml` index card to understand how to install the package into the clean test kitchen (`test_env`). It worked, confirming your packaging configuration is good!

### 4.3 Short Break (2:30 – 2:45 PM)

Stretch break! Ready to share (to the test library).

### 4.4 Publishing to Test PyPI with `twine` (2:45 – 3:30 PM)

**Topics:**

*   **Test PyPI vs. Real PyPI:** Use Test PyPI (`test.pypi.org`) for practice.
*   **`twine`:** The secure tool for uploading.
*   **Authentication: API Tokens (Recommended):** Instead of typing your password directly, it's **highly recommended** to use API tokens for both Test PyPI and the real PyPI.
    *   **How to get one:** Go to your account settings on Test PyPI (or PyPI). Find the API tokens section. Create a new token, potentially scoping it to a specific project if desired. **Copy the token immediately** – you won't see it again.
    *   **How to use:** When `twine` prompts for a username, enter `__token__`. When prompted for the password, paste your **entire API token** (including the `pypi-` prefix).

**Hands-On Exercise:**

1.  **Get a Test PyPI API Token:** Go to [Test PyPI Account Settings](https://test.pypi.org/manage/account/) -> API Tokens, create one, and copy it.
2.  **Navigate to your project root** (`my_package_project`).
3.  **Run `twine upload`:**
    ```bash
    twine upload --repository-url https://test.pypi.org/legacy/ dist/*
    ```
4.  **Enter Credentials:**
    *   Username: `__token__`
    *   Password: (Paste your copied Test PyPI API token here)
5.  **Check for Success:** Look for the success message and the URL to your package on Test PyPI.
    ```
    View at:
    https://test.pypi.org/project/my-simple-package/0.1.0/
    ```
6.  **Visit the URL** to see your package page.

**Layman’s Terms:**

Using the secure courier (`twine`), you sent your packages from `dist/` to the practice library (`Test PyPI`). This time, instead of showing your regular library card (username/password), you used a special, super-secure one-time passkey (**API Token**) for authentication (`__token__` / your token value). Your package is now listed in the test library's catalog online.

### 4.5 Installing from Test PyPI (3:30 – 4:15 PM)

**Topics:**

*   **Verify Upload:** Install using `pip` from Test PyPI.
*   **`pip install --index-url`:** Flag to point `pip` to the test repository.

**Hands-On Exercise:**

1.  **Create and activate another fresh virtual environment** (`install_test_env`).
    ```bash
    python -m venv install_test_env
    source install_test_env/bin/activate # Or .\install_test_env\Scripts\activate
    ```
2.  **Install from Test PyPI:** Use the `name` from `pyproject.toml`.
    ```bash
    pip install --index-url https://test.pypi.org/simple/ my-simple-package
    ```
    *(For packages with dependencies not on Test PyPI, you might need: `pip install --index-url https://test.pypi.org/simple/ --extra-index-url https://pypi.org/simple/ my-simple-package`)*
3.  **Verify installation:**
    ```bash
    python
    >>> import my_package
    >>> print(my_package.say_hello())
    # Expected: Hello, World from my_package!
    >>> exit()
    ```
4.  **Deactivate:**
    ```bash
    deactivate
    ```

**Layman’s Terms:**

Asking another friend (`install_test_env`) to visit the practice library (`--index-url`), find your binder by its official name (`my-simple-package`), check it out (`pip install`), and try the recipe (`import my_package`). It worked, proving the end-to-end modern process is successful!

### 4.6 Troubleshooting, Best Practices & Next Steps (4:15 – 5:00 PM)

**Topics:**

*   **Common Pitfalls:**
    *   **Build Errors:** Check `pyproject.toml` syntax (use a TOML validator). Ensure `build-system` requirements are met. Make sure source files (`README`, `LICENSE`, code in `src/`) exist where expected. Check `[tool.setuptools.packages.find]` if using `src` layout.
    *   **Import Errors:** Correct `import` names (`my_package`, not `my-simple-package`). Ensure `__init__.py` exists. If using `src` layout, make sure it's configured correctly in `pyproject.toml` so `setuptools` finds it.
    *   **Upload Errors (403 Forbidden/Bad Request):** Invalid API token? Trying to upload the *same version* again (not allowed)? Make sure `__token__` username is used with the token. Package name already taken on PyPI/Test PyPI?
    *   **Installation Errors:** Check `requires-python` in `pyproject.toml`. Check `dependencies` list for correct syntax and availability on the index. Read `pip` errors!
    *   **README Rendering:** `pyproject.toml`'s `readme` field usually handles content type well. Check Markdown validity if rendering is poor.
*   **Versioning (SemVer):** Still `MAJOR.MINOR.PATCH`. Increment `version` in `pyproject.toml` **before** building a new release. Tools like `bump2version` or features within `hatch`/`poetry` can help automate this.
*   **`pyproject.toml` is Key:** Embrace it as the central configuration point.
*   **Advanced Tools (Beyond 80/20):** For more complex workflows, look into integrated tools like:
    *   [Poetry](https://python-poetry.org/): Manages dependencies, virtual environments, builds, and publishing, all configured via `pyproject.toml`. Uses its own build backend.
    *   [Hatch](https://hatch.pypa.io/): Another excellent, modern tool handling environments, versioning, builds (highly configurable), and publishing, also using `pyproject.toml`. Uses `hatchling` as its default backend.
    These offer more features but have a slightly steeper learning curve than the basic `build`/`twine` combo.
*   **Documentation:** `README` is minimal. Use [Sphinx](https://www.sphinx-doc.org/) or [MkDocs](https://www.mkdocs.org/) and host on [Read the Docs](https://readthedocs.org/) for larger projects.
*   **Testing:** Use `pytest`. Run tests automatically.
*   **Automation (CI/CD):** Use GitHub Actions, GitLab CI, etc., to automate testing, building, and publishing (using API tokens securely stored as secrets).

**Layman’s Terms:**

Quality control and future plans:
*   **Troubleshooting:** Fixing recipes using the modern index card (`pyproject.toml`) as the guide. Check the card's syntax and ingredient lists (`dependencies`). Can't re-submit the exact same edition (`version`).
*   **Versioning:** Keep track of editions clearly in `pyproject.toml`.
*   **Advanced Tools:** Like upgrading from basic kitchen tools to a full professional chef's suite (`Poetry`, `Hatch`) that manages everything smoothly.
*   **Documentation/Testing/Automation:** Writing full cookbooks (Sphinx), rigorous taste-testing (`pytest`), and robotic kitchen assistants (CI/CD).

---

## Chapter 5: Wrap-Up (5:00 – 5:15 PM)

You've navigated the essentials of modern Python packaging!

### Recap: What You Achieved

*   **Understood the Why:** Benefits of packaging.
*   **Created Modern Structure:** Used `src` layout and necessary files.
*   **Configured with `pyproject.toml`:** Defined build requirements and package metadata using the standard file.
*   **Added Essential Files:** `README.md`, `LICENSE`.
*   **Built with `build`:** Generated `sdist` and `wheel` using the standard tool.
*   **Tested Locally:** Verified installation via `pip install .` in a `venv`.
*   **Published (to Test PyPI) with `twine`:** Used API tokens for secure upload.
*   **Installed from Test PyPI:** Confirmed installation via `pip install --index-url`.

You've mastered the core 20% of modern Python packaging!

### Next Steps on Your Packaging Journey

1.  **Publish to Real PyPI:** Get a PyPI account and API token. Run `twine upload dist/*`. Choose a unique package `name` in `pyproject.toml`.
2.  **Refine Package:** Add features, manage `dependencies` in `pyproject.toml`.
3.  **Improve Docs/Testing:** Use Sphinx/MkDocs, write `pytest` tests.
4.  **Explore Build Options:** Look into `[tool.setuptools]` options in `pyproject.toml` for including package data, etc.
5.  **Set Up CI/CD:** Automate workflows with GitHub Actions, etc.
6.  **Consider Advanced Tools:** Evaluate if `Poetry` or `Hatch` fit your workflow needs as projects grow.

### Final Thoughts

Practice is key. You've built the foundation using current best practices. Experiment, consult the Python Packaging User Guide ([packaging.python.org](https://packaging.python.org/)), and don't fear error messages – they are learning opportunities.

---

## Conclusion: Happy Packaging!

You now have the practical skills to package your Python code the modern way, using `pyproject.toml`, `build`, and `twine`. This opens the door to sharing your work effectively within the Python ecosystem. Keep building, keep learning, and share your creations!

---