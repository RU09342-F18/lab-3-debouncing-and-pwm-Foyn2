# Software PWM
The purpose of this portion of the lab is to produce a Pulse-Width Modulation (PWM) whose duty cycle can be incremented by 10% every time that the button is pressed. By implementing this, The LED should brighten up (increasing duty cycle by 10%) until the duty cycle reaches 100%, and after that it should reset back down to 0%.

Because of the general format of PWM, A Capture Compare Register was utilized for the processor to decide between when to output high and when to output low. This switch between both high and low creates the PWM necessary for the processor to use. CCR0 is set at 1000 and does not change through the process. Timer A1 in CCR0 is set to 50000, which allows the timer to overflow every 10ms, creating the 10% increase in the duty cycle. The for loop created right after this puts the duty cycle incrementation into play. Two interrupts are created, one for a basic timer interrupt of both timers (A0, A1) in CCR0; and the other for incrementing the duty cycle if the value is less than 100% or equals 100%. Switch states were used with the cases of Timer A0 and A1 being the same (When the values are counted up to, the interrupts are disbled). The duty cycle cases were there so that the LED would keep incrementing by 10% if the PWM value was not at 100%. The other case would be to reset the duty cycle back down to 0 once the value of the pwm did equal 100%. The PWM was set to 50% with the line of code: 
```c 
int pwm = 500;
```
By doing this, we would eliminate the need for using two Capture Compare registers and simply set the pwm value to start at 50%.

Both the G2553 and F5529 had similar implementations, but the pin assignments were different. Even so, the line of code mentioned above was also used in the F5529 so that an additional CCRn register did not have to be used.

Most microprocessors will have a Timer module, but depending on the device, some may not come with pre-built PWM modules. Instead, you may have to utilize software techniques to synthesize PWM on your own.

## Task
You need to generate a 1kHz PWM signal with a duty cycle between 0% and 100%. Upon the processor starting up, you should PWM one of the on-board LEDs at a 50% duty cycle. Upon pressing one of the on-board buttons, the duty cycle of the LED should increase by 10%. Once you have reached 100%, your duty cycle should go back to 0% on the next button press. You also need to implement the other LED to light up when the Duty Cycle button is depressed and turns back off when it is let go. This is to help you figure out if the button has triggered multiple interrupts.

## Deliverables
You will need to have two folders in this repository, one for each of the processors that you used for this part of the lab. Remember to replace this README with your own.

### Hints
You really, really, really, really need to hook up the output of your LED pin to an oscilloscope to make sure that the duty cycle is accurate. Also, since you are going to be doing a lot of initialization, it would be helpful for all persons involved if you created your main function like:

```c
int main(void)
{
	WDTCTL = WDTPW | WDTHOLD;	// stop watchdog timer
	LEDSetup(); // Initialize our LEDS
	ButtonSetup();  // Initialize our button
	TimerA0Setup(); // Initialize Timer0
	TimerA1Setup(); // Initialize Timer1
	__bis_SR_register(LPM0_bits + GIE);       // Enter LPM0 w/ interrupt
}
```

This way, each of the steps in initialization can be isolated for easier understanding and debugging.


## Extra Work
### Linear Brightness
Much like every other things with humans, not everything we interact with we perceive as linear. For senses such as sight or hearing, certain features such as volume or brightness have a logarithmic relationship with our senses. Instead of just incrementing by 10%, try making the brightness appear to change linearly.

### Power Comparison
Since you are effectively turning the LED off for some period of time, it should follow that the amount of power you are using over time should be less. Using Energy Trace, compare the power consumption of the different duty cycles. What happens if you use the pre-divider in the timer module for the PWM (does it consume less power)?
