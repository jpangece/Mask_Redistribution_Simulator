# Mask_Redistribution_Simulator

## Summary
Simulation of mask redistribution among the cities facing COVID-19 with a certain mask production capacity. The redistribution of masks is performed based on the seriousness of the infection in each city compared with the current number of masks as well as other geometrical factors (e.g. the distance between cities). The simulation ends either when the
infected number gets down to 0 or when the ratio of infection is too large in a certain city (reaches the threshold ratio of infection as an input).

## Design

### Features of the software 
When staring the program, the user input interface will be exposed (Figure 1)
![1](https://github.com/jpangece/Mask_Redistribution_Simulator/assets/122253772/a45c6d50-c6e3-4c91-ac42-40e40185ef79)
(Figure 1)

The users will be asked to input mask production capacities for two types of cities (Major industrial city, i.e., Xiantao, and other cities in Hubei Province) and the tolerable infection ratio. 

The users can click on the corresponding blocks and type in their choices. 
After giving their input, the users can proceed to the next interface by clicking the **"OK"** button. 
If the users encounter any problems, they can click the **"i"** (information) button to find some guidance.

![2](https://github.com/jpangece/Mask_Redistribution_Simulator/assets/122253772/b39b8831-2f0c-426c-b780-9de0a487db52)
(Figure 2)

#### 3 Fields of Interface

1. **Message Box**
  - Located on the upper left
  - When the simulation ends, it will print out the ending message as well as the reason for ending
    (whether a city exceeds the threshold or there are no infection cases in Hubei).
  
2. **Hubei Map**
  - The map is scaled based on reality (size, shape), with different colors indicating the seriousness of infection in       each city (darker colors represent higher infection rates).

  - There are two types of roads among these cities.
  1) The **red roads** all originate from Xiantao City (major production city) and are used, with animation, when there       are transportations of masks (massive amounts) from Xiantao to respective cities every 6 hours.
  
  2) The **yellow roads** cover two regions in Hubei Province and are used, with animation, when there are                    transportations of masks (small amounts) within these regions every 2 hours.
     The number of transportations (exports or imports) is shown alongside the roads.
  
    - Additionally, the time and Province name are printed in the bottom side.

3. **Data List**
  - This list shows 4 significant values (total number of citizens, infection number, recovered number, mask number) for     all the cities in Hubei Province.

With time, the components on this interface changes as the simulation progresses. Users can also click the "i" button to access the guide. Additionally, they can click on a city on the map to view the trend of infection in that specific city.














