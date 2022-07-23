<h1>Linux Syscoin-core masternode 4.4.0rc1 through SSH and CLI</h1>  

This build was done on Debian 11 using a VM on Proxmox hypervisor  
Recommended specs are at least a 2 core CPU and 8GB ram or 4GB+4GB swap file  
100GB or greater SSD for optimal results  
  
<b>MAKE SURE YOU ARE ROOT (su) FOR ALL COMMANDS</b>   

You can copy and paste each line

<h2>Connect to server with SSH or VNC</h2>  

<h3>Download Syscoin-core and extract then install binaries to your path</h3>  

    wget https://github.com/syscoin/syscoin/releases/download/v4.4.0rc1/syscoin-4.4.0rc1-x86_64-linux-gnu.tar.gz  
    
    tar xf syscoin-4.4.0rc1-x86-64-linux-gnu.tar.gz  
    
    install -m 0755 -o root -g root -t /usr/local/bin syscoin-4.4.0rc1/bin/*
    

<h3>Open ports and install dependencies</h3>  
  
    apt install ufw python virtualenv git unzip pv -y  
    ufw allow ssh/tcp   
    ufw limit ssh/tcp   
    ufw allow 18369/tcp   
    ufw allow 30303/tcp   
    ufw logging on   
    ufw enable      
    
<h3>Create syscoin.conf and open in nano</h3>  

    mkdir .syscoin  
    
    nano ~/.syscoin/syscoin.conf  
    
<h3>Copy paste this config in to nano</h3>

    testnet=1 
    [test]  
    rpcuser=user  
    rpcpassword=password  
    listen=1    
    daemon=1    
    server=1    
    assetindex=1    
    port=18369    
    rpcport=18370   
    rpcallowip=127.0.0.1    
    addnode=54.190.239.153    
    addnode=52.40.171.92    
    
Ctrl + o to save then ctrl + x to go back to CLI  

<h3>Run Syscoin-core</h3>

    syscoind
    
<h3>Check blocks status</h3>

    syscoin-cli getblockchaininfo

<h2>Install Sentinel</h2>

    apt update  
    apt -y install git python3 virtualenv   
    git clone https://github.com/syscoin/sentinel.git  
    cd sentinel  
    git checkout dev-4.x  
    virtualenv -p $(which python3) ./venv  
    ./venv/bin/pip install -r requirements.txt  
    
<h3>Edit sentinel configuration file</h3>    
    
    nano sentinel.conf    
    
Uncomment the line that begins syscoin_conf= and change it to /root/ instead of /home/YOURUSERNAME/  

Also comment out mainnet and remove # from testnet. This is how it should look:                               

    # specify path to syscoin.conf or leave blank  
    # default is the same as Syscoin  
    syscoin_conf=/root/.syscoin/syscoin.conf  

    # valid options are mainnet, testnet (default=mainnet)    
    #network=mainnet  
    network=testnet  

    # database connection details  
    db_name=database/sentinel.db  
    db_driver=sqlite  
    
<h3>Finishing Sentinel configuration</h3>  

You need to set your EDITOR= env variable to nano first  

    export EDITOR="/usr/bin/nano"  

Open crontab next but set nano as default editor first  

    crontab -e
    
Set a cron job to wake the system every 5 minutes, this goes after the last line
    
    * * * * * cd /root/sentinel && ./venv/bin/python bin/sentinel.py 2>&1 >> sentinel-cron.log

Then add this line that will start the Syscoin-core daemon at reboot  
    
    @reboot /usr/local/bin/syscoind -daemon >/dev/null 2>&1
    
Exit and prepare to intialize the Masternode

<h2>Register the node in part 2</h2>


