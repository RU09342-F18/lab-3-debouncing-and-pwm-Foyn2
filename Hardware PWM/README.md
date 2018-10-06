# Hardware PWM
Similar to what was implemented in the Software PWM, the Hardware PWM was to be designed to increment the duty cycle by 10%, and reset back down to 0% once the duty cycle was at 100%. The LED was to showcase the same behavior as well, brightening by 10% to correspond with the duty cycle increasing by 10% after each button press.

Instead of setting the LED cases manually, as we did in the software PWM via the switch statements, the hardware PWM required use of the OUTMOD hardware feature. In this case, OUTMOD7 was used, which allows a reset/set functionality. By setting OUTMOD7 to CCR1, the microprocessor will carry out the functions implemented in the software PWM. The CCR1 register will count up to its value, trigger a reset, and then toggle once the value in CCR0 is reached. This way, the two registers are compared through the OUTMOD7 hardware functionality, eliminating the need to implement cases for the different states of the LEDS. The only interrupts utilized are a button interrupt (occurs when the button in pressed) and the timer interrupt (stops the timer once 100% duty cycle is reached). 

The differences between the G2553 and the FR2311 were the ports and the Timer module names. While all of the timers in the G2553 were "TA", the nomenclature in the FR2311 changed to "TB"

