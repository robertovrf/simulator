# emergent_web_server

Some notes to help you get started:

Make sure you download and install the latest version of Dana at http://projectdana.com.

First thing to do is to set up the database. We usually dedicate the server scc-mc10.lancs.ac.uk to the database. The first thing to do is to enable incoming requests to the database from different IPs other than localhost. For that, please ssh into scc-mc10.lancs.ac.uk and run "redir --lport=3306 --laddr=scc-mc10.lancs.ac.uk --cport=3306 --caddr=localhost" on the command line. Then, go to "repository/danapedia" and compile "DanaPediaDBCreator.dn" with the following command "dnc DanaPediaDBCreator.dn" and execute it using the following command "dana DanaPediaDBCreator.o".

The second step is to define an entry point machine. This machine can be your own laptop, but usually I defined the entry point as being scc-mc1.lancs.ac.uk. The entry point is important, because that is the IP address you should use when accessing the web server. Make sure to change the IP addresses on the ws_client components to point to the defined entry point for the system. Just go to ws_client folder and workload_A, workload_B and workload_C and change every single one of those components to point to entry point IP.

Next, we have to compile the entire project. For that, we use "make.dn" which is located in the root of the project. This script works in both Linux-based (ubuntu, raspberry pi and Mac) and Windows platforms and requires an argument either -w for Windows or -l for Linux-based systems. To compile make.dn type: "dnc make.dn -v".

You will also find a folder named "scripts_compositions". In this folder you will find two subfolders named "common_node" and "entry_point". These folders hold the configuration files that will guide make.dn to properly compile the project and to filter the unwanted components.

To compile for the entry_point node, please type: "dana make -l scripts_compositions/entry_point_node/four_main_compositions/four_compositions.config". If you want to compile for the common_node, i.e. the nodes that are not the entry point but are going to be part of the system, please type: "dana make -l scripts_compositions/common_node/four_main_compositions/four_compositions.config". Note that, if you are running the system locally, please compile it as a common_node.

After compiling the entire project, it is time to run the system. For that, we need to start the InteractiveDistributor component and the Manager component in the entry point node, and for each common node machine execute the ESLauncher component. To run InteractiveDistributor, go to the metacom folder and type: "dana -sp ../repository/ InteractiveDistributor ../repository/TCPNetwork.o scripts_population/10_servers.config"

Note that there is a subfolder in "metacom" named "scripts_population". This subfolder holds some configuration files that let our system know which nodes are participating in the system. Please edit them with the IP addresses of your infrastructure. If you are running the system locally, please run with 1_server.config file instead of the 10_servers.config file used in the example.

Again, the config files in "scripts_population" need to be updated to point to the right machines. Also, remember to update the danapedia.config file (also in metacom folder) to point to your database machine. You don't have to touch the field "memcached_ep: localhost".

To execute the Manager.o component go to the entry point machine, in the metacom folder and execute "dana -sp ../repository/ Manager.o". For each one of the common node machines, go to metacom and execute the ESLauncher component with the command "dana -sp ../repository/ ESLauncher.o".

When the system is running, one good way to make sure everything is working properly is to go to a web browser and type http://"entry_point_address":2012/danapedia. If you get a Danapedia page and are able to navigate through the web site properly, then it means everything is up and running.

After the system is started, execute one client from the ws_client folder. Just compile one of them, for example, the Client5T.dn component located in "ws_clients/workload_A", with the command "dnc Client5T.dn" and execute it with "dana Client5T". In the InteractiveDistributor terminal type help for a list of all possible commands. You can list all possible compositions by typing "get_all_configs", or collect information from the entry point machine by typing "run 5 experiment", this command will iterate through all available software composition and collect data 5 rounds of data from each individual composition and dump the data into a file named experiment_0.data for the first composition, experiment_1.data for the second composition and so forth. 

# simulator
