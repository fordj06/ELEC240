---
layout: "post"
title: "ELEC240 Notes"
date: "2018-12-10 06:52"
---

# ELEC240 - Interrupts

## Exceptions & Interrupts

Exceptions are events that cause changes to the program flow. When an exception is raised by an internal or external event, the processor suspends the current task after completion of the command that is currently in operation and then executes a part of the program called **the exception handler**. After the execution of the exception handler in completed, the processor resumes normal program execution form the point in which it exited the program when the exception was raised. In the ARM architecture, interrupts are one type of exception. Interrupts are usually generated from peripherals or external inputs, and in some cases are triggered by software (traps). The exception handlers for interrupts are also referred to as **interrupt service routines (ISR)**.

Exceptions are processed by the **Nested Vector Interrupt Controller (NVIC)**. The NVIC can handle a number of interrupt requests (IRQs) and a Non-Maskable Interrupt (NMI) request. NMI is an interrupt that cannot be disabled.  Usually IRQs are generated by on-chip peripherals or from external interrupt inputs through the I/O ports. The NMI could be used by a watchdog timer or a brownout detector (a voltage monitoring unit that warns the processor when the supply voltage drops below a certain level).

The processor itself is also a source of exception events. These could be fault events that indicate system error conditions, or exceptions generated by software to support embedded OS operations.

#### Exception Types

![alt text](https://raw.githubusercontent.com/fordj06/ELEC240/master/img/interrupt_types.png "exception types")


Each exception source has an exception number. Exception numbers 1 - 15 are classified as system exceptions. Exceptions 16 - 255 are for interrupts. The design of the NVIC in the Cortex-M4 processor can support up to 240 interrupt inputs. In practice the number of interrupt inputs implemented in the design is far less, typically in the range of 16 to 100.

The exception number is reflected in various registers and is used to determine the exception **vector address**. Exception vectors are stored in a **vector table**. The processor reads this table to determine the starting address of an exception handler during the entrance sequence. Note that the exception number definitions are different from interrupt numbers in the CMSIS device-driver library. In the CMSIS driver library, interrupt numbers start from 0, and system exception numbers have negative number values.

**Interrupt latency** of the Cortex-M4 is very low, at a minimum of 12 clock cycles. This is very fast but does not account for and software overhead or extra wrapper code that may be required. It could also be slower than polling depending on the application. If you were continuously polling a push button with no delay between checks, it would be faster than using an external interrupt but also very inefficient.  

#### NESTED VECTOR INTERRUPT CONTROLLER (NVIC)

The NVIC is a part of the Cortex-M processor. It is programmable and its registers are located in the System Control Space (SCS) of the memory map. The NVIC handles the exception and interrupt configurations, prioritization, and interrupt masking. The NVIC has the following features:

  - Exception and interrupt management
  - Nested exception/interrupt support
  - Vectored exception/interrupt entry
  - Interrupt masking

###### Management
Each interrupt (apart from the NMI) can be enabled or disabled and can have its pending status set or cleared by software. The NVIC can handle various types of interrupt sources:

  - *Pulsed interrupt request* - the interrupt request is at least one clock cycle long. When the NVIC receives a pulse at its input, the pending status is set and held until the interrupt gets serviced.
  - *level triggered interrupt request* - the interrupt source holds the request high until the interrupt is serviced.

The signal level at the NVIC input is active high. However, the actual external interrupt input on the microcontroller could be designed differently and is converted to active high signal by on-chip logic.

###### Nested Support
Each exception has a priority level. Some exceptions, such as interrupts, have programmable priority levels and others (e.g., NMI) have fixed priority. When the exception occurs, the NVIC will compare the priority level of this exception to the current level. If the new exception has a higher priority, the current running task will be suspended. Some of the registers will be stored on the stack memory, the processor will start executing the handler of the new exception. This process is called **"preemption"**. When the higher priority exception handler is complete, it is terminated with an exception return operation and the processor automatically restores the registers from the stack and resumes the task that was previously running. This mechanism allows nesting of exception services without any software overhead.

###### Vectored entry
When an exception occurs, the processor will need to locate the starting point of the corresponding exception handler. The Cortex-M processors automatically locate the starting point of the exception handler from the vector table in memory. As a result, the delays from the start of the exception to the execution of the handlers are reduced.

###### Interrupt Masking
The NVIC in the Cortex-M4 provides several interrupt masking registers such as PRIMASK special register. Using the PRIMASK register you can disable all exceptions, excluding HardFault and NMI. This masking is useful for operations that should not be interrupted, like critical control tasks or real-time multimedia codecs. You could also use the BASEPRI register to select mask exceptions or interrupts which are below a certain priority level.


###### Polling Vs Interrupt Driven

- **Interrupt** - Whenever a device needs servicing, it notifies the MCU by send it an interrupt signal.
  - When a flag has been raised the MCU stops whatever it is doing and serves the interrupt.
  - It first completes the current machine instruction!

- **Polling** - The MCU continuously monitors the status of a given device; when the status condition is met, it performs the service.
  - After that it moves on to the next device until each is served.
  - Although polling can monitor the status of several devices and serve each of them as certain conditions are met, its not efficient use of the microcontroller. Neither is interrupts if the flag is raised often enough.

###### Pros / Cons

- Interrupts should be used for infrequent events (1000s of clock cycles)
- MCU response time to interrupts is very fast - 12 clock cycles for the ARM Cortex-M.
- If response time is critical, a tight polling loop is used.
  - polling can be faster than Interrupts
- Polled code leads to variable latency in servicing peripherals
  - Interrupts give highly predictable servicing latencies
- Polling uses a lot of power check the device to see if the condition has been met
- Polled code is generally messy and unstructured
  - Poor use of interrupts can also result in code that is hard to read and debug


##### Interrupt Advantages

- Ensure adequate response times
- Facilitate event-driven applications and preemptive multitasking
- Maximize MCU utilization and efficiency
- Allow use of sleep/idle states to save power
  - Vital for battery power applications

##### Traps
Traps are programmer initiated and therefore expected transfer of control to a special handler routine. Also known as Software Interrupts (SWI). Traps are synchronous interrupts.