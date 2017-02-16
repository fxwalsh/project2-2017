# Wyliodrin: Sensors and Dashboard

## Street Lightning
#### source:  http://ocw.cs.pub.ro/courses/iot/labs/08

### Introduction

<div class="level3">

As previously stated, the classical Arduino board is basically a micro-controller, which is capable of running one piece of software at once and that has little processing power and no network connectivity. So you will use the board to gather data from the environment and then pass it on to the Raspberry Pi.

The Raspberry Pi is a computer that is capable of processing data and communicating with other smart devices. For instance, you could visualise the temperature on your smart phone.

The two boards need to be connected in order to send data between them. You can connect them via the USB cable and a serial connection will be established between the two. Once this is done, they can exchange data and the Arduino board can be controlled via the Raspberry Pi. This is done by the **firmata** protocol. The protocol allows the Raspberry Pi to send the Arduino messages in which it requests for a certain action or information and the Arduino will respond accordingly.

In order to implement the protocol, you need to flash the Arduino with the **StandardFirmata** firmware.

In the Wyliodrin STUDIO interface you have two tabs referring to **SOFTWARE** and **FIRMWARE**. So far, you used only the **SOFTWARE** tab, as you wrote applications for the Raspberry Pi solely. Now, select the **FIRMARE** tab and there is where you can write code which will be run on the Arduino. In this case, you will import an existing project.

[![](/courses/_media/iot/labs/select-firmata.png?w=300&tok=4049a1)](/courses/_detail/iot/labs/select-firmata.png?id=iot%3Alabs%3A08 "iot:labs:select-firmata.png")

Hit the **Show examples** button and select **Arduino**. Then **use** the **Firmata/StandardFirmata** example. Now that you have the software to run on the Arduino, once you hit the **run** button, you will be asked the tyype of the Arduino board and if you want to flash it or not. Select the board you are using and **RUN AND FLASH**. The **StandardFirmata** firmware will be deployed on the Arduino.

As you know, any micro-controller, including the Arduino, once flashed, runs the same firmware until another one is uploaded on the board. Thus, you don't have to flash the board each time you run a new Raspberry Pi application. If you are confident that the Arduino is running **StandardFirmata**, you can skip this step. [![](/courses/_media/iot/labs/flash-arduino.png?w=300&tok=0f36de)](/courses/_detail/iot/labs/flash-arduino.png?id=iot%3Alabs%3A08 "iot:labs:flash-arduino.png")

</div>



<div class="level2">

In this application, you will build a system to monitor the light level and if there is the case, you will turn on the street lights brighter or dimmer, depending on the amount of light.

</div>

### What you need



*   <div class="li">One Raspberry Pi connect to \Wyliodrin;</div>

*   <div class="li">One Arduino connected to the Raspberry Pi;</div>

*   <div class="li">One photocell;</div>

*   <div class="li">One LED;</div>

*   <div class="li">One 220 <span class="MathJax_Preview" style="color: inherit; display: none;"></span><span tabindex="0" class="MathJax" id="MathJax-Element-1-Frame" role="presentation" style="position: relative;" data-mathml='<math xmlns="http://www.w3.org/1998/Math/MathML"><nobr aria-hidden="true"><span class="math" id="MathJax-Span-1" style="width: 0.92em; display: inline-block;"><span style="width: 0.73em; height: 0px; font-size: 126%; display: inline-block; position: relative;"><span style="left: 0em; top: -2.27em; position: absolute; clip: rect(1.38em, 1000.69em, 2.45em, -1000em);"><span class="mrow" id="MathJax-Span-2"><span class="mi" id="MathJax-Span-3" style="font-family: MathJax_Main;">Ω</span></span><span style="width: 0px; height: 2.27em; display: inline-block;"></span></span></span><span style="width: 0px; height: 1.04em; overflow: hidden; vertical-align: -0.07em; border-left-color: currentColor; border-left-width: 0px; border-left-style: solid; display: inline-block;"></span></span></nobr><span class="MJX_Assistive_MathML" role="presentation"><math xmlns="http://www.w3.org/1998/Math/MathML"><mi mathvariant="normal">Ω</mi></math></span><mi mathvariant="normal">&amp;#x03A9;</mi></math>'></span><script id="MathJax-Element-1" type="math/tex">\Omega</script> resistor;</div>

