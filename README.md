# projectzomboid-server-arm
Project Zomboid - Dedicated Server on ARM (Oracle Cloud Ampere VM)

\
Random Notes:
- Ampere VM: 4CPU, 24 GB ram, ~40GB disk
- Ubuntu 22.04
- published ports: 16261/udp 16262/udp
- (broken again) ZRAM: apt install zram-tools libblockdev-kbd2 ; enabled (dpkg-reconfigure zram-tools)
- install FexEmu: https://fex-emu.com/ -> https://github.com/FEX-Emu/FEX?tab=readme-ov-file#quick-start-guide
- Install LinuxGSM: wget -O linuxgsm.sh https://linuxgsm.sh && chmod +x linuxgsm.sh
- perform root part installation of LinuxGSM
- these additional deps might be required:

      curl wget file tar bzip2 gzip unzip bsdmainutils python3 util-linux ca-certificates binutils bc jq tmux netcat lib32gcc-s1-amd64-cross lib32gcc-s1-x32-cross lib32stdc++6-amd64-cross lib32stdc++6-x32-cross libsdl2-2.0-0:i386 steamcmd default-jre rng-tools
- add a dedicated user, e.g. "pzserver"
- FEXRootFSFetcher as the dedicated user to download the rootfs amd64, extract squashfs 
- Use LinuxGSM with the dedicated user to prepare the environment (steamcmd and the steam app) - (some dependency errors due to the arm/amd emulation are fine)
- tweak LinuxGSM configuration if required
- start the server (~/pzserver start"
- visit https://github.com/davidedg/projectzomboid-server-scripts
- enjoy!
