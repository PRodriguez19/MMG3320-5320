
## Logging in to VACC-OOD:  

1. Use the [VACC-OOD](https://vacc-ondemand.uvm.edu) link to access the site

2. Add your UVM netid and password

3. You should be viewing the following dashboard

    ![dashboard](../img/dashboard.png)

4. To access the Terminal Go to <button>Clusters</button>  and click
`>_VACC Shell Access`

    ![terminal dashboard](../img/terminal_dashboard.png)


## Connecting to VACC with SSH 


1. Type in the `ssh` command at the command prompt followed by a space, and then type **your** username (e.g. uvm net id) plus the address of the cluster `@vacc-user1.uvm.edu`.

    ```bash 
    ssh username@vacc-user1.uvm.edu 
    ```

2. Press the return/enter key and you should receive a prompt to input your password. Type in your password and note that **the cursor will not move as you type** it in! This is normal and know that the computer is receiving and  transmitting your typed password to the remote system.

    ![login](../img/login2.png)


3. If this is the first time connecting to the cluster, **a warning will pop up** and will ask you if you are sure you want to do this; **type `Yes` or `Y`**.


    Once logged in, you should see a new command prompt:

    ![login](../img/login.png)


## Using VACC-OOD OFF-campus

To use OFF-campus you will need to VPN first. See [install-cisco-vpn](https://www.uvm.edu/it/kb/article/install-cisco-vpn/) for more information!
