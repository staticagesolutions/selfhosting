<h1>Deploy a full LXC CLI development environment for NEVM</h1>  

  <i>Requires Proxmox VE server and comfort with CLI and SSH</i>  
  
<h2>Setup turnkey Linux nodejs container<h2>
  
<h3>Downloading LXC containers</h3>
  
  <h4>Connect to server using SSH or use browsers VNC shell</h4>  
    
  You can also open a browser and navigate to https://server_ip:8006 to access your server 
  
    ssh root@se rver_ip  
 
  <h4>Updte and view available containters</h4>
  
    pveam update  
    pveam available
  
  <h4>Locate and pull Turnkeylinux nodejs template</h4>
  Syntax is pveam download [storage] [LXC]  
  
    pveam download local debian-10-turnkey-nodejs_16.1-1_amd64.tar.gz
  
  <h4>List all downloaded images on storage local</h4>
  
      pveam list local  
  
  <h4>Use pct to create an LXC from the template passing just a unique ID number<h4>
    
  <i>You can also use the Proxmox VE web interface GUI to download, list and delete container templates</i>  

      pct create 999 local:vztmpl/debian-10.0-standard_10.0-1_amd64.tar.gz  
 
    
  
  
      
