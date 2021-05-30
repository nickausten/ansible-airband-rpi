# Ansible setup files to install rtl_airband on Raspberry Pi
This project provides simple [Ansible](https://www.ansible.com/) configuration files needed to build, install and run [rtl_airband](https://github.com/szpajder/RTLSDR-Airband) on a [Raspberry Pi (RPI)](http://www.raspberrypi.org) micro controller. 

<mark>Many thanks to Tomasz Lemiech, along with previous project authors, and contributors for ***rtl_airband***!</mark>

The installation process:
1. Updates and upgrades Raspberry Pi OS
2. Installs all required tools and libraries
3. Downloads, builds and installs *rtl_airband*
4. Installs customised example *rtl_airband.conf* file
5. Installs, enables and configures services to run on boot for:
   * *pulseaudio*
   * *rtl_airband*
6. Contains a sample *rtl_airband* configuration 

# Procedure
1. Customise config files
   1. Set IP address of RPI(s) on which to install (file: `inventory`)
   2. If needed, update `files/rtl_airband.conf` (see [rtl_airband wiki](https://github.com/szpajder/RTLSDR-Airband/wiki)). 
      1. Set frequencies for your local airport
      2. Optional: Configure Optional: Configure icecast (if using it)
      3. Optional: Configure pusleaudio 'streams', devices, mixers, levels, etc.
      4.  if needed
      5. Example config in repo: 
         1. Assumes using typical RTLSDR device
         2. Configured for Scanning Mode (scans list of frequencies)
         3. Contains example set of frequencies for Perth International Airport (YPPH), Western Australia
         4. Has output configs for:
            1. basic pulseaudio config (assuming default audio device available, configured, operational - via *pulseaudio*) 
            2. basic icecast2 config (commented out) [Note: install and configure an Icecast2 server and then update this config file with Icecast server IP, login and password]
               * Typically on a linux machine you can simple 
                    ```
                    sudo apt install icecast2
                    ``` 
               * Then configure passwords for source, relay (if used) and administrator that match those in this *rtl_airband.conf* file. 
2. Install OS on RPI
   1. See [Standard Installation](https://www.raspberrypi.org/documentation/installation/)
   2. Enable ssh and networking (see [Setting up a Raspberry Pi headless](https://www.raspberrypi.org/documentation/configuration/wireless/headless.md))
3. Establish ssh connection to RPI
   1. Determine IP address (usually by logging into your home route/DHCP-server)
   2. If required, generate ssh-key (on local machine):
        ```
        ssh-keygen
        ```
      Answer all prompts with <CR> to use defaults. 
   3. Copy ssh-key from ansible-computer to RPI
        ```
        ssh-copy-id -i ~/.ssh/id_rsa.pub pi@192.168.1.100
        ```
   4. Try ssh-ing into RPI - after the first time, should no longer require password.
   5. When logged into RPI, try to access internet
        ```
        ping google.com
        ``` 
4. Execute the Ansible 'playbook':
    ```
    ansible-playbook -i inventory airband.yml
    ```
5. Wait until finished - will take minutes and more - depending on internet speed
6. Check:
   1. If RPI has audio output device, plug in earbuds, external speaker/amplifier
   2. If you configured rtl_airband.conf to use Icecast, check your icecast server. For the configured example browse:
        ```
        http://192.162.1.3:8000/atc.mp3
        ```
