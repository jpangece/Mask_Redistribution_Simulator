#                                       README

    ###               VG 101 Project Mask Redistribution Simulator ###

*The program simulates redistribution of masks among the cities facing Covid-19 with a certain mask production capacity. The redistribution of masks is performed based on the seriousness of infection in a city compared to the number of masks stocked and geometrical factors (e.g. distance between cities). The simulation ends either when the infected number gets down to 0 or when the number of infection is too large.*

#### General usage note ####

1. The program can only be compiled in windows system. Because it includes some header files that only works in windows system, like <windows.h>
2. The program uses OpenGL to make plotting and animation. So please set up the corresponding OpenGL environment and include <GL/glut.h>.
3. The program has some mouse-triggered events. So we suggest you to read the guide carefully before entering the main simulation animation. The guide can be found in the left hand side of the login interface.

#### Main Feature of the Program ####

​    The program tends to simulate a scientific mask redistribution in Hubei Province, China, during the COVID-19.

​    Note that in our model, Xiantao is deemed as the major mask production city with a higher production capacity (rich in fabric production). Other cities have a lower production capacities. The program will trigger you to set the the mask production capacitiy of each city and tolerable infected ratio right after you go into the program.

​    You can see a message box on the upper left, the map on the left and an information list on the right.

- For each city, The gradation of color indicates the severity of the infected rate.

- Red road indicates the mask transportation from the major city (every 6 hours). Yellow road indicates the mask transportation between some non-major cities (every 2 hours).
- When clicking the cities with mouse, a dialog will be shown for you to inspect the trending of the infected number and mask number over days. 

- A message box will be shown when the simulation ends

  In general, the key points of this program lies in the algorithm of determine how to distribute the mask according to the infection level of each cities, also the algorithm of how the mask number affects a city's infection people number, recovered people number, suspect people number and so on. Detailed algorithm can be found in the report.

#### General Description of Each Code File ####

* **mask_simulation.h:**  Define the main data structure (structure city, structure sys...) and include the function list
* **global_values.cpp:**  initialize the city structure and the key values that will be reused again and again in other code files.
* **input_functions.cpp:**  functions about the login dialog box and the mouse-triggered events.
* **sir_functions.cpp:**  functions containing the key algorithm to distribute the mask and evaluate the infected condition
* **time_initializatoin.cpp:**  initialize the virtual time and some city data (like the complicated outline of each city).
* **openGL_basic_functions.cpp**:  the functions containing the elements of plotting and animation interface
* **openGL_structure_functions.cpp:**  the main function to show the user interface and refresh data over time
* **main.cpp:** The regular main function used in OpenGL

#### contact information

\- Jeongsoo Pang (519370990016)

\- Muzhe Wu (519021910946)

\- Chenhao Zheng (519021911270)



