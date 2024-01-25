
***
<h1>SAMBA SERVER CONFIGURATION</h1>

***

Samba is an open-source software suite that facilitates file and print services between different operating systems. It enables interoperability between systems running Windows, macOS, Linux, and other Unix-based operating systems. Samba implements the SMB/CIFS (Server Message Block/Common Internet File System) protocol, which is commonly used for sharing files, printers, and other resources on a network. Samba has the following features

**File Sharing:** Samba allows users to share files and directories between computers running different operating systems, such as Windows, Linux, and macOS.

**Printer Sharing:** It supports printer sharing, enabling printers connected to a Samba server to be accessed by clients on the network.

**Domain Controller:** Samba can act as a domain controller in a Windows-based network environment, providing services such as user authentication and centralized management of users, groups, and permissions.

**Integration with Active Directory:** Samba can integrate with Microsoft Active Directory, allowing Linux and Unix systems to participate in an Active Directory domain.

**Security:** Samba provides various security features, including support for encrypted communication using protocols like SMB encryption and integration with security mechanisms such as Kerberos.


The application will be based on sharig files with the following steps

- Install samba with the command
	-	sudo apt install samba
- Create a directory to share and permissions defined accordingly using the following command
	- sudo mkdir/share
	- sudo chmod 777 /share
**Note:** You can use any directory name.

- Edit the samba configuration file /etc/samba/smb.conf with the following at the last line
	- Name of share 
	- Path to share
	- Public
	- Valid users
	- Users that can read
	- Users that can write
	- Visisbility to remote shares
	- Comment

	**See sample below:**
	[shared-files]
		path = /share
		public = yes
		comment = User's Samba File Server
		browsable = yes
		write list = tix
		readlist = tix
		valid users = user1, user2, user3 ...
	**Note:** tis not limited to the above parameters which include readonly, create mask, directory mask, etc

- Test parameters with the command
	-	sudo testparm
	If the parameters displayed successfully without any errors, then the samba configuration file was successfully edited.

- check if samba is running and enabled using the command: systemctl status smbd and systemctl status nmbd by using the following commmand
	- sudo systemctl status smbd
	- sudo systemctl status nmbd
*if not enabled start and enable the service with the following command*
	- sudo systemctl start smbd
	- sudo systemctl start nmbd
	- sudo systemctl enable smbd
	- sudo systemctk enable nmbd

- Create Samba Users for with no login access using the following command
	*starting with their usernames with*
	-	sudo useradd -s /sbin/nologin user1
	- 	sudo useradd -s /sbin/nologin user2
	*and password with*
	-	sudo smbdpasswd -a user1
	- 	sudo smbdpasswd -a user2
**Note:** you can create as many users as you can

- Finally is to access the shared directory from the Linux server. The advantages of Samba server is that is compactible with different operating systems. 
Ensure your network configurations are well configured such that they can connect with each other. You can ping each network to test network communication
	- From windows client, one method is to type search for "run" and input the values of your ip address. for example \\***.***.***.*** and click ok. This will connect to the Public folder where the files can be shared.
	- for Linux client, one method is to navigate to Files folder from the GUI, and click on other locations tab. Locate the connect to server tab type smb://ip address of linux server machine with a sample like this smb://***.***.***.*** and click on connect. 
	A prompt will be displayed asking if you want to login as a registered user or anonymous user. As a registered user, you will have to input password for the user selected, which have been created earlier. Now connection from Linux server to linux client is successful.