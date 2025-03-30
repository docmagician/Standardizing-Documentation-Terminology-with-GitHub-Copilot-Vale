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
   This tells Vale where to find custom rules (styles) that define terminology standards.  
   - ``MinAlertLevel = warning``
   This sets the alert level for rule violations. Setting it to warning ensures that Vale flags incorrect terminology but doesn’t prevent documentation updates.  
   - ``[*.md]``
   This specifies which files Vale should check.If you want to check YAML files too.  
   - ``BasedOnStyles = Terminology``
   This tells Vale to apply rules from a style file named Terminology.yml.
   The file must be stored inside .vale/Styles/Terminology.yml
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