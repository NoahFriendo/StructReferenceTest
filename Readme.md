# Problem Description

## PLC:
- ST_StopStatus: PLC custom type containing 5 boolean variables.
- ReferenceBlock: PLC function block containing two variables:
	- stStopStatus: ST_StopStatus
	- refStopStatus: REFERENCE TO ST_StopStatus
	
	refStopStatus is set to reference stStopStatus

We have one instance of ReferenceBlock in MAIN, called fbRef

## HMI:

In our HMI Project, we map PLC1.MAIN.fbRef from the PLC.

We have two custom usercontrols:
- StopStatus.usercontrol: 5 checkboxes, mapped to the booleans from ST_StopStatus 
	- Parameter: ST_StopStatus, using PLC ST_StopStatus type
- StructAndReference.usercontrol: Contains 2 of StopStatus.usercontrol. The left one is mapped to Dev_ReferenceBlock::stStopStatus, the right one is mapped to Dev_ReferenceBlock::refStopStatus
	- Parameter: Dev_ReferenceBlock, using PLC ReferenceBlock type

On our Desktop.view, we have one StructAndReference.usercontrol. This is mapped to PLC1.MAIN.fbRef.
Below that, we have two of StopStatus.usercontrol, mapped directly to PLC1.MAIN.fbRef::stStopStatus and PLC1.MAIN.fbRef::refStopStatus.

The two controls on the bottom are able to access the PLC variables, including the "REFERENCE TO" variable.
When mapping the entire PLC1.MAIN.fbRef to the StructAndReference usercontrol instance, the stStopStatus mappings work as expected, but the refStopStatus variable is not mapped through.

Looking at the Live View debugger, it seems that TwinCAT HMI is not actually creating the symbol for refStopStatus when it is mapped through a usercontrol parameter.