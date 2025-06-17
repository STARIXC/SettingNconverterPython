# Setting Up nbconvert for LaTeX PDF Conversion with MiKTeX on Windows

This guide explains how to set up Jupyter `nbconvert` to convert notebooks to PDF using LaTeX with MiKTeX on Windows. It covers Anaconda environments (recommended for PyCharm integration), standard Python virtual environments, and global Python environments, with installation options for the **current user only** using `AppData\Local` paths. It also addresses using an existing environment when the working directory (e.g., containing `Data_Extraction.ipynb`) has been moved to a different location, such as `D:\Projects\NewFolder`.

## What is a User Account?
A **user account** in Windows is a unique profile identified by a username (e.g., the name you see when logging in). It controls where user-specific files and programs are stored, under `C:\Users\<UserAccount>`. The `<UserAccount>` placeholder represents your Windows username, found by running `whoami` in PowerShell (e.g., `computername\username`). Software can be installed:
- **For all users**: In `C:\Program Files`, requiring admin rights, accessible to all accounts.
- **For current user**: In `C:\Users\<UserAccount>\AppData\Local`, no admin rights needed, isolated to your account.

This guide uses current user installations to avoid permission issues and ensure isolation.

## Prerequisites
- Windows 10 or later.
- Administrative access for installing MiKTeX (or user-only MiKTeX installation).
- Jupyter notebook file (e.g., `Data_Extraction.ipynb`) in a working directory (e.g., `C:\Users\<UserAccount>\PyCharmMiscProject` or a new location like `D:\Projects\NewFolder`).
- PyCharm Professional (for Jupyter support and **File > Export to PDF**).

