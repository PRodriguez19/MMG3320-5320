## Download the Git for Windows installer 

### Introduction
Git Bash is a command-line interface that provides a Unix-like environment on Windows, allowing you to interact with your system and manage Git repositories efficiently. It combines Git, a popular version control system, with Bash, a shell commonly used in Linux. This tool is essential for anyone learning programming, working on collaborative projects, or managing code for research purposes.

### Why Use Git Bash?

**Unix Commands on Windows:** Git Bash provides Unix command-line utilities like ls, cp, and rm, which are standard in many programming environments.

**Essential for Programming and Research:** Whether you’re working on a software project, analyzing data, or managing code in a research lab, Git Bash simplifies file management and collaboration.

### Installation 

#### Step 1: Watch this YouTube Video first: https://youtu.be/yo7Z-BEG62A?si=Gy3YvjBOsBOUlRmL 

<iframe width="560" height="315" src="https://www.youtube.com/embed/yo7Z-BEG62A?si=Gy3YvjBOsBOUlRmL" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

This is also another very nice tutorial that will show you the installation process: https://www.stanleyulili.com/git/how-to-install-git-bash-on-windows 

#### Step 2: Download and run the installer 

Download and run the git installer from here: https://gitforwindows.org/ 

#### Step 3: Run the installer and follow the steps below 
    
    + Click on “Next” four times (two times if you’ve previously installed Git). You don’t need to change anything in the information, location, components, and start menu screens.
        
    + Select **“Use the nano editor by default”** and click on “Next”.

    + Select "Let Git decide" and click on "Next"

    + Select "Git from the command line and also from 3rd party software" and click on "Next"
   
    + Select “Use bundled OpenSSH” and click on “Next”.

    + Select “Use the OpenSSL Library” and click “Next”.

    + Keep “Checkout Windows-style, commit Unix-style line endings” selected and click on “Next”.
        
    + Select “Use Windows’ default console window” and click on “Next”.
        
    + Select “Default (fast-forward on merge)” and click on “Next”.
        
    + Select “None” (Do not use a credential helper) and click on “Next”.
        
    + Select “Enable file system caching” and click on “Next”.

    + Ignore “Configuring experimental options” and click on “Install”.
        
    + Click on “Install”.
        
    + Click on “Finish”.
        
    + If your “HOME” environment variable is not set (or you don’t know what this is):
        + Open command prompt (Open Start Menu, then type cmd and press [Enter])
        + Type the following line into the command prompt window exactly as shown: setx HOME "%USERPROFILE%"
        + Press [Enter], and you should see SUCCESS: Specified value was saved.
        + Quit the command prompt by typing exit and then pressing [Enter]
        
