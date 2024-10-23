# Initial Setup for Windows

1. **Install and setup WSL2 for Windows**  
   [WSL2 Installation Guide](https://docs.microsoft.com/en-us/windows/wsl/install)
   - Use **Ubuntu 20.04**

2. **Install Docker Desktop for Windows**  
   [Docker Desktop Installation Guide](https://docs.docker.com/desktop/windows/install/)

3. **Install Visual Studio Code**

4. **Install Git**

5. **Install Choco**  
   [Chocolatey Installation Guide](https://chocolatey.org/install)

6. **Install JetBrains Nerd Font**

7. **Install Windows Terminal**  
   - Set **Ubuntu** as the default profile.
   - Windows terminal config can be found in the config files folder. Feel free to modify it as needed. You only need to copy the schemes and paste them into your config.
   - Set the default **JetBrains Nerd Font** for the default profile.

8. **Install Starship Prompt**  
   Install via command:  
   `choco install starship` (Run in PowerShell as Administrator)  
   [Starship Configuration Guide](https://starship.rs/config/#lua)
   - Starship config can be found at the following location, feel free to modify as needed. Keep your Starship config synced between Windows, Bash, and Fish terminals.
   - Create directory: `C:\Users\username\.config`
   - Copy the downloaded Starship config file to `C:\Users\username\.config\starship.toml`
   - Run the following to initialize Starship on launch:  
     `Invoke-Expression (&starship init powershell)`
   - You should see that your PowerShell window changes format, which should look noticeably different from the default terminal.

Once the above steps are complete, you should be able to open Windows Terminal, which will automatically start up in Ubuntu.

---

# Initial Setup for Mac

1. **Install Brew**  
   [Brew Installation Guide for Ubuntu](https://www.how2shout.com/linux/how-to-install-brew-ubuntu-20-04-lts-linux/)

2. **Using Brew, install the following:**

   - **Git**  
     ```bash
     brew install git
     ```

   - **AWS CLI**  
     ```bash
     brew install awscli
     ```

   - **jq**  
     ```bash
     brew install jq
     ```

   - **tfenv**  
     ```bash
     brew install tfenv
     ```

   - **python3**  
     (Should be automatically installed with AWS CLI, if not, install it manually)
     ```bash
     brew install python
     ```

   - **kubectl**  
     ```bash
     brew install kubectl
     ```

   - **JetBrains Nerd Font**  
     ```bash
     brew install --cask font-jetbrains-mono-nerd-font
     ```

   - **Starship**  
     Install Starship using:
     ```bash
     brew install starship
     ```
     Create file `~/.config/starship.toml` and copy the following:
     ```toml
     # ~/.config/starship.toml

     add_newline = true
     format = "[$env_var]$all"
     
     [character]
     success_symbol = "✔️ "
     error_symbol = "✖️ "

     [git_branch]
     symbol = " "
     ```

   - **Fish Shell (optional, mainly for Linux)**  
     1. Create a new file `/usr/local/bin/fishlogin`  
        Make it executable with:  
        ```bash
        sudo chmod a+rx /usr/local/bin/fishlogin
        ```
     2. Add the following content to the `fishlogin` file:
        ```bash
        #!/bin/bash -l
        exec -l fish "$@"
        ```
     3. Run the following command to add it to the list of shells:
        ```bash
        echo /usr/local/bin/fishlogin | sudo tee -a /etc/shells
        ```
     4. Set Fish as your default shell:
        ```bash
        chsh -s /usr/local/bin/fishlogin
        ```
     5. Verify that the new terminal starts with Fish.

   - **Add the following to your `~/.profile`**  
     (Replace `<username>` with your username)
     ```bash
     export PATH="/Users/<username>/go/bin:/opt/homebrew/bin:/opt/homebrew/sbin:$PATH"
     eval "$(/opt/homebrew/bin/brew shellenv)"
     
     if [ -f ~/.bashrc ]; then
        . ~/.bashrc
     fi
     ```

   - **Add the following to `~/.bashrc`**  
     ```bash
     eval "$(starship init bash)"
     cd ~/OneDrive/projects/<project>
     ```

   - **Create `~/.config/fish/config.fish`**  
     Paste the following code and save:
     ```bash
     starship init fish | source
     
     function fish_greeting
     end

     function gp
     git pull && git add ./ && git commit -m "$argv" && git push
     end

     alias k="kubectl"
     egrep "^export " ~/.profile | while read e
        set var (echo $e | sed -E "s/^export ([A-Z_]+)=(.*)\$/\1/")
        set value (echo $e | sed -E "s/^export ([A-Z_]+)=(.*)\$/\2/")
        
        set value (echo $value | sed -E "s/^\"(.*)\"\$/\1/")
        
        if test $var = "PATH"
           set value (echo $value | sed -E "s/:/ /g")
           eval set -xg $var $value
           continue
        end
        
        set value (eval echo $value)
        set -xg $var $value
     end
     ```

   - **Visual Studio Code Settings.json**  
     Paste the following settings for VSCode:
     ```json
     {
       "telemetry.telemetryLevel": "off",
       "editor.fontFamily": "Menlo, Monaco, 'Courier New', monospace, JetBrainsMono Nerd Font",
       "terminal.integrated.profiles.osx": {
         "bash": {
           "path": "bash",
           "args": ["-l"],
           "icon": "terminal-bash"
         },
         "fish": {
           "path": "fish",
           "args": ["-l"],
           "icon": "terminal-fish"
         },
         "pwsh": {
           "path": "pwsh",
           "icon": "terminal-powershell"
         }
       },
       "terminal.integrated.defaultProfile.osx": "fishlogin",
       "explorer.confirmDelete": false
     }
     ```

---