# Sample_Project_RH850_S1_CAN_FD_RX_Interrupt_With_Rule
Sample_Project_RH850_S1_CAN_FD_RX_Interrupt_With_Rule

update @ 2025/06/24

1. initial RH850 EVB - RH850/F1KM-S1 (BLDC) Starter Kit , to test below function 

- UART : RX:P10_9 , TX:P10_10

- CAN1 : RX:P10_7 (interrupt , with RX rule , to filter ID DATA) , TX:P10_6 (polling)

	- check : p->rrt_handle[q].mask.bit.MID=STANDARD_ID_BIT_IS_COMPARED;

```c

static void can_rrt_set(CAN_REG_TYP * can,
                          CAN_BUS_HANDLE *p,
                          const CAN_RX_RULE_TABLE_T *rule)
{
    unsigned int q=0;

    for(q=0;q<CAN_RX_RULE_TABLE_AMOUNT;q++)
    {
        //RCFDCnCFDGAFLIDj
        p->rrt_handle[q].id.bit.ID=q;
        p->rrt_handle[q].id.bit.LB=0;
        p->rrt_handle[q].id.bit.IDE=0;
        p->rrt_handle[q].id.bit.RTR=0;
        
        //RCFDCnCFDGAFLMj
        p->rrt_handle[q].mask.bit.MID=STANDARD_ID_BIT_IS_COMPARED;
        // p->rrt_handle[q].mask.bit.MID=EXTEND_ID_BIT_IS_COMPARED;//The corresponding ID bit is compared
        // p->rrt_handle[q].mask.bit.MID=ALL_ID_BIT_IS_NOT_COMPARED;


```

```c

#define CAN_USE_RX_RULE
// #define CAN_RX_POLLING
#define CAN_RX_INTERRUPT

```
2. Below is EVB switch

![image](https://github.com/released/Sample_Project_RH850_S1_CAN_FD_RX_Interrupt_With_Rule/blob/main/EVB_CAN_cfg.jpg)

3. Below is PCAN config setting 

![image](https://github.com/released/Sample_Project_RH850_S1_CAN_FD_RX_Interrupt_With_Rule/blob/main/PCAN_cfg.jpg)

4. Below is power on message

![image](https://github.com/released/Sample_Project_RH850_S1_CAN_FD_RX_Interrupt_With_Rule/blob/main/log_MCU_power_on.jpg)

5. when use UART terminal , which send CAN TX message from RH850 EVB , and rececive with PCAN


digit 1 , 
![image](https://github.com/released/Sample_Project_RH850_S1_CAN_FD_RX_Interrupt_With_Rule/blob/main/log_tx1.jpg)


digit 2 , 
![image](https://github.com/released/Sample_Project_RH850_S1_CAN_FD_RX_Interrupt_With_Rule/blob/main/log_tx2.jpg)


digit 3 , 
![image](https://github.com/released/Sample_Project_RH850_S1_CAN_FD_RX_Interrupt_With_Rule/blob/main/log_tx3.jpg)


digit 4 , 
![image](https://github.com/released/Sample_Project_RH850_S1_CAN_FD_RX_Interrupt_With_Rule/blob/main/log_tx4.jpg)


6. Below is different ID test condition , which send by PCAN , and rececive with RH850 RX

- RX rule , check : tbl_can_bus_rx_rule_ch1


- rule 0 , filter ID : 100 , accept Data Frame , Extended ID , compare RTR(Data frame or Remote frame), compare IDE(Standard ID or Extended ID)

![image](https://github.com/released/Sample_Project_RH850_S1_CAN_FD_RX_Interrupt_With_Rule/blob/main/rx_interrupt_rule_0_ID_100.jpg)


- rule 1 , filter ID : 101 , accept Data Frame , Standard ID , compare RTR(Data frame or Remote frame), compare IDE(Standard ID or Extended ID)

![image](https://github.com/released/Sample_Project_RH850_S1_CAN_FD_RX_Interrupt_With_Rule/blob/main/rx_interrupt_rule_1_ID_101.jpg)


- rule 2 , filter ID : 102 , accept Remote Frame , Extended ID , compare RTR(Data frame or Remote frame), compare IDE(Standard ID or Extended ID)

![image](https://github.com/released/Sample_Project_RH850_S1_CAN_FD_RX_Interrupt_With_Rule/blob/main/rx_interrupt_rule_2_ID_102.jpg)


- rule 3 , filter ID : 103 , accept Data Frame , Standard ID , NOT compare RTR(Data frame or Remote frame), compare IDE(Standard ID or Extended ID)

![image](https://github.com/released/Sample_Project_RH850_S1_CAN_FD_RX_Interrupt_With_Rule/blob/main/rx_interrupt_rule_3_ID_103.jpg)


- rule 4 , filter ID : 104 , accept Data Frame , Extended ID , compare RTR(Data frame or Remote frame), NOT compare IDE(Standard ID or Extended ID)

![image](https://github.com/released/Sample_Project_RH850_S1_CAN_FD_RX_Interrupt_With_Rule/blob/main/rx_interrupt_rule_4_ID_104.jpg)
