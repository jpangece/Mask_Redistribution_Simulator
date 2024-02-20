# Mask_Redistribution_Simulator

## Summary
Simulation of mask redistribution among the cities facing COVID-19 with a certain mask production capacity. The redistribution of masks is performed based on the seriousness of the infection in each city compared with the current number of masks as well as other geometrical factors (e.g. the distance between cities). The simulation ends either when the
infected number gets down to 0 or when the ratio of infection is too large in a certain city (reaches the threshold ratio of infection as an input).

## Design

### Features of the software 
When starting the program, 
the user input interface will be exposed (Figure 1)
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

With time, the components on this interface changes as the simulation progresses. 
Users can also click the "i" button to access the guide. 
Additionally, user can click the city on the map to view the trend of infection in specific city.

#### Models for the Simulation : **Revised SIR Model**
SIR Model is used for the mask simulation interacting with infection numbers.
(https://scipython.com/book/chapter-8-scipy/additional-examples/the-sir-epidemic-model/)

3 Groups of people are divided for pademic in SIR model.
  - People susceptible but not yet infected, the number of whom is denoted as S(_t_)
  - Infectious/infected individuals, the number of whom is denoted as I(_t_)
  - Individuals who have recovered from the disease and now have immunity, the number of
    whom is denoted as R(_t_)
    
with the differential equation describing how they change:

<p align="center">
  <img width="130" height="130" src="https://github.com/jpangece/Mask_Redistribution_Simulator/assets/122253772/961f44b8-6a1f-4df0-b597-083deb70c372">
</p>

SIR model is setted for every city in Hubei province and the cahnge of S, I, R can be achieved with iteration as long as _dt_ is small enough and the initial conditions are provided
  - (http://www.mnw.cn/news/shehui/2245524.html)

The important thing need to be addressed is how to describe parameters and , which should
consider the influence of masks and other determinants in reality:

  - The effective contact rate **β** :
    
    Inspired by the in-class question in Matlab part - "the possibility of a point falling into a circle
    in a region", we treat each person as a point who randomly falls in the city, either in
    "infectious/contacting area" or in "safe area".

    Initially, there were no person in a city (just an assumption) and all the area will be the safe ("infectious area"
    is zero). Then we can put each person into the city. starting from from the first person, the infectious area grows
    with an increment of a 3m-radius circle (the potential risk each person brings).

    For each person, the possibility that he/she might get infected can be approximated as the infectious area before
    he/she is put into consideration divided by the total area. By adding the possibility of all people and divide it
    with the number of people, we can get the average contact rate. Of course the masks will reduce the effective
    contact rate.

    We chose to directly subtract the number of people by the mask number and calculate the remaining people. Moreover,
    since in reality, the only group of people that might get infected is the susceptible (infected and recovered can be
    infected again), the people we discussed before are directly the susceptiblle

    Overall, parameter γ can be expressed as:
    ![5](https://github.com/jpangece/Mask_Redistribution_Simulator/assets/122253772/de1e9a35-3768-48f3-b91a-2e32fa3810fc)


    (k is parameter of adjustment, which is canceled with _sus_num_ in the denominator)
    * it need to be discussed whether (_susceptible_num - mask_num_) < _total_area_ in detailed calculation
   
   - The mean recovery rate **γ**

     The average cure period for COVID-19 is 15 days (http://ask.39.net/question/66075613.html),
     so γ should be around $\frac{1}{15}$
     
     However, when there are too many patients, the lack of medical resources can result in a longer average cure
     period. Therefore, taking into account medical resources (using the number of "sanjia" hospitals in each city as
     the standard, see https://tieba.baidu.com/p/6468395697), we introduce the following parameter:

![image](https://github.com/jpangece/Mask_Redistribution_Simulator/assets/122253772/5facf03c-1e81-495f-bbff-7c446fa30388)

![image](https://github.com/jpangece/Mask_Redistribution_Simulator/assets/122253772/4f8ac8ef-ba98-452f-a57a-870d5d020448)

Moreover, at the beginning of the COVID-19, the effectiveness of medical treatment are low
due to the outburst of pandemic and the panic, so γ can be described in this case as:

![image](https://github.com/jpangece/Mask_Redistribution_Simulator/assets/122253772/b979d8ed-c3a4-47c3-b09d-5a1ab39596da)

We also need to change the mask number in each cities. The number of masks is changed from
three perspectives: the daily consumption of citizens (decrease) , self production (increase),
transportation between cities (increase for non-major cities, decrease for major cities).

- For the daily consumption, we calculate the required mask number for a day:
  _req_msk_num = mask_use_rate * (1 * susceptible_num + 2 * infected_num + 0.5 * recovered_num)_
  ($maskuserate \in (0,1]$, describing the ratio of people going outdoors which decrease with the time (due to the
  increase of awareness of COVID-19 for citizens). In each iteration, we subtract the required mask number of every
  _dt_ from the current mask number.

- For the daily consumption, likewise, we just add the mask production capacity (see the
  "feature part") times _dt_ to the current mask number.

- There are two type of transportation, i.e. transportation between
  major industrial city and each non-major city and transportation within two small regions. For the first kind, we
  first find the order of urgency (the greater $\frac{dI}{dt}$ , the more urgent) of all cities (including the major).
  Masks will be transported following this order. For cities whose orders are before the major city's (more urgent),
  masks will be transported there as long as the remaining mask number of major city is greater than 0, the number of
  transported masks as large as possible; for cities whose orders are after the major city's, the transportation will
  happen if there will be masks more than the required number (calculated previously) in the major city after the
  transportation, the number of transported masks as much as possible.

With all these differential equations, the description of parameters, as well as the change of mask numbers for each city, we can simulate the general progression of COVID-19 infection in Hubei Province through numerous iterations (there is an end_check).











    
    

