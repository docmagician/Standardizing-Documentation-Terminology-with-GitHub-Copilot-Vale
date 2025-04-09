## How to Use the Terminology Check Setup
After setting up GitHub Copilot, Vale, and GitHub Actions, you can start using this system to keep your documentation clear and consistent. Here’s how it fits into your day-to-day workflow:  
### Write Your Docs with GitHub Copilot
GitHub Copilot acts as a smart writing assistant inside VS Code. It suggests phrases and completions based on what you're writing, which helps maintain a consistent writing style and speeds up the process.

To use it:
 - Open a Markdown (.md) or YAML (.yml, .yaml) file in VS Code.
- Start typing — Copilot will suggest content automatically.
- Press Tab to accept a suggestion or use Alt + ] / Alt + [ to move between suggestions.  
```sh
Note: Copilot suggests helpful completions, but it doesn’t enforce terminology rules. Vale takes care of that in the next step.
```
### Run Vale Locally Before You Push
Vale checks your writing against the terminology rules you’ve set up. Running it locally helps you catch issues early.  
- Open your terminal and run `vale .` to scan the entire repository.
To check a specific file, run `vale filename.md` (e.g., `vale guide.md`).
- Vale highlights any terms that don’t match your rules.   
Example:
`guide.md:15:12 warning Use 'see' instead of 'refer'. Terminology`
- Update the flagged terms to follow your style guide.
### Push Changes and Let GitHub Actions Do the Rest
Once you push your changes to the main or develop branch, or if you open a pull request, GitHub Actions automatically runs the terminology check.  

What happens:  
The GitHub Actions workflow installs Vale and checks your content.   
If there are no issues, you’ll see a success message.    
If there are problems, it flags the incorrect terms and shows what needs to be fixed.  
You can view the results in the Actions tab of your repository or in the pull request under the Checks section.