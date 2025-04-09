## How to Use the Terminology Check Setup
After setting up GitHub Copilot, Vale, and GitHub Actions, you can start using this system to keep your documentation clear and consistent. Here’s how it fits into your day-to-day workflow:  
### Write Your Docs with GitHub Copilot
GitHub Copilot acts as a smart writing assistant inside VS Code. It suggests phrases and completions based on what you're writing, which helps maintain a consistent writing style and speeds up the process.

To use it:
 - Open a Markdown `(.md)` or YAML `(.yml, .yaml)` file in VS Code.
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
- The GitHub Actions workflow installs Vale and checks your content.   
- If there are no issues, you’ll see a success message.    
- If there are problems, it flags the incorrect terms and shows what needs to be fixed.  

You'll find the results in the Actions tab of your repo, or directly in your pull request under Checks.
### Fix Any Terminology Issues
If Vale reports a problem:
1. Open the affected file.
2. Find the word Vale flagged.
3. Replace it with the preferred term (as defined in your Terminology.yml file).
4. Save, commit, and push the update.    

The workflow will run again and confirm once the issue is resolved.
### Update Your Terminology Rules (Optional)
Keep your Vale rules up to date with the style and terms your team has agreed on. If new preferences come up during team discussions or reviews, just update the Terminology.yml file in .vale/Styles.  
For example, if your team prefers using see instead of refer, or shows instead of displays, add those in the file like this:
```She 
swap:  
  displays: shows  
  refer: see  
  user-friendly: intuitive
  ```
  Save your changes and commit them to the repo. Vale will use the updated rules the next time it runs, whether locally or in GitHub Actions.

## Project Structure Overview
Your repository should look like this after completing the setup:
