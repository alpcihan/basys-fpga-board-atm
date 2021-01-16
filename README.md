# CS303-Logic-Digital-System-Design

Design and the implementation of a simple ATM (Automated Teller Machine) on the BASYS FPGA board.

In the design, there is login operation where the user first should insert its debit card and then enter his/her password. After having entered the password correctly, user may do: 1) deposit/withdraw money to/from his/her account, 2) change the password of the account or 3) log out from the ATM. 

## Table of content
* [Inputs](#inputs)
* [Outputs](#outputs)
* [ASM Chart](#asm-chart)
* [Operation Steps](#operation-steps)
* [Modules](#modules)
* [License](#license)

## Inputs

There are 5 (five) inputs in the circuitry:

*	rst sets the circuitry to its initial state. This input should be provided from BTN0 of the BASYS board.

*	BTN3 is used for different operations in different states. For example, in the IDLE state, it is used to insert the debit card while it is used for entering the password in another state. Details will be provided below. This input should be provided from BTN3 of the BASYS board.

*	BTN2 will be used for changing your password and withdrawing money from your account.  Details will be provided below. This input should be provided from BTN2 of the BASYS board.

*	BTN1 will be used to log out from ATM and navigating in ATM menu. Details will be provided below. This input should be provided from BTN1 of the BASYS board.

*	SW is a 4-bit input and it will be used to set password or the amount of money to deposit/withdraw to/from your account. Details will be provided below. This input should be provided from SW[3:0] of the BASYS board, where SW[0] will be the least significant bit of its value.

## Outputs
There are 2 (two) different types of outputs in your circuitry: 

*	LED outputs: You are required to show in which state your system is by using LEDs. Details will be provided below. 

*	7-segment displays (SSDs): SSDs will be used to show messages and balance information. Details will be provided below. 

## ASM Chart
Here is the ASM Chart of the circuitry:

<img width="800" alt="Screen Shot 2021-01-16 at 20 18 18" src="https://user-images.githubusercontent.com/37274614/104818431-aaafe880-5838-11eb-950f-74f31db82f53.png">

## Operation Steps
The circuitry starts in the IDLE state, in which it displays ‘CArd’ in SSDs with only LED0 is being ON. In the IDLE state, the user can insert its debit card by simply pressing BTN3. Then, the circuitry goes to the password entry state where ‘PE-3’ will be displayed in SSDs with only LED7 is being ON. The initial password of the user is ‘0000’ and the user should enter the password by setting the SW input and pressing BTN3.

The user has 3 rights to enter the password correctly at the password entry state. If the user enters the password wrong for the first time, LED6 should also be ON along with LED7 and ‘PE-2’ should be displayed on SSD. If the user enters the password wrong for the second time, LED5 should also be ON along with LED6 and LED7, and ‘PE-1’ should be displayed on SSDs. If the user enters the password wrong for the third time in a row, the ATM should be locked (no button works except for the reset button) for 5 seconds where all LEDs should be ON and ‘FAIL’ should be displayed on SSDs. After 5 seconds, the circuitry should go to the IDLE state. If the user presses BTN1 at any point of the password entry stage, it should be logged out from the system and go to the IDLE state.

If the user enters the password correctly, it goes to the ATM menu state where only LED4 is ON and ‘OPEn’ is displayed on SSDs. In this state, the user has 3 options: 1) press BTN3 and perform money deposit/withdraw operations, 2) press BTN2 and change the password, 3) press BTN1 and log out from the ATM (go to IDLE state).

If the user presses BTN3 in the ATM menu state, the circuitry goes to a state where the user can see the account balance and deposit/withdraw money to/from the account. In this state, only LED3 is ON and the current balance of the user gets displayed on SSDs in 4-digit hexadecimal format. In this state (money operation state), the user can deposit money to the account by setting the SW input and pressing BTN3. After the money is deposited into the account, the updated account balance gets displayed on SSDs. In this state, the user can withdraw money from the account by setting the SW input and pressing BTN2. After the money is withdrawn from the account, the updated account balance gets displayed on SSDs. If the user tries to withdraw an amount of money that is higher than the money in the account, the circuitry does not perform the operation and it gets locked for 2.5 seconds (no button works except for reset button), and ‘-NA-’ gets displayed on SSDs with all LEDs are ON. Then, the circuitry goes back to the money operation state. In this state, if the user presses BTN1, the circuitry goes back to the ATM menu state.

If the user presses BTN2 in the ATM menu state, the circuitry goes to a state where the user can change the password. In this state (password change state), only LED2 is ON and ‘PC-3’ gets displayed on SSDs. In this state, the user first should enter the current password by setting the SW and pressing BTN3. The user has 3 rights to enter the current password correctly. If the user enters the password wrong for the first time, LED1 should also be ON along with LED2 and ‘PC-2’ should be displayed on SSD. If the user enters the password wrong for the second time, LED0 should also be ON along with LED1 and LED2, and ‘PC-1’ should be displayed on SSDs. If the user enters the password wrong for the third time, the circuitry gets locked (no button works except for reset button) for 5 seconds, all LEDs ON, and ‘FAIL’ gets displayed on SSDs. After 5 seconds, the circuitry goes to the IDLE state. If the current password is entered correctly, only LED1 is ON and ‘PASS’ gets displayed on SSDs. Then, the user should enter the new password by setting the SW and pressing BTN3. Then, the circuitry goes back to the ATM menu state. In the password change state, if the user presses BTN1, the circuitry goes back to the ATM menu state.

If the user presses BTN1 in the ATM menu state, the user gets logged out and the circuitry goes to the IDLE state.

Unless the user resets the circuitry, the user’s current balance and password should not be reset. For example, the user enters the system, changes the password, deposit money into the account, and log out from the ATM. If the user wants to re-enter the ATM, he/she should use the new password (and the user’s account balance should be equal to the amount of money deposited.).

## Modules

* **The clock divider module** (clk_divider.v) generates a clock signal with a period of 50 ms, from a 25 MHz input clock (Please note that the BASYS boards can provide 3 different clock frequency: 25 MHz, 50 MHz, 100 MHz. These are set using the jumpers on the BASYS boards. We set the clock of all BASYS boards to 25 MHz.). 

* **The debouncer** (debouncer.v) circuit gets the input from a push button and generates a one clock pulse output.

*	**The seven segment driver** (ssd.v) module, drives the segments.

* **The Top module** (top_module.v) binds the clock divider, the debouncer and the seven segment driver modules.


## License
[MIT](./LICENSE)
