# Setting Up Terminology Standardization with GitHub Copilot & Vale
Let’s set up terminology consistency in your docs with GitHub Copilot, Vale, and GitHub Actions. This way, you can be sure your content sticks to your style guide, saving time on manual checks and keeping everything clear and consistent.
## Install GitHub Copilot
GitHub Copilot is like having a smart writing partner right in your editor. It suggests relevant text based on the context, helping you write faster and more efficiently. We’ll configure it to help enforce terminology rules while writing.
### Enable Copilot in Visual Studio Code(a.k.a VS Code)
1. Open **VS Code**.
2. Accessing Extensions
   - Select the Extensions icon in the Activity Bar on the left side of the **VS Code** window.
   - Alternatively, press `Ctrl+Shift+X` (Windows/Linux) or `Cmd+Shift+X` (Mac) to open the Extensions Marketplace.
3. In the Extensions Marketplace, search for **GitHub Copilot**.  
4. Select **GitHub Copilot** (by GitHub) from the results.
5. Click **Install**.
5. Restart **VS Code** to complete the installation. 
6. Open the **GitHub Copilot** settings by selecting the gear icon ⚙️ in the Extensions list.
7. To sign up for GitHub Copilot:  
     - **Create a GitHub Account**: Visit [GitHub's sign-up page](https://github.com/signup) to register.  
     - **Subscribe to Copilot**: After signing in, go to the [GitHub Copilot subscription page](https://github.com/features/copilot) to choose a plan and subscribe.  
     **Note**: An active GitHub Copilot subscription is required to use the service.
9. Go to **VS Code** **Settings > Extensions > GitHub Copilot**.
10. Enable GitHub Copilot if it's not already enabled.
11.  Enable Copilot for Markdown (.md) and YAML (.yml) files. 

✅ Copilot is now ready to assist with writing consistent documentation!  
 
## Set Up Vale
Vale is a simple tool that helps us keep our documentation consistent by making sure we're using the right terms. We'll set it up to catch any terminology issues as we write.
1. Install Vale
    - Windows (via Chocolatey)
      ```sh
      choco install vale 
    -  Mac (via Homebrew)
        ```sh    
        brew install vale
      -  Linux (via Curl)
           ```sh    
          curl -fsSL https://install.vale.sh | sh
2. Create a Vale Configuration File ``(.vale.ini)``
     - In the root directory of your repository (where your documentation files are stored), create a new file named ``.vale.ini``.  
     Example:
       ```sh 
       📂 my-documentation-repo  
        ├── 📄 .vale.ini  ✅ (This is the file you created)  
        ├── 📂 .vale  
        │    ├── 📂 Styles  
        │         ├── Terminology.yml  
        ├── 📄 README.md  
        ├── 📄 guide.md  
        ├── 📄 install.md  
3. Add content in the below content in the file
   - ``StylesPath = .vale/Styles``
   specifies the directory where Vale looks for custom rules (styles) that define terminology standards.  
   - ``MinAlertLevel = warning``
   sets the alert level for rule violations. Setting it to warning ensures that Vale flags incorrect terminology but doesn’t prevent documentation updates.  
   - ``[*.md]``
   specifies which files Vale should check. If you want to check YAML files as well, you can add them in the same way, like [*.{md,yml,yaml}].
   - ``BasedOnStyles = Terminology``
   tells Vale to apply rules from a style file named Terminology.yml. The file must be stored inside .vale/Styles/Terminology.yml.  
   Example:
      ```sh 
     StylesPath = .vale/Styles  
     MinAlertLevel = warning  

     [*.{md,yml,yaml}]  
     BasedOnStyles = Terminology
    Note: You can use the same file as it is attached in project repository.  
4. Create a Terminology Rules File. Inside .``vale/Styles/,`` create a file called ``Terminology.yml`` and add the following rules:
    ```sh
    extends: substitution  
    message: "Use '%s' instead of '%s'."  
    level: warning  
    ignorecase: true  
    swap:  
    displays: shows  
    refer: see  
  You can use file as it is attached in project and it would be great if you contribute to improve it.

## Automate Terminology Checks in GitHub Actions
GitHub Actions is a workflow automation system built into GitHub. Let’s set it up to run Vale and ensure consistent terminology whenever someone pushes changes or opens a pull request.
1. Create a .github/workflows folder in your repository to store your GitHub Actions workflow files.  
2. Add a new YAML file for the workflow, e.g., terminology-check.yml.
3.  Copy and paste the following workflow into the YAML file.
```sh 
name: Terminology Check

on:
  push:
    branches: [main, develop]
  pull_request:
    branches: [main, develop]

jobs:
  check-terminology:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout code
        uses: actions/checkout@v3

      - name: Install Vale
        run: |
          curl -fsSL https://install.vale.sh | sh
          echo "$HOME/.local/bin" >> $GITHUB_PATH

      - name: Run Vale
        run: vale .

      - name: Output on Success
        if: success()
        run: echo "✅ Terminology check passed."

      - name: Output on Failure
        if: failure()
        run: echo "❌ Issues found. Please fix terminology errors."
  ```
| Term/Section             | What It Means                                                                                                          |
|--------------------------|-------------------------------------------------------------------------------------------------------------------------|
| `name:`                  | The name of the workflow. Appears in the GitHub Actions tab. In this case, it’s called “Terminology Check.”           |
| `on:`                    | Defines when this workflow should run. Here, it runs on push or pull request events targeting `main` and `develop`.   |
| `push:` / `pull_request:`| These are triggers. The workflow runs automatically when code is pushed or a PR is opened for the listed branches.     |
| `jobs:`                  | Groups all the steps needed to perform a task (a job).                                                                 |
| `check-terminology:`     | The name of the job. You can rename this if needed.                                                                    |
| `runs-on:`               | Defines the virtual machine GitHub will use to run the job. `ubuntu-latest` uses the latest Ubuntu Linux environment. |
| `steps:`                 | A list of actions or commands GitHub Actions will execute in order.                                                    |
| `- name:`                | A readable name for the step, shown in the GitHub Actions UI.                                                          |
| `uses:`                  | Refers to a GitHub Action maintained by others. Here, `actions/checkout@v3` checks out the repo code into the runner. |
| `run:`                   | Runs shell commands directly on the runner. This is used to install Vale and run it on the repository files.          |
| `curl -fsSL ...`         | This command downloads and installs the Vale binary using a shell script from Vale’s official site.                    |
| `echo "$HOME/.local/bin" >> $GITHUB_PATH` | Ensures the installed Vale binary is added to the system PATH so it can be used in the next step.       |
| `vale .`                 | Runs Vale on the current directory (.) to check all markdown and documentation files according to your rules.          |
