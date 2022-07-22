<h1>Syscoin V4.3 Linux headless CLI setup</h1>

<i>Recommend Debian 11</i>


<h2>Setting up Syscoin 4.3</h2>


<h3>Install Syscoin from source</h3>

<b>YOU MUST BE ROOT (su) FOR THIS TO WORK, DOUBLE CHECK ALL COMMANDS!!!</b>

<h3>Download and unpack Syscoin Core</h3>

<code>
wget https://github.com/syscoin/syscoin/releases/download/v4.3.0/syscoin-4.3.0-x86_64-linux-gnu.tar.gz 
tar xf syscoin-4.3.0-x86_64-linux-gnu.tar.gz \\
install -m 0755 -o root -g root -t /usr/local/bin syscoin-4.3.0/bin/* \\
rm -r syscoin-4.3.0rc2 \\
mkdir ~/.syscoin'' \\
</code>

<br>

<h3>Create the config file</h3>

<code>
nano ~/.syscoin/syscoin.conf
</code>

<h3>Paste in the following</h3>

<code><br>
testnet=1<br>
[test]<br>
rpcuser=user<br>
rpcpassword=password<br>
listen=1<br>
daemon=1<br>
server=1<br>
assetindex=1<br>
port=18369<br>
rpcport=18370<br>
rpcallowip=127.0.0.1<br>
gethtestnet=1<br>
addnode=54.190.239.153<br> 
addnode=52.40.171.92
</code>

<h4>Press Ctrl + O then enter to save the file and Ctrl + X and enter to exit</h4>

<br>
<br>

<h2>Run Syscoin daemon</h2> 

<code>
syscoind
</code>

<h4>Wait a few minutes for banlist, block index, and geth to load</h4>

<br>

<h3>Check status of blocks</h3>

<code>
syscoin-cli getblockchaininfo
</code>

<h4>You should see something like this</h4>
<code>
{
  "chain": "test",
  "blocks": 547051,
  "headers": 1008033,
  "bestblockhash": "0000000c0fe984bcb7cc35caa97e0eed6f322a77a65249b983f7d977872a8acd",
  "difficulty": 0.01452472661000394,
  "time": 1614913087,
  "mediantime": 1614912654,
  "verificationprogress": 0.4321373241464761,
  "initialblockdownload": true,
  "chainwork": "0000000000000000000000000000000000000000000000000000052c70e058ce",
  "size_on_disk": 288879863,
  "pruned": false,
  "softforks": {
    "bip34": {
      "type": "buried",
      "active": true,
      "height": 1
    },
    "bip66": {
      "type": "buried",
      "active": true,
      "height": 1
    },
    "bip65": {
      "type": "buried",
      "active": true,
      "height": 1
    },
    "csv": {
      "type": "buried",
      "active": true,
      "height": 1
    },
    "segwit": {
      "type": "buried",
      "active": true,
      "height": 0
    },
    "taproot": {
      "type": "bip9",
      "bip9": {
        "status": "active",
        "start_time": -1,
        "timeout": 9223372036854775807,
        "since": 0,
        "min_activation_height": 0
      },
      "height": 0,
      "active": true
    }
  },
  "warnings": ""
}
</code>

<br>

<h3>Create a crontab entry to start syscoind at boot</h4>

<h4>open crontab</h4>

<code>
crontab -e
</code>

<h4>If promopted to choose an editor select option 1 nano</h4>

<h4>Paste this below the commented out lines, automatically starts syscoind on boot</h4>

<code>
@reboot /usr/local/bin/syscoind -daemon >/dev/null 2>&1
</code>

<h3>Press Ctrl + O then enter to save the file and Ctrl + X and enter to exit</h3>

<br>
<br>

<h3>Reboot to make sure the daemon starts at boot</h3>

<code>
shutdown -r now
</code>

<h3>Check daemon is running</h3>

<code>
syscoin-cli getblockchaininfo
<code>

<br>
<br>

<h2>Finished!</h2>
