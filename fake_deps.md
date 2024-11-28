# Fake dependencies for ARM/x86:

During the initial install, this warning message is displayed:

    Warning! Missing dependencies: lib32gcc-s1 lib32stdc++6
    Warning! pzserver does not have sudo access. Manually install dependencies or run ./pzserver install as root.
     Run: 'sudo dpkg --add-architecture i386; sudo apt update; sudo apt install lib32gcc-s1 lib32stdc++6' as root to install missing dependencies.
    Failure! Missing dependencies required to run SteamCMD.

The suggested actions did not work for me.
\
Instead, I created 2 dummy packages to satisfy the dependencies (those libraries will be emulated by FEX anyway):

    sudo apt update
    sudo apt install -y equivs
    
    # Dummy package for lib32gcc-s1
    equivs-control lib32gcc-s1-control
    
    cat <<EOL > lib32gcc-s1-control
    Section: misc
    Priority: optional
    Standards-Version: 3.9.2
    
    Package: lib32gcc-s1
    Version: 1.0
    Maintainer: Your Name <youremail@example.com>
    Architecture: all
    Description: Dummy package for lib32gcc-s1
    EOL
        
    # Dummy package for lib32stdc++6
    equivs-control lib32stdc++6-control
    
    cat <<EOL > lib32stdc++6-control
    Section: misc
    Priority: optional
    Standards-Version: 3.9.2
    
    Package: lib32stdc++6
    Version: 1.0
    Maintainer: Your Name <youremail@example.com>
    Architecture: all
    Description: Dummy package for lib32stdc++6
    EOL


    equivs-build lib32stdc++6-control
    equivs-build lib32gcc-s1-control

Then install them:

    sudo apt install ./lib32stdc++6_1.0_all.deb ./lib32gcc-s1_1.0_all.deb

\
Here's the build output:
  
    equivs-build lib32stdc++6-control
    dpkg-buildpackage: info: source package lib32stdc++6
    dpkg-buildpackage: info: source version 1.0
    dpkg-buildpackage: info: source distribution unstable
    dpkg-buildpackage: info: source changed by Your Name <youremail@example.com>
    dpkg-buildpackage: info: host architecture arm64
     dpkg-source --before-build .
     debian/rules clean
    dh clean
       dh_clean
     debian/rules binary
    dh binary
       dh_update_autotools_config
       dh_autoreconf
       create-stamp debian/debhelper-build-stamp
       dh_prep
       dh_auto_install --destdir=debian/lib32stdc\+\+6/
       dh_install
       dh_installdocs
       dh_installchangelogs
       dh_perl
       dh_link
       dh_strip_nondeterminism
       dh_compress
       dh_fixperms
       dh_missing
       dh_installdeb
       dh_gencontrol
       dh_md5sums
       dh_builddeb
    dpkg-deb: building package 'lib32stdc++6' in '../lib32stdc++6_1.0_all.deb'.
     dpkg-genbuildinfo --build=binary -O../lib32stdc++6_1.0_arm64.buildinfo
     dpkg-genchanges --build=binary -O../lib32stdc++6_1.0_arm64.changes
    dpkg-genchanges: info: binary-only upload (no source code included)
     dpkg-source --after-build .
    dpkg-buildpackage: info: binary-only upload (no source included)
    
    The package has been created.
    Attention, the package has been created in the current directory,
    not in ".." as indicated by the message above!

    root@oci:~# sudo apt install ./lib32stdc++6_1.0_all.deb ./lib32gcc-s1_1.0_all.deb
    Reading package lists... Done
    Building dependency tree... Done
    Reading state information... Done
    Note, selecting 'lib32stdc++6' instead of './lib32stdc++6_1.0_all.deb'
    Note, selecting 'lib32gcc-s1' instead of './lib32gcc-s1_1.0_all.deb'
    The following NEW packages will be installed:
      lib32gcc-s1 lib32stdc++6
    0 upgraded, 2 newly installed, 0 to remove and 0 not upgraded.
    Need to get 0 B/3922 B of archives.
    After this operation, 18.4 kB of additional disk space will be used.
    Get:1 /root/lib32gcc-s1_1.0_all.deb lib32gcc-s1 all 1.0 [1964 B]
    Get:2 /root/lib32stdc++6_1.0_all.deb lib32stdc++6 all 1.0 [1958 B]
    Selecting previously unselected package lib32gcc-s1.
    (Reading database ... 160140 files and directories currently installed.)
    Preparing to unpack /root/lib32gcc-s1_1.0_all.deb ...
    Unpacking lib32gcc-s1 (1.0) ...
    Selecting previously unselected package lib32stdc++6.
    Preparing to unpack /root/lib32stdc++6_1.0_all.deb ...
    Unpacking lib32stdc++6 (1.0) ...
    Setting up lib32gcc-s1 (1.0) ...
    Setting up lib32stdc++6 (1.0) ...
