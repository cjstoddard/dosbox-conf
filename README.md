# dosbox-conf
How to turn a perfectly good modern system into a 16 bit retro computer.

The following instructions will walk you through setting up a system so it boots directly into DOSBox-x. These instructions do require some knowledge of Debian Linux.

1. Install Debian 12. Do not install a desktop environment or windows manager, Do install ssh server.

2. Log in as root and change the apt repos to sid, and run;

        apt update && apt dist-upgrade
        
   These updated libraries are neccessary for DosBox-x.

3. Log in as root once again and install the following;

        apt install sudo mc xorg twm lightdm pulseaudio shellinabox vsftpd
        
   Once sudo is installed, add you normal user account into the /etc/sudoers file.
    
4. Install the packages neccessary to compile DosBox-x;

        apt install automake gcc g++ make libncurses-dev nasm libsdl-net1.2-dev libsdl2-net-dev libpcap-dev libslirp-dev fluidsynth libfluidsynth-dev libavformat-dev libavcodec-dev ibavcodec-extra libswscale-dev libfreetype-dev libxkbfile-dev libxrandr-dev
    
5. Get the source code for DosBox-x;

        git clone https://github.com/joncampbell123/dosbox-x.git

6. cd into the dosbox-x folder and run the following command;

        ./build-debug-sdl2

   This will take a while, once it is done, run this command;
   
        make install
    
7. You can now reboot the system, when you come back up, you will get a login screen. Login as your normal user account. The screen will be blank except for the wallpapaer. Click on the desktop and a menu should appear, mouse over Debian > Applications > Shells, click on bash. This will open a commandline for you. first run "xrandr", this will tell you your screen resolution, write this down, you will need it later. Next run "ip addr", this will give you your IP address and the name of you network device, it will be something like enp3s0. Write both of these down as well. Lastly run "mkdir dosbox".

8. Run the following commands;

        mkdir .config/dosbox-x
  
        cp /usr/share/dosbox-x/dosbox-x.reference.full.conf .config/dosbox-x/dosbox-x-2023.03.31.conf
  
   Next run;
   
        nano .config/dosbox-x/dosbox-x-2023.03.31.conf
  
   Change the following lines to match your screen resosultion per xrandr output.
        
        fullresolution    = 1680x1050
        
        windowresolution  = 1680x1050
  
   Change the following line to match your network device per "ip addr" output.
   
        realnic = enp3s0
     
   Finally add the following lines at the bottom of the file.
        
        mount c ~/dosbox
        
        c:
        
        SET BLASTER=A220 I5 D1 H5 P330 T6
  
   Save and close the file
  
9. Now we want to setup auto login, so the system boots straight to Dosbox-x. Edit the following file;
        
        sudo nano /etc/lightdm/lightdm.conf
        
   Uncomment the following two linesand add the name of you normal user next to autologin-user=
   
        autologin-user=chris
        
        autologin-user-timeout=0
   
   Save and exit the file

10. Edit the following file;

        nano .xsession

    add the following line;
        
        exec dosbox-x
    
    save and exit the file

11. Now reboot the system, you should boot straight into DosBox-x

12. If you wish to maintain the Linux portion of this system, you can access the command line using ssh or by pointing a browser at the system IP address like this "https://192.168.0.120:4200". If you wish to transfer files, you can access the system through FTP using Filezilla or similar.

13. You can now install games or applications, and even Windows 3.1. You should have SoundBlaster compatible sound and networking. The networking can be used by configuring it as an NE2000 card in MTCP or using Windows for Workgroups 3.11. You can also mount folders in your home directory as a CDROM or a floppy drive, simply use FTP to transfer the programs you wish to install to folders in your home directory, mount the folder as a CDROM and install away.
