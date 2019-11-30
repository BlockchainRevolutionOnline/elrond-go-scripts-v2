[Elrond Node Deploy Scripts V2]

[INTRODUCTION]

The current scripts version aims to bring the validator experience closer to mainnet levels and also implements an optional auto update feature.
Following a few simple steps, you can run your node(s) both on local machine and/or multiple remote machines, relying on SSH and RSYNC.
Each node will run in background as a separate systemd unit.


[REQUIREMENTS]

- Running Ubuntu 18.04 & up / Debian / CentOS 8 / Fedora 30
- Running the script requires sudo priviledges.
- Remote machines should be accesible via SSH using key pairs.

[SCRIPT SETTINGS - MUST BE MODIFIED BEFORE FIRST RUN]

- config/variables.cfg - used to define username, home path, keys location and SSH port.
- config/identity 	   - used to define the path to the SSH key - <MAKE SURE YOU EDIT THIS FILE TO MATCH YOUR SETUP!>
- config/target_ips    - used to define the list of remote machines (IPs or hostnames), each machine in a new line

[KEY MANAGEMENT]

Each machine must have its own key set(s) copied locally. 
For running only one node per machine, the 2 keys (initialBalancesSk.pem and initialNodesSk.pem) should be placed in a zip file named 'node-0.zip', in the path previously specified in variables.cfg file (NODE_KEYS_LOCATION)
For running additional nodes on the same machine, simply create additional zip files incrementing the numeric value (i.e. for second node: 'node-1.zip', for third node: 'node-2.zip', etc..), containing the additional key sets.

File structure example:

	$HOME/VALIDATOR_KEYS/node-0.zip
	$HOME/VALIDATOR_KEYS/node-1.zip
	$HOME/VALIDATOR_KEYS/node-2.zip
	...
	$HOME/VALIDATOR_KEYS/node-x.zip
	

If no key sets are found in the specified location, the script will generate new keys and the node(s) will run as Observer(s).
 
[RUNNING THE SCRIPT]

	[FIRST RUN]
		#installs the node(s) on the local machine
		./script.sh install 
		
		#installs the node(s) on all the machines specified in 'target_ips' file 
		./script.sh install-remote 
		
		Running the script with the 'install' or 'install-remote' parameter will prompt for each machine the following:
			- number of nodes to be ran on the machine
			- validator display name for each node (this will only be asked one time)
			- optionally enable the auto-update feature (default 'No') - see [AUTO UPDATE] section for more details	
			
	[UPGRADE]
		#upgrades the node(s) on the local machine
		./script.sh upgrade
		
		#upgrades the node(s) on all the machines specified in 'target_ips' file
		./script.sh upgrade-remote 
		
	[START]
		#starts the node(s) on the local machine
		./script.sh start
		
		#starts the node(s) on all the machines specified in 'target_ips' file
		./script.sh start-remote 
		
	[STOP]
		#stops the node(s) on the local machine
		./script.sh stop
		
		#stops the node(s) on all the machines specified in 'target_ips' file
		./script.sh stop-remote 
				
	[CLEANUP]
		#Removes all the node(s) files on the local machine
		./script.sh cleanup
		
		#Removes all the node(s) files on all the machines specified in 'target_ips' file
		./script.sh cleanup-remote 

[AUTO UPDATE]

If you choose to enable this function, a cron job will be created. The job searches every 10 minutes for new releases.
You can check the status of the auto-update job in the file $HOME/autoupdate.status

[TERMUI NODE INFO]

This version of scripts will start your nodes as separate systemd services an additional termui binary will be build for you on each machine and placed in your $CUSTOM_HOME folder.
During the install process your nodes will have rest api sockets assigned to them following this pattern:

	elrond-node-0 will use localhost:8080
	elrond-node-1 will use localhost:8081
	elrond-node-2 will use localhost:8082
	...
	elrond-node-x will use localhost:(8080+x)
	
You can check the status of each of your nodes in turn by going to your $CUSTOM_HOME folder and using this command (making sure you select the proper socket for the desired node:

	./termui -address 127.0.0.1:8080
	or
	./termui -address 127.0.0.1:8081
	...

[FINAL THOUGHTS]

	KEEP CALM AND VALIDATE ON ELROND NETWORK!