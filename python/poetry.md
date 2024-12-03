- [**1. Initializing a Project**](#1-initializing-a-project)
- [**2. Installing Dependencies**](#2-installing-dependencies)
- [**3. Updating Dependencies**](#3-updating-dependencies)
- [**4. Running Scripts and Tools**](#4-running-scripts-and-tools)
- [**5. Dependency Checking**](#5-dependency-checking)
- [**6. Environment Management**](#6-environment-management)
- [**7. Publishing**](#7-publishing)
- [**8. Lock Files**](#8-lock-files)
- [**9. Exporting Dependencies**](#9-exporting-dependencies)
- [**10. Debugging**](#10-debugging)

Python's Poetry is a powerful tool for dependency management and packaging. Here are some of the most commonly used commands:

---

### **1. Initializing a Project**

- **`poetry init`**  
  Starts an interactive wizard to create a `pyproject.toml` file for your project.

---

### **2. Installing Dependencies**

- **`poetry install`**  
  Installs the dependencies listed in `pyproject.toml`.

- **`poetry add <package>`**  
  Adds a dependency to your project.  
  Example: `poetry add requests`

- **`poetry add <package> --dev`**  
  Adds a development dependency.  
  Example: `poetry add pytest --dev`

---

### **3. Updating Dependencies**

- **`poetry update`**  
  Updates all dependencies to the latest versions compatible with `pyproject.toml`.

- **`poetry update <package>`**  
  Updates a specific package.

---

### **4. Running Scripts and Tools**

- **`poetry run <command>`**  
  Runs a command in the virtual environment.  
  Example: `poetry run python script.py`

---

### **5. Dependency Checking**

- **`poetry check`**  
  Checks the validity of your `pyproject.toml`.

---

### **6. Environment Management**

- **`poetry shell`**  
  Activates the virtual environment.

- **`poetry env list`**  
  Lists all Poetry-managed virtual environments.

- **`poetry env remove <env>`**  
  Removes a specific virtual environment.

---

### **7. Publishing**

- **`poetry publish`**  
  Publishes the package to PyPI or a specified repository.

- **`poetry build`**  
  Builds the package for distribution.

---

### **8. Lock Files**

- **`poetry lock`**  
  Updates the `poetry.lock` file with the latest dependencies.

---

### **9. Exporting Dependencies**

- **`poetry export`**  
  Exports dependencies to `requirements.txt` or another format.  
  Example: `poetry export -f requirements.txt -o requirements.txt`

---

### **10. Debugging**

- **`poetry debug`**  
  Shows detailed information about the Poetry environment.

---

These commands form the backbone of most workflows with Poetry.
