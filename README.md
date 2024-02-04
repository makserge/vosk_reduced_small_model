# Reduced small model for Vosk (Kaldi based) howto

Based on info from https://habr.com/ru/articles/735480/

0. Download stable Debian 12.4 (debian-12.4.0-amd64-netinst.iso) from official repository
     https://www.debian.org/CD/http-ftp/#stable
1. Create VirtualBox machine with 2GB RAM, 2CPU, 20GB storage, set first network adapter to bridge mode with ethernet adapter on host machine
2. Run VM and select "Install" option
3. Select "English" and press "Enter"
4. Select "United States" and press "Enter"
5. Select "American English" and press "Enter"
6. Enter hostname and with "Tab" button select "Continue"
7. Leave domain name empty and with "Tab" button select "Continue"
8. Set root password and with "Tab" button select "Continue"
9. Reenter root password and with "Tab" button select "Continue"
10. Set full name for non-root user and with "Tab" button select "Continue"
11. Set username for non-root user and with "Tab" button select "Continue"
12. Set password for non-root user and with "Tab" button select "Continue"
13. Reenter password and with "Tab" button select "Continue"
14. Set timezone and press "Enter"
15. Set "Guided - use entire disk" and press "Enter"
16. Press "Enter"
17. Press "Enter"
18. Press "Enter"
19. Select "Yes" and press "Enter"
20. Press "Enter"
21. Press "Enter"
22. Press "Enter"
23. Press "Tab" button and select "Continue"
24. Press "Enter"
25. Select only "SSH server" and with "Tab" button select "Continue"
26. Press "Enter"
27. Select "/dev/sda" and press "Enter"
28. Press "Enter"
29. Connect via SSH as regular user

ssh user@host

Accept connection

30. Change user to root

    su -

31. Install dependencies

    apt update
    apt upgrade
    apt install unzip git clang make automake sox gfortran libtool subversion g++ zlib1g-dev sudo -y

32. Add user to sudoers 

    usermod -aG sudo username

33. Install python2.7

    mkdir python2.7
    cd python2.7
    wget http://ftp.debian.org/debian/pool/main/p/python2.7/python2.7_2.7.18-8+deb11u1_amd64.deb
    wget http://ftp.debian.org/debian/pool/main/p/python2.7/python2.7-minimal_2.7.18-8+deb11u1_amd64.deb
    wget http://ftp.debian.org/debian/pool/main/p/python2.7/libpython2.7-stdlib_2.7.18-8+deb11u1_amd64.deb
    wget http://ftp.debian.org/debian/pool/main/o/openssl/libssl1.1_1.1.1w-0+deb11u1_amd64.deb
    wget http://ftp.debian.org/debian/pool/main/libf/libffi/libffi7_3.3-6_amd64.deb
    wget http://ftp.debian.org/debian/pool/main/p/python2.7/libpython2.7-minimal_2.7.18-8+deb11u1_amd64.deb

    dpkg -i libffi7_3.3-6_amd64.deb libssl1.1_1.1.1w-0+deb11u1_amd64.deb libpython2.7-minimal_2.7.18-8+deb11u1_amd64.deb python2.7-minimal_2.7.18-8+deb11u1_amd64.deb libpython2.7-stdlib_2.7.18-8+deb11u1_amd64.deb python2.7_2.7.18-8+deb11u1_amd64.deb  
    
34. Return back to regular user
    exit

35. Get small model

    mkdir new-model && cd new-model
    wget "https://alphacephei.com/vosk/models/vosk-model-small-ru-0.22.zip"
    unzip vosk-model-small-ru-0.22.zip

36. Get Kaldi

    git clone https://github.com/kaldi-asr/kaldi
    cd kaldi
    
37. Install extras

    sudo tools/extras/install_mkl.sh -sp debian intel-mkl-64bit-2020.0-088

38. Check Kaldi dependencies
    
    cd tools
    extras/check_dependencies.sh
    
40. Install tools
        
    make -j 4

41. Install more extras

    extras/install_irstlm.sh
    extras/install_opengrm.sh
    