*   <div class="li">One 10 k<span class="MathJax_Preview" style="color: inherit; display: none;"></span><span tabindex="0" class="MathJax" id="MathJax-Element-2-Frame" role="presentation" style="position: relative;" data-mathml='<math xmlns="http://www.w3.org/1998/Math/MathML"><nobr aria-hidden="true"><span class="math" id="MathJax-Span-4" style="width: 0.92em; display: inline-block;"><span style="width: 0.73em; height: 0px; font-size: 126%; display: inline-block; position: relative;"><span style="left: 0em; top: -2.27em; position: absolute; clip: rect(1.38em, 1000.69em, 2.45em, -1000em);"><span class="mrow" id="MathJax-Span-5"><span class="mi" id="MathJax-Span-6" style="font-family: MathJax_Main;">Ω</span></span><span style="width: 0px; height: 2.27em; display: inline-block;"></span></span></span><span style="width: 0px; height: 1.04em; overflow: hidden; vertical-align: -0.07em; border-left-color: currentColor; border-left-width: 0px; border-left-style: solid; display: inline-block;"></span></span></nobr><span class="MJX_Assistive_MathML" role="presentation"><math xmlns="http://www.w3.org/1998/Math/MathML"><mi mathvariant="normal">Ω</mi></math></span><mi mathvariant="normal">&amp;#x03A9;</mi></math>'></span><script id="MathJax-Element-2" type="math/tex">\Omega</script> resistor;</div>

*   <div class="li">Jumper wires.</div>

</div>

### The Setup

<div class="level3">

![fritzing schema](./img/light_sensor_schematics.png)

The photocell works just like resistor with a variable resistance. Depending on the amount of light it receives, its resistance gets higher or lower. As a result, you can connect it in a voltage divider, as well.

The sensor is connected to the 5V via the 10 K Ohms resistor and to the GND pin on the other side. In order to read the sensor's value, the yellow jumper wire is connected to the A0 pin of the board. The A0-A5 pins can be used for reading digital values, in this case values ranging from 0 to 1024\.

For the photocell, the resistance decreases proportionally with the amount of light, so for this schematics, the brighter the environment, the higher is the read value.

In addition, the photocell has a resistance varying from hundred of ohms to mega ohms and in order for the voltage divider to work, you should use the sensor as a pull-down resistor, while the actual resistor needs to have a resistance comparable with the sensor's, otherwise its effect is not visible. This is why you need a 10 k Ohms resistor.

The other part of the setup, the LED is connected similar to the previous schematics except that you can notice that its behaviour is controller by pin 3\. That pin has a tilde next to it. That means that it is a PWM pin so you can control if the LED should light up brighter or dimmer. You can write values ranging from 0 to 255 on these pins, 0 being the equivalent of digital 0 and 255 the equivalent of digital 1\.

</div>

### The Code

<div class="level3">

[![](/courses/_media/iot/labs/street_lights_code.png?w=400&tok=6b4d67)](/courses/_detail/iot/labs/street_lights_code.png?id=iot%3Alabs%3A08 "iot:labs:street_lights_code.png")

First of all, you need to use an **Arduino in** node in order to read the values from the sensor. This node gets triggered each time the value on the pin it reads from changes. To actually configure the node's behavior you need to set its properties.

[![](/courses/_media/iot/labs/arduino_in_properties.png?w=400&tok=efa9fc)](/courses/_detail/iot/labs/arduino_in_properties.png?id=iot%3Alabs%3A08 "iot:labs:arduino_in_properties.png")

The first property allows you to set the Raspberry Pi connection to the Arduino board so the Raspberry Pi knows the port through which it should send the messages. Usually, the port is **/dev/ttyACM0**. However, in order to make sure, go to the **Shell** tab and type **dmesg**. There you should see messages stating that an FTDI cable has been connected to the board and the port it is connected to. Concatenate that value to **/dev/** and place the value as the first property of the node.

Further on, you need to specify which pin you want to read from. In this case, the pin is 0 and is an analog pin. Afterwards, you will use a **range** node in order to map the read values to values that should light up the LED. For this, you need to adapt the system to your environment. Depending on the regular amount of light, usually you will read a value of around 400\. So in this case, the range scales values going from 400 to 1024 to 0 to 255\. However, if the regular value is different, you will need to scale a different range.

[![](/courses/_media/iot/labs/range-light.png?w=400&tok=757879)](/courses/_detail/iot/labs/range-light.png?id=iot%3Alabs%3A08 "iot:labs:range-light.png")

The last node to use is **Arduino out**. This node acts just like the **digital write** node used in the previous chapters. However, you still need to select the port for the Arduino board and you can choose from analog or digital write. As you want to make the LED light up brighter or dimmer, you will set the type to **analog** and the **pin** property has to be 3\.

<div class="noteclassic">If you use more than one Arduino node in the application, add a new Raspberry Pi board only for one node. For the rest of the nodes, choose the previously added board from the drop-down.</div>

Now you can run the application and watch how the LED gets brighter as you cover up the sensor.

</div>

## Exercises

<div class="level2">

1.  <div class="li">Display the values coming from the light sensor in the dashboard using the **gauge** graph.</div>

2.  <div class="li">Connect all the sensors in the kit to the Arduino and display the values you read in the dashboard. **Note:** Do not connect the Hall sensor, which works in a different way.</div>

3.  <div class="li">Search the temperature sensor's datasheet online (TMP36) and transform the read values to celsius degrees.</div>

</div>
