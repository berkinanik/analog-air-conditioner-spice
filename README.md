# analog-air-conditioner-spice
Analog Air Conditioner Design using LTSpice

Following project report is generated using [**Vertopal**](https://www.vertopal.com/) _PDF to MD Converter_.

# Project Report

### MIDDLE EAST TECHNICAL UNIVERSITY

### EE313 -- Analog Electronics Laboratory 2021-2022 Term Project

## Micro Air Conditioner Design Project

> ### Berkin Anık
>
> #### Department of Electric-Electronics Engineering, METU

**_Abstract_---** Air conditioning is a technique that can change the temperature of the air by cooling and heating it. Air conditioner systems,which have become increasingly popular in recent years, are systems that can perform these operations. A more developed system would have ability to perform these operations automatically using a feedback system to ensure that system is always trying to set ambient temperature towards the set temperature or staying in idle if set temperature is reached with a given margin on temperature. This project is about a micro analog air conditioner system design with automatic functionality given a target temperature, and this report includes the design of the air conditioner system as well as various simulations which are run using different numerical inputs.

**_Keywords--- air conditioner, analog, feedback, design._**

# I.INTRODUCTION

The goal of this project is to design a micro air conditioning system.
This system will be composed of four main subsystems which can be seen
in Fig. 1: a sensing unit, a display unit, a control unit, and an
operation unit. Also, there will be a temperature adjustment unit (set
unit) to obtain target temperature from users. The temperature
adjustment unit will send the intended temperature to the control unit
and display unit, which is a voltage value determined using a system
uses a potentiometer, and the sensing unit will provide the ambient
temperature to both control unit and the display unit, which is also a
voltage value. The control unit will then, compare these to voltage
values and drive the operation unit. The control unit\'s main goal is to
turn on the cooling system if the ambient temperature is higher than the
desired temperature when the difference is larger than 1 degree Celsius
the heating function will be activated. If the temperature difference
between the targeted and ambient temperatures is less than 1 degree
Celsius, the system will enter idle state, which means neither cooling
nor heating will operate.

![](/media/image1.png)

> Fig. 1. Micro Air Conditioner System Diagram

In the following parts of this report, firstly requirements of the
desired system will be investigated, after that the design approach used
in each unit, their designs and simulation results obtained using a
SPICE software, namely LTSpice, will be investigated.

# II.DESIGN REQUIREMENTS

### _A.Control Unit_

The control unit must be able drive the operation unit until the desired
temperature is reached. The ambient temperature and set value obtained
from adjustment unit must always be checked and if they deviate 1 degree
from each other, necessary operation unit must be driven to reach target
temperature.

### _B.Operation Unit: Heater and Cooler_

The operation unit must be consisting of a cooler and a heater. Heating
element will be an electrical resistor. Cooling element will be an
electric fan. Their driver circuits must be implementing according to
used circuit elements and devices while constructing them.

### _C.Sensing Unit_

The sensing unit must sense the ambient temperature. Then the sensor
must inform the display and control units about increase or decrease in
temperature by a variable parameter level.

### _D.Display Unit_

The display unit must contain a RGB led and a switch to show the set or
the ambient temperature level whenever they are needed. The display unit
must cover the visible spectrum continuously. Required color scale and
temperature ranges can be seen in Fig. 2.

#### _E.Temperature Adjustment Unit and Design Specifications_

- The temperature adjustment unit (set unit) must be able to allow users to set a temperature between 24 and 40 degrees

- Celsius and fed this target temperature to both display and controls
  units by a variable parameter level.

- The system must be testable and designed using a modular approach.
  Temperature range will be 24-40 degrees Celsius. RGB led indicator must
  cover all visible spectrum regarding this temperature range. Set value
  must be adjusted by a potentiometer, and system must be autonomous when
  there is a 1-degree Celsius difference between ambient and target
  temperature. The heater and cooler operations must not be working
  simultaneously.

![](/media/image2.png)

> Fig. 2. RGB Led Color Scale and Temperature Range

### _F.Design Approach_

A system which would meet these requirements will be designed in
following manner. A determined voltage range will be used the represent
the ambient and target temperature. Determining this temperature range,
temperature sensor will be used in sensing unit and its output will be
the deciding factor. After these sensed ambient temperature and target
temperature are obtained as voltage levels, they will be normalized to
this determined voltage range. 0 degree Celsius is represent with 0
potential difference between these to voltages, positive and negative
values will be considered as the resultant to drive the necessary
operation unit. Heater will run with positive voltage outputs and cooler
will run with negative voltage outputs of the control unit. The
normalized voltage outputs from sensing and adjustment unit will be fed
to the display unit and a RGB Led driver circuit will be designed to
meet the required color scale depending on these voltage values.

## III.SENSING UNIT

The sensing unit is the subsystem which is responsible from the sensing
of ambient temperature and providing this information to control and
display units. LM35 Temperature Sensor is considered as the sensor will
be used in the system which will be constructed. Regarding to its
datasheet, this temperature sensor can work in a temperature range of
-40 and 110 degrees Celsius and, will provide a voltage output with a
10mV/Celsius voltage step within this range \[1\]. It outputs 0 V when
the sensed environment is 0 degree Celsius and 240mV when the ambient
temperature is 24 degrees Celsius.

Considering these specifications, voltage will be fed into the control
unit is determined to be in the range of 240-400mV for 24-40 degrees
Celsius while every 10mV is representing 1 degree Celsius from the
sensing unit. This determined voltage range will also be the key factor
of designing the control, display, and temperature adjustment units.

## IV.CONTROL UNIT

The control unit is the subsystem which is responsible of comparing the
sensed ambient temperature with the target temperature set by the user
and driving the necessary operation unit depending on this comparison.
As mentioned above, a decision unit to output positive or negative
voltage depending on the necessary operation to reach target
temperature, will be designed.

Normalized voltage values from both the sensing unit and the adjustment
unit will be fed into a difference amplifier circuit. The system will
subtract the voltage value of ambient temperature from the target
temperature value coming from the adjustment unit. Every 10mV voltage
difference between these two voltage values is considered as 1-degree
Celsius temperature difference between ambient and target temperature.

Design specifications require this system to be operating if there is
temperature difference greater than 1 degree Celsius, otherwise none of
the operation units must be working. Difference amplifier is designed to
amplify the signal almost 70 times to obtain voltage output around 700mV
if there is a 10mV voltage difference between voltage values coming from
sensing and adjustment units. Using diodes which would have 0.7V
threshold voltage levels, would provide the required idle state when
there is a voltage difference lower than 10mV which means a temperature
difference less than 1 degree Celsius. A diode model is used to provide
this characteristic is used in SPICE designs. The control unit is
designed in this manner and can be seen in Fig. 3.

![](/media/image3.png)

> Fig. 3. Control Unit Design

Using a modular design approach, in this design, voltage values which will be obtained from sensing and adjustment units representing ambient temperature and target temperature are represented
as voltage sources namely "ROOM_TEMP" AND "SET_TEMP". U1 Op-Amp is the
difference amplifier, and it amplifies these signals 67 times. After
using an Op-Amp as voltage buffer this output voltage is fed into
decision unit using two oppositely directed diodes. After these diodes
output signals are amplified 100 times to obtain a voltage level around
10\~12V. After this amplification operations units are driven. A voltage
bias with a voltage level of 5V is connected at the other ports of the
operation unit resistances to obtain a \~5V voltage level to operate.
Operation units are represented with 1kOhm resistors in SPICE designs.

With the voltage levels given as input in Fig. 3, system is expected to
be driving only the cooler. 0.32V coming from sensing unit which
represents ambient temperature being 32 degrees Celsius and 0.28V coming
from adjustment unit representing 28 degrees Celsius, system must be
operating as a cooler to cool the environment down to target temperature
level. Simulation result showing a -5mA current is only passing through
the cooler resistance in this configuration can be seen in Fig. 4.

![](/media/image4.png)

> Fig. 4. Cooler ON Configuration Simulation Result

A configuration to allow system to perform as a heater is provided in
Fig. 5. Ambient temperature voltage level is still 0.32V and target
temperature voltage level is increased to 0.36V. Using this
configuration, a -5mA current is only passing from the heater as
expected in the simulation result which can be seen > Fig. 6.

![](/media/image5.png)

> Fig. 5. Heater ON Configuration

![](/media/image6.png)

> Fig. 6. Heater ON Configuration Simulation Result

System is expected to be in idle state, none of the operation units are
running, when there is a voltage difference level lower than 10mV. A
configuration for this is shown in the Fig. 7., SET_TEMP voltage value
is set as 0.32V and ROOM_TEMP voltage value is set as 0.315V. There is a
5mV voltage difference between these two inputs and amplifed voltage
output is not sufficient to exceed diode threshold voltage value and
thus, to drive none of the operating units as expected. The simulation
result regarding this configuration can be seen in Fig. 8.

![](/media/image7.png)

> Fig. 7. Idle Mode Configuration

![](/media/image8.png)

> Fig. 8. Idle Mode Simulation Result

## V.OPERATION UNIT

The operating unit is the subsystem that contains the elements which
perform heating and cooling functions of the system. A stone resistor as
heater, and a computer case fan (5-12V) can be used in the system
constructed. These elements are represented as 1kOhm resistors SPICE
designs. Amplified voltage coming from the control unit and the biasing
voltage provides a 5\~6V voltage value to operate these units.

## VI.DISPLAY UNIT

The display unit is the subsystem responsible for giving visual output
to users about the ambient temperature and target temperature set using
the temperature adjustment unit. The display unit would consist of a RGB
Led and a switch to change visual output between ambient and target
temperature. A color scale representing temperature levels between 24
and 40 degrees Celsius is required in display unit.

This requirement would be met designing and constructing an approach as
follows. Normalized voltage levels representing ambient and target
temperatures where every 10mV voltage value corresponding to a 1-degree
Celsius temperature interval, would be provided to this display unit.
Display unit would use these voltage levels to determine the duty cycles
to derive each leg of a common cathode or anode RGB led to obtain
required color output. A common cathode or anode RGB led depending on
which type is used, require a positive or negative voltage being
supplied to its legs, to provide various amounts of glowing of each red,
green, and blue colors. Resulting a color with various illumination
levels of red, green, and blue in visible spectrum depending on these
determined duty cycles.

Color LEDs will need 3\~3.2V to work (considering a grounded common
cathode RGB LED). A decision unit for this display unit would apply duty
cycle outputs using the adjustment and sensing unit voltage outputs to
apply 3\~3.2V signal with necessary duty cycle to each leg separately to
obtain desired color output. In further detail this decision subunit
would provide following

function, considering the normalized determined voltage level (240 --
400mV) coming from input, voltage levels lower than 240mV would only
power blue LED with constant 100% duty cycle. Starting from 240mV up to
320mV duty cycle of blue LED will start to decrease from 100% pulse
width modulation (PWM) and would be completely off after 320mV and
higher voltages. Green LED will start with 0% duty cycle at 240mV and
would reach 100% at 320mV. After 320mV green LED would start to decrease
and will be completely off at 400mV. Red LED would start from 0% duty
cycle at 320mV and reach 100% cycle at 400mV and stay there with 100%
PWM for voltage levels higher than that.

Since normalized voltage levels are already obtained and will be
supplied to this display unit, same circuitry to drive RGB led and
determining duty cycles of color legs would used for displaying both
ambient and target temperatures. A switch used for changing between the
voltage provided to this duty cycle deciding unit would be used to
displaying ambient and target temperatures.

## VII.TEMPERATURE ADJUSTMENT UNIT

Aim of this temperature adjustment unit is to allow users to set a
target temperature between 24 and 40 degrees Celsius with help of a
potentiometer. Sensing unit is providing a voltage output between -400mV - 1.1V corresponding to from -40 to 110degrees Celsius temperature where
voltage output is altered 10mV for every 1 degree Celsius in LM35
temperature sensor. Design specifications require a temperature interval
of 24 to 40 degrees Celsius. This adjustment unit is designed to be
provide a voltage output between 240 and 400mV. Design of the
temperature adjustment unit can be seen in Fig. 9.

![](/media/image9.png)

> Fig. 9. Temperature Adjustment Unit Design

A 100k potentiometer represented with the "POT1" and "POT2" naming for
each of its resistance between different legs used in the design.
Changing the resistance using this potentiometer which will work as a
voltage divider with the positive DC voltage input which is being used
throughout the system would provide a voltage deviation. This voltage
divider followed by a voltage amplifier of factor 1/20 and 1/3.3, a
positive voltage between \~1-2mV to \~160-170mV is obtained. This 160mV
voltage interval obtained using this circuitry constructed with the
potentiometer would allow changing the voltage in a 160mV voltage
interval (240mV 400mV range). To further normalize this temperature
adjustment output voltage. Using another voltage divider circuitry to DC
offset the system with a \~240mV voltage value is providing a voltage
value output between \~240 and \~400mV. Second voltage divider using two
resistors for dividing the DC input voltage (12V) to obtain this DC
voltage value. The DC offset voltage and potentiometer circuitry outputs
are summed using a voltage divider Op-Amp configuration and resulting
negative voltage output with a range of \~ -400mV (maximum output on one
end of the potentiometer, design and simulation results can be seen in
Fig. 11 and Fig. 12 respectively) and \~ -240mV (minimum output on other
end of the potentiometer, design and simulation results can be seen in
Fig. 9 and Fig. 10 respectively).

![](/media/image10.png)

> Fig. 10. Temperature Adjustment Unit Maximum Output Simulation Result

![](/media/image11.png)

> Fig. 11. Configuration for Minimum Output of Temperature Adjustment Unit

![](/media/image12.png)

> Fig. 12. Temperature Adjustment Unit Minimum Output Simulation Result

This negative output voltage of the temperature adjustment unit is
provided to the control unit using an inverting amplifier. Overall
diagram of this designed system including this temperature adjustment
unit, control unit and representative operation units can be seen in
Fig. 13. Used configuration in Fig. 13 showing a state of 240mV voltage
input from sensing unit (24 degrees Celsius) whereas the potentiometer
set to its maximum resistance to obtained maximum voltage from the
adjustment unit. Resulting voltage output of adjustment unit is fed into
the control unit as a voltage level of \~400mV as can be seen in the
Fig. 10. In this configuration target temperature is 40 degrees Celsius
and heater is expected to perform. As can be seen in the simulation
results in Fig. 14 only heater operation is running since target
temperature is greater than the ambient temperature.

![](/media/image13.png)

> Fig. 13. Overall System Design Diagram

![](/media/image14.png)

> Fig. 14. Simulation Result Showing Heater Performing

When ambient temperature is higher than 24 degrees Celsius, providing a
voltage level greater than 240mV to control unit while target
temperature is set to 24 degrees Celsius with minimum configuration of
adjustment unit giving 240mV output, only the cooler operation is run as
expected. Configuration and simulation results can be seen in Fig. 15
and Fig. 16 respectively.

![](/media/image15.png)

> Fig. 15. Configuration to Run The Cooler

![](/media/image16.png)

> Fig. 16. Simulation Result Showing Cooler Performing

When temperature adjustment unit and sensing unit voltage output
difference is less than 10mV corresponding to a temperature difference
less than 1-degree Celsius system is expected to be in idle mode where
none of the operation units are working. To obtain an idle state,
sensing unit output voltage is set to 235mV corresponding to 23.5
degrees Celsius and temperature adjustment unit is set to its minimum
state where it is providing a voltage level of \~240mV. This
configuration can be seen in Fig. 17. As expected, since voltage
difference between sensing unit and adjustment unit outputs which is
corresponding to a temperature difference less than 1-degree Celsius,
thanks to diodes used after the control unit while driving the operation
units that threshold voltage provides system an idle state when the
voltage difference is less 10mV. Simulation results can be seen in Fig.
18 showing the idle mode of the system.

![](/media/image17.png)

> Fig. 17. Idle Mode Configuration

![](/media/image18.png)

> Fig. 18. Idle Mode Simulation Result

## VIII.CONCLUSION

To conclude, a micro air conditioner system which automatic functioning
given a target temperature input, can be constructed using the design
approaches, subsystems designed and numerical values for the circuit
elements mentioned in this report. Further designs approaches can be
developed for example to provide system a resting interval after passing
the target temperature with a determined margin to ensure system won't
turn on and off continuously at edge of a 1-degree Celsius temperature
difference between target and ambient temperatures. Dynamic circuit
elements for allowing system to be keep performing its cooling or
heating operations after control unit output is set off to create this
mentioned margin can be used in these applications.

Even though some realistic models for circuit elements are using in
SPICE designs, practical application of this micro air conditioner would
result in errors in expected outputs from subsystems. Further
investigation and fine tuning of resistances used in the design would be
required to obtain desired voltage levels and intervals for given design
specifications.

## REFERENCES

> \[1\] Texas Instruments. (2017, December). _LM35 Precision Centigrade
> Temperature Sensors Datasheet_. Texas Instruments Symlink. Retrieved
> February 10, 2022, from https://www.ti.com/lit/ds/symlink/lm35.pdf
