
# Inventor_Files_Dependency
## Introduction
This project explores efficient ways to visualize Inventor files dependency. The files include `.iam`, `.idw`,`.ipn`, more features will be considered in the future.   

[Demo](https://myshare.autodesk.com/:v:/g/personal/yuqin_shen_autodesk_com/Eeg9nQzeLBpLtbB3u0SmzuEBXbAJvv02vg2kZC5f5DKp0Q?e=ph6jbS) could be be downloadd by click it or you could download directly from the repository. `Final Intern Showcase` please click read only, and it takes some time to load the data, please be patient.   
   
 More information about this project please reach out to Will Secor will.secor@autodesk.com, `RefResolver` source code please reach out to Yuming Zeng yuming.zeng@autodesk.com.  
### `Notice`  
- local server   
`Examples` folder has `Embeded_data` folder, which contains .html that could be directly open in your web browser because raw data has been embeded as variables, and `parse_local_data` folder, which contains .html parses data from your local files and for security, you could only access the data via your local server. Follow the steps:    
1. Installed a server globally: `npm install http-server -g`   
2. Navigate to your html files directory, run the script`http-server`, Now you can visit `http://localhost:8080/` to view your server.
 For example, `http://localhost:8080/index.html`.    
 More about [http-server](https://www.npmjs.com/package/http-server).     
    
- **With limited time, all code need to be refined, especially for graphs.**    
`force_direct graph` is hard to select nodes as labels are floating above it.      
`tree graph` is difficult to visualize the structure if the nodes label are too long.      

## Get Started
### 1. Inventor environment set up:
Run the script on Command Prompt
```
subst r: c:\Inventor
subst n: r:\application\app
call r:\tools\"build tools"\SetInventorEnv.bat debug64
```
### 2. Start Inventor Built version 
Run the script or  Navigated to  R drive-> Tools -> Build Tools, then click ` Start CMD with Debug 64 bit Inventor Environment `.
```
R:
cd Tools\Build Tools
Start CMD with Debug 64 bit Inventor Environment 
Inventor
```
### 3. Start Visual Studio
Navigated to  R drive-> Tools -> Build Tools, then click ` Start VS160 with Debug 64 bit Inventor Environment with MPC ` .  

in case "inventor" doesn't work
Extract: 
```
M25_31_x64_Dev_GitDebugPDB.zip
M25_31_x64_Dev_GitExe.zip
```
then:
`call r:\tools\"build tools"\SetInventorEnv.bat debug64`
## View Dependency App
Currently we designed two features on the ribbon: **Graph**, **Client**. As a prototype, we basically added a *`View Dependency ID`* under *`How To`*, to make it formal, we will 
design an individual tab for View Dependency in the future.
### View Dependency
The code flow:
```
R: Build->Inventor->13 solution 
Find -> IDR_HOWTO

R:\AppFW\FwUIRes\Resource.h
  #define VIEW_DEPENDENCY 9751

R:\AppFW\FwUIRes\FwUIRes.rc
  IDR_HOWTO MENU
BEGIN
    MENUITEM "&How To...",                  ID_HOWTO
	POPUP "&View Dependency..."
	BEGIN
		MENUITEM "&Graph", ID_VIEW_GRAPH
		MENUITEM "&Client", ID_VIEW_CLIENT
	END
END
```
To get more details, please refer to `VIEW_DEPENDENCY`, `ID_VIEW_GRAPH`, `ID_VIEW_CLIENT`.
### Graph   
We currently investigated potentials of force_directed graph and tree graph. Both graphs are based on d3 library https://d3js.org/.
- Force_directed Graph   
Which shows the connection between unique items. The source code refers to [force_directed graph](https://bl.ocks.org/mbostock/4062045/5916d145c8c048a6e3086915a6be464467391c62)   

- Tree Graph     
Which shows the tree structure of targets. The source code refers to [tree graph](http://bl.ocks.org/d3noob/8375092).
### Client   
This function shows clients which use current items in order to help users understand influence on stakeholders when they change the design. This part needs more work, as I only show am image of the client structure as an example. Options:
1. Refer to Bill of Materials   
2. Refer to contrains reference   
## RefResolver
This application takes the full path of our target file as input, and generated `_force.txt`, `_tree.txt`, `_.csv` files. RefResolver parses the information from Inventor and writes in these files which will be used to generate .html files. 



