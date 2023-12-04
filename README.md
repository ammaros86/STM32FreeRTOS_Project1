# STM32 FreeRTOS Project1

STM32 FreeRTOS Project 1

In this project, an STM32L476RG microcontroller was utilized. The microcontroller was configured as follows:

- SysClock set to 80 MHz
- UART 2 is activated to transfer UART Data
- FreeRTOS was activated using CMSIS_V1 with the default configuration.
- Two tasks, "task1" and "task2," were created
- Task 1 has normal priority (osPriorityNormal), and Task 2 has Idle priority (osPriorityHigh)
- Timebase Source was configured to Timer1 "TIM1"
- SEGGER System View was added to this project
  
Figure 1 illustrates the SysTick interrupt, which occurs every 1 ms. After the SysTick interrupt is completed, the scheduler
is activated, responsible for deciding which task will be executed next.

![Screenshot 04-12-2023 11 25 00](https://github.com/ammaros86/STM32FreeRTOS_Project1/assets/56800295/dda601e8-a0f0-41ad-a647-9842a73e1aa2)

In Figure 2, it is shown that during the SysTick interrupt, Task 2, which has a higher priority, receives the "Ready" status. 
The scheduler is then activated, initiating Task 2. In Task 2, a message is transmitted via UART with the text "Task 02 \n". 

![Screenshot 04-12-2023 11 26 32](https://github.com/ammaros86/STM32FreeRTOS_Project1/assets/56800295/54891b51-cb38-40ff-8ddc-add0850feef1)

Following this, the osDelay method is invoked with a parameter of 4, causing Task 2 to be blocked for 4 ms. Task 2 is preempted 
before the next SysTick is completed, during which the Idle Task comes into play.

Shortly thereafter, the SysTick interrupt is reactivated, and during this time, Task 1 is assigned the "Ready" status. 

![Screenshot 04-12-2023 11 32 24](https://github.com/ammaros86/STM32FreeRTOS_Project1/assets/56800295/d6e862fd-1a80-4c05-8472-e0a73a42570c)

The scheduler, activated shortly afterward, allows Task 1 to run. While executing Task 1, a text is sent via UART with the message 
"Task 01 \n", and the osDelay method is also called with a parameter of 4, introducing a 4 ms delay until the next execution.

After both tasks are executed, the idle task continues to run until the 4 ms wait time of Task 2 is completed, corresponding to 4 SysTicks.
Subsequently, Task 2 restarts, followed by Task 1, and the entire process repeats.