## Step 1: Install MiKTeX
1. **Download MiKTeX**:
   - Visit [miktex.org/download](https://miktex.org/download).
   - Download the latest installer (e.g., MiKTeX 24.1).
2. **Install MiKTeX**:
   - Run the installer, selecting "Install for current user" to place it in `C:\Users\<UserAccount>\AppData\Local\Programs\MiKTeX`.
   - Enable "Install missing packages on-the-fly."
3. **Verify Installation**:
   - Open PowerShell:
     ```powershell
     xelatex --version
     ```
   - Expected output: `MiKTeX-XeTeX 4.10` or similar.
4. **Add MiKTeX to PATH** (if not automatic):
   - Open Environment Variables (search "Environment Variables" in Windows).
   - Add `C:\Users\<UserAccount>\AppData\Local\Programs\MiKTeX\miktex\bin\x64` to user PATH.
   - Restart PowerShell and verify `xelatex --version`.

## Step 2: Set Up Anaconda or Python Environment
### Option A: Anaconda Environment (Recommended)
1. **Install Anaconda**:
   - Download the Anaconda installer from [anaconda.com/download](https://www.anaconda.com/download) (64-bit, Python 3.12 recommended).
   - Run the installer:
     - Select "Install for current user" (installs to `C:\Users\<UserAccount>\AppData\Local\Anaconda3`).
     - Uncheck “Add Anaconda to PATH” (use Anaconda Prompt to avoid conflicts).
     - Check “Register Anaconda as my default Python.”
   - Verify:
     ```powershell
     & "C:\Users\<UserAccount>\AppData\Local\Anaconda3\Scripts\conda.exe" --version
     ```
     - Expected: `conda x.x.x`.
2. **Create an Anaconda Environment**:
   - Open Anaconda Prompt (from Start Menu).
   - Create an environment:
     ```powershell
     conda create -n pycharm_nbconvert python=3.12
     ```
   - Activate it:
     ```powershell
     conda activate pycharm_nbconvert
     ```
3. **Install Jupyter, nbconvert, and notebook**:
   ```powershell
   conda install jupyter nbconvert notebook
   ```
4. **Verify Installation**:
   ```powershell
   python --version
   jupyter --version
   jupyter-nbconvert --version
   notebook --version
   ```
   - Expected: `Python 3.12.x`, version numbers for Jupyter packages.

### Option B: Standard Python Virtual Environment
1. **Install Python**:
   - Download Python 3.12 from [python.org/downloads](https://www.python.org/downloads).
   - Run the installer:
     - Select “Install for current user” (installs to `C:\Users\<UserAccount>\AppData\Local\Programs\Python\Python312`).
     - Check “Add Python to PATH.”
   - Verify:
     ```powershell
     & "C:\Users\<UserAccount>\AppData\Local\Programs\Python\Python312\python.exe" --version
     where.exe python
     ```
     - Expected: `Python 3.12.x`, path includes `C:\Users\<UserAccount>\AppData\Local\Programs\Python\Python312\python.exe`.
2. **Create a Virtual Environment**:
   - Choose a location for the environment (e.g., `C:\Users\<UserAccount>\Environments\myenv`):
     ```powershell
     & "C:\Users\<UserAccount>\AppData\Local\Programs\Python\Python312\python.exe" -m venv C:\Users\<UserAccount>\Environments\myenv
     ```
3. **Activate Virtual Environment**:
   ```powershell
   C:\Users\<UserAccount>\Environments\myenv\Scripts\Activate.ps1
   ```
4. **Install Jupyter and nbconvert**:
   ```powershell
   pip install --upgrade pip
   pip install jupyter nbconvert notebook
   ```
5. **Verify Installation**:
   ```powershell
   jupyter --version
   jupyter-nbconvert --version
   notebook --version
   ```

### Option C: Global Python Environment
1. **Install Jupyter and nbconvert**:
   ```powershell
   & "C:\Users\<UserAccount>\AppData\Local\Programs\Python\Python312\python.exe" -m pip install jupyter nbconvert notebook
   ```
2. **Verify Scripts Directory in PATH**:
   - Check PATH:
     ```powershell
     $Env:Path
     ```
   - Add `C:\Users\<UserAccount>\AppData\Local\Programs\Python\Python312\Scripts` if missing:
     - Open Environment Variables.
     - Edit user PATH.
     - Restart PowerShell.
3. **Handle Multiple Python Versions**:
   - Prioritize 3.12:
     - Reorder PATH to place `C:\Users\<UserAccount>\AppData\Local\Programs\Python\Python312` and `Scripts` first.
     - Uninstall unused versions via `Control Panel > Programs > Programs and Features`.
   - Verify:
     ```powershell
     python --version
     ```

## Step 3: Using an Existing Environment with a Moved Working Directory
If your notebook (e.g., `Data_Extraction.ipynb`) has been moved to a new directory (e.g., `D:\Projects\NewFolder`), you can use an existing Anaconda or virtual environment without recreating it:
1. **Locate the Existing Environment**:
   - **Anaconda**: Environment is at `C:\Users\<UserAccount>\AppData\Local\Anaconda3\envs\pycharm_nbconvert`.
   - **Virtual Environment**: Environment is at `C:\Users\<UserAccount>\Environments\myenv` or `C:\Users\<UserAccount>\PyCharmMiscProject\.venv`.
2. **Activate the Environment**:
   - **Anaconda**:
     - Open Anaconda Prompt:
       ```powershell
       conda activate pycharm_nbconvert
       ```
   - **Virtual Environment**:
     ```powershell
     C:\Users\<UserAccount>\Environments\myenv\Scripts\Activate.ps1
     ```
3. **Test PDF Conversion in Terminal**:
   - Navigate to the new directory:
     ```powershell
     cd D:\Projects\NewFolder
     ```
   - Run `nbconvert` with the notebook’s path:
     ```powershell
     jupyter nbconvert --to pdf Data_Extraction.ipynb
     ```
   - Or specify the full path if not in the current directory:
     ```powershell
     jupyter nbconvert --to pdf D:\Projects\NewFolder\Data_Extraction.ipynb
     ```
   - Output: `Data_Extraction.pdf` in `D:\Projects\NewFolder`.
4. **Verify Environment**:
   - Ensure `jupyter`, `nbconvert`, and `notebook` are installed:
     ```powershell
     jupyter --version
     jupyter-nbconvert --version
     notebook --version
     ```
   - If missing, reinstall (in activated environment):
     ```powershell
     conda install jupyter nbconvert notebook  # For Anaconda
     pip install jupyter nbconvert notebook   # For virtual environment
     ```

## Step 4: Configure PyCharm for File > Export to PDF
1. **Set Up Environment in PyCharm**:
   - Open PyCharm, go to `File > Settings > Project: PyCharmMiscProject > Python Interpreter`.
   - Click gear icon > `Add Interpreter`:
     - **Anaconda**: Select `Conda Environment` > `Existing environment` > `C:\Users\<UserAccount>\AppData\Local\Anaconda3\envs\pycharm_nbconvert\python.exe`.
     - **Virtual Environment**: Select `Existing environment` > `C:\Users\<UserAccount>\Environments\myenv\Scripts\python.exe`.
   - Apply and wait for indexing.
2. **Update Working Directory for Moved Notebook**:
   - If `Data_Extraction.ipynb` is in `D:\Projects\NewFolder`:
     - Open `Data_Extraction.ipynb` in PyCharm.
     - Go to `File > Settings > Project: PyCharmMiscProject > Project Structure`.
     - Add `D:\Projects\NewFolder` as a content root.
     - Or set the working directory in a run configuration (see below).
3. **Verify Packages**:
   - In `Python Interpreter`, click `+`, search for `jupyter`, `nbconvert`, and `notebook`, and ensure they’re installed.
4. **Test Export to PDF**:
   - Open `Data_Extraction.ipynb` in PyCharm.
   - Go to `File > Export to PDF`.
   - Check `D:\Projects\NewFolder` for `Data_Extraction.pdf`.
5. **Optional: Create Run Configuration for Terminal Conversion**:
   - Go to `Run > Edit Configurations` > `+` > `Python`.
   - Set:
     - **Script path**: `C:\Users\<UserAccount>\AppData\Local\Anaconda3\envs\pycharm_nbconvert\Scripts\jupyter.exe` (or equivalent for virtual environment).
     - **Parameters**: `nbconvert --to pdf D:\Projects\NewFolder\Data_Extraction.ipynb`.
     - **Working directory**: `D:\Projects\NewFolder`.
     - **Python interpreter**: Environment’s `python.exe`.
   - Save and run.

## Step 5: Troubleshoot Common Issues
1. **"jupyter is not recognized"**:
   - Ensure the environment’s `Scripts` directory is in PATH (e.g., `C:\Users\<UserAccount>\AppData\Local\Anaconda3\envs\pycharm_nbconvert\Scripts`).
   - Check for `jupyter.exe`:
     ```powershell
     
     dir C:\Users\<UserAccount>\AppData\Local\Anaconda3\envs\pycharm_nbconvert\Scripts\jupyter*
     ```
   - Reinstall:
     ```powershell
     conda activate pycharm_nbconvert
     conda install jupyter nbconvert notebook
     ```
   - Use full path:
     ```powershell
     C:\Users\<UserAccount>\AppData\Local\Anaconda3\envs\pycharm_nbconvert\Scripts\jupyter.exe nbconvert --to pdf D:\Projects\NewFolder\Data_Extraction.ipynb
     ```
2. **PyCharm SDK Invalid**:
   - Delete and recreate the environment (Step 2).
   - In PyCharm, go to `File > Invalidate Caches / Restart`.
   - Update PyCharm to 2025.1.1 or later (`Help > Check for Updates`).
3. **File Not Found (Moved Directory)**:
   - Ensure the notebook path is correct:
     ```powershell
     dir D:\Projects\NewFolder\Data_Extraction.ipynb
     ```
   - Update PyCharm’s content root or run configuration to point to `D:\Projects\NewFolder`.
4. **LaTeX Compilation Errors**:
   - Check `D:\Projects\NewFolder\notebook.log` for errors.
   - Install missing packages via MiKTeX Console.
   - Simplify `Data_Extraction.ipynb` (e.g., remove complex markdown or images).
5. **PDF Not Generated**:
   - Run with verbose output:
     ```powershell
     jupyter nbconvert --to pdf D:\Projects\NewFolder\Data_Extraction.ipynb --no-prompt
     ```
   - Manually compile LaTeX:
     ```powershell
     cd D:\Projects\NewFolder
     xelatex notebook.tex
     ```
   - Convert to HTML as fallback:
     ```powershell
     jupyter nbconvert --to html D:\Projects\NewFolder\Data_Extraction.ipynb
     ```

## Notes
- MiKTeX’s `xelatex` is used by `nbconvert` for PDF conversion.
- Anaconda environments simplify package management and avoid PATH conflicts.
- Existing environments can be reused for notebooks in different directories by activating the environment and specifying the notebook path.
- Current user installations isolate software to `C:\Users\<UserAccount>\AppData\Local`.
- Python 3.12 is recommended for compatibility with PyCharm and Jupyter.

## Example Command (Anaconda)
In Anaconda Prompt:
```powershell
conda activate pycharm_nbconvert
cd D:\Projects\NewFolder
jupyter nbconvert --to pdf Data_Extraction.ipynb
```

## Example Command (Virtual Environment)
In PowerShell:
```powershell
C:\Users\<UserAccount>\Environments\myenv\Scripts\Activate.ps1
cd D:\Projects\NewFolder
jupyter nbconvert --to pdf Data_Extraction.ipynb
```