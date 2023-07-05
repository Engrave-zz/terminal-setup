Initial setup for windows:
    Install and setup WSL2 for windows (https://docs.microsoft.com/en-us/windows/wsl/install)
        -  Ubuntu 20.04
    Install docker desktop for windows (https://docs.docker.com/desktop/windows/install/)
    Install Visual Studio Code
    Install Git
    Install Choco (https://chocolatey.org/install)
    Install Jetbrains nerd font
    Install Windows Terminal (select ubuntu as the defualt)
        - windows terminal config can be found in the config files folder, feel free to modify as you see fit. You only need to copy the schemes and paste it to your
          config.
        - Set default jetbrains nerd font to the defualt profile
    Install Starship via command choco install starship in powershell as administrator (https://starship.rs/config/#lua)
        - starship config can be found at the following location, feel free to modify as you see fit. You'll need to make sure that you keep
          your starship config synced between windows, bash and fish terminals.
        - create directory C:\Users\username\.config
        - copy downloaded starship config file to C:\Users\username\.config\starship.toml
        - run Invoke-Expression (&starship init powershell) to make starfish startup on launch
        - you should see that ur powershell window changes format, should look alot different than the default terminal.

Once the above is done you should be able to open windows terminal which will automatically startup in ubuntu.

Initial setup for Ubuntu:
    Install Brew (https://www.how2shout.com/linux/how-to-install-brew-ubuntu-20-04-lts-linux/)
    Using brew install the following:
        - git
        - aws cli
        - jq
        - tfenv
        - python3 (should automatically be installed with awscli if not then install)
        - kubectl
        - starship (curl -sS https://starship.rs/install.sh | sh) follow same instructions above for setting up starship, you need to create a file ~/.config/starship.toml and copy the same config.
        - fish
        - create file /usr/local/bin/fishlogin
            make it executable sudo chmod a+rx /usr/local/bin/fishlogin
            add this to fishlogin file:
            #!/bin/bash -l
            exec -l fish "$@"

            run the following command:
            echo /usr/local/bin/fishlogin | sudo tee -a /etc/shells
            chsh -s /usr/local/bin/fishlogin

            verify that new terminal starts with fish
        - create fish_import_bash_aliases.fish under ~/.config/fish/functions
        paste in the following code and save:
            function fish_import_bash_aliases \
            --description 'import bash aliases to .fish function files.'
            for a in (cat ~/.bash_aliases  | grep "^alias")
                set aname (echo $a | sed 's/alias \(.*\)=\(\'\|\"\).*/\1/')
                set command (echo $a | sed 's/alias \(.*\)=\(\'\|\"\)\(.*\)\2/\3/')
                if test -f ~/.config/fish/functions/$aname.fish
                    echo "Overwriting alias $aname as $command"
                else
                    echo "Creating alias $aname as $command"
                end
                alias $aname $command
                funcsave $aname
                end
            end

            within the fish terminal run fish_import_bash_aliases to import any bash aliases you add to bash.

that should be it :D please update if this doesnt cover everything.