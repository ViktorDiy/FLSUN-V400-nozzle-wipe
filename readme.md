It really can save a lot of time...</br>
https://www.facebook.com/100094217897627/videos/2682152451932166/ </br>
</br>
For this setup you need:</br>
</br>
- x1 M4 20mm screw </br>
- x2 M4 nut</br>
- x2 bearing 624 ZZ or another wiht 4mm inner diameter, 13mm outer diameter and 5mm width</br>
- x1 MG90S Metal Gear Servo like this - https://www.aliexpress.com/item/1005004195697567.html</br>
- x1 metal brush like this - https://www.aliexpress.com/item/1005004449962700.html</br>
</br>
First step - you should print 3 parts from STL files - base, arm and bucket. It was developed for limited printer space for most popular enclosure in internet.</br>
</br>
It will be usefull to read about this nozzel wiper for Voron 3d printer: https://github.com/VoronDesign/VoronUsers/tree/main/printer_mods/chirpy/NozzleWiper</br>
</br>
Usually you can connect your PWM servo pin via BLTOUCH PWM pin on 3d printer MCU (PA8 for FLSUN castrated clone of MKS Robin nano v2), but after failure try I find 
out that on this pin I have non standard PWM signal, but heart attack cardiogram )))</br>
</br>

I connected PWM servo pin to E1 STEP pin PD15 and power to BLTOUCH GND and +5V:</br>
![image](https://github.com/ViktorDiy/FLSUN-V400-nozzle-wipe/assets/147925158/a32e8b0b-0394-4e7f-bca7-0ee199edee72)</br>
</br>
MG90S servo pinout:</br>
![image](https://github.com/ViktorDiy/FLSUN-V400-nozzle-wipe/assets/147925158/5bb4ba07-d599-49fb-af3d-2f71cc971184)</br>
</br>
Assembled rotation part:</br>
![image](https://github.com/ViktorDiy/FLSUN-V400-nozzle-wipe/assets/147925158/1b627ad5-fa38-441a-a3de-f6b337808114)</br>
</br>
I used this type of servo arrow arm:</br>
![image](https://github.com/ViktorDiy/FLSUN-V400-nozzle-wipe/assets/147925158/b05194e9-f32c-494f-8739-ed448f7d2972)</br>
</br>
Assembled parts in middle arm position of servo should be very close to enclosure wall:</br>
![image](https://github.com/ViktorDiy/FLSUN-V400-nozzle-wipe/assets/147925158/7e75890f-0aca-4b11-99e7-11304e907240)</br>
</br>
Then you should add to your printer.cfg file string [include nozzlewiper.cfg].</br>
</br>
And download to your printer configuration folder example file nozzlewiper.cfg</br>
</br>
After save and restar on printer dashbord will appear nozzel wiper macro with prefix "NW".</br>
</br>
Befor first use you should calibrate initial/parking position angle of arm and working position angle (minimum_pulse_width: for 0 arm angle and maximum_pulse_width: for
180 angle).</br>
I use command line on dashboard for that. I try to find by command "SET_SERVO SERVO=wipeServo WIDTH=0.001" number when arm doesn't move after WIDTH decrease and the same
command wiht WIDTH increase for maximum angle.</br>
Then you should find by this command volumes for parking position angle and wiping position angle. Wiping position angle of arm should be 90 degree angle between arm
and Xaxis (front/face of printer).</br>
</br>
And you should find nozzle position under center of the backet. You should move printer head manualy to nozzel position under center of the backet and 1-2 mm lower
than metal brash.</br>
You should write this X,Y,Z volumes to nozzelwiper.cfg file.</br>

How I use this setup... How I configured it...

I put macroses from nozzelwiper.cfg to slicer Machine start G-CODE and it looks like:</br>
</br>
G21</br>
G90</br>
M82</br>
G28</br>
NW_DEPLOY</br>
M107 T0</br>
M140 S[bed_temperature_initial_layer_single]</br>
M104 S[nozzle_temperature_initial_layer] T0</br>
M190 S[bed_temperature_initial_layer_single]</br>
M109 S[nozzle_temperature_initial_layer] T0</br>
NW_WIPE</br>
NW_RETRACT</br>
G1 F3000 Z1</br>
G1 X-150 Y0 Z0.4</br>
G92 E0</br>
G3 X0 Y-130 I150 Z0.3 E30 F2000</br>
G92 E0</br>
</br>

![image](https://github.com/ViktorDiy/FLSUN-V400-nozzle-wipe/assets/147925158/1a70a169-4a22-4097-bb0b-95009762da92)</br>
</br>
Befor start of heating nozzle and plate bucked should move to wiping position and nozzel to the center under the bucket. During nozzel and plate heating waste filament from nozzel will drop to the bucket. After nozzel and plate heating nozzel will be wiped by metal brush. </br>
And then printing will start as usual )))
