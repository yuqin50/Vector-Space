
# Inventor_Files_Dependency
[Demo](https://myshare.autodesk.com/:v:/g/personal/yuqin_shen_autodesk_com/Eeg9nQzeLBpLtbB3u0SmzuEBXbAJvv02vg2kZC5f5DKp0Q?e=ph6jbS)   
This project explores efficient ways to visualize Inventor files dependency. The files include `.iam`, `.idw`,`.ipn`, more features will be considered in the future.
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
Which shows the connection between unique items. The source code refers to [force_directed graph](https://bl.ocks.org/mbostock/4062045/5916d145c8c048a6e3086915a6be464467391c62).
- Tree Graph     
Which shows the tree structure of targets. The source code refers to [tree graph](http://bl.ocks.org/d3noob/8375092).
### Client   
This function shows clients which use current items in order to help users understand influence on stakeholders when they change the design.
## RefResolver
This application takes the full path of our target file as input, and generated `_force.txt`, `_tree.txt`, `_.csv` files. RefResolver parses the information from Inventor and writes in these files which will be used to generate .html files. 



