# Software PWM
The purpose of this portion of the lab is to produce a Pulse-Width Modulation (PWM) whose duty cycle can be incremented by 10% every time that the button is pressed. By implementing this, The LED should brighten up (increasing duty cycle by 10%) until the duty cycle reaches 100%, and after that it should reset back down to 0%.

Because of the general format of PWM, A Capture Compare Register was utilized for the processor to decide between when to output high and when to output low. This switch between both high and low creates the PWM necessary for the processor to use. CCR0 is set at 1000 and does not change through the process. Timer A1 in CCR0 is set to 50000, which allows the timer to overflow every 10ms, creating the 10% increase in the duty cycle. The for loop created right after this puts the duty cycle incrementation into play. Two interrupts are created, one for a basic timer interrupt of both timers (A0, A1) in CCR0; and the other for incrementing the duty cycle if the value is less than 100% or equals 100%. Switch states were used with the cases of Timer A0 and A1 being the same (When the values are counted up to, the interrupts are disbled). The duty cycle cases were there so that the LED would keep incrementing by 10% if the PWM value was not at 100%. The other case would be to reset the duty cycle back down to 0 once the value of the pwm did equal 100%. The PWM was set to 50% with the line of code: 
```c 
int pwm = 500;
```
By doing this, we would eliminate the need for using two Capture Compare registers and simply set the pwm value to start at 50%.

Both the G2553 and F5529 had similar implementations, but the pin assignments were different. Even so, the line of code mentioned above was also used in the F5529 so that an additional CCRn register did not have to be used.

