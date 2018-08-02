**Setting up CNTK on RHEL in openstack/openhouse**

 1. Choose RHEL7 machine
 
 2. install easy_install
 3. install `pip` with `easy_install`

 4. install `openmpi` with `yum`
 5. add the following lines to `~/.bash_profile`
 

    `PATH=$PATH:$HOME/.local/bin:$HOME/bin:/usr/lib64/openmpi/bin
    export LD_LIBRARY_PATH=/usr/lib64/openmpi/lib:$LD_LIBRARY_PATH`

 6. install `cntk` using `pip`
 7. run `"python -c "import cntk; print(cntk.__version__)"`
 
 8. if it complains abt libstdc++ version
 9. create `/etc/yum.repos.d/Fedora-Core23.repo` containing

    `[warning:fedora]
        name=fedora
        mirrorlist=http://mirrors.fedoraproject.org/mirrorlist?repo=fedora-23&arch=$basearch
        enabled=1
        gpgcheck=0`

 
 10. run `yum install gcc --enablerepo=warning:fedora`
