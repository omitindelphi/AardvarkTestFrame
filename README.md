********# AardvarkTestFrame
Delphi test framework for automated or scripted  unit or integration test of legacy GUI VCL applications. 
Can work together with DUnit or DunitX on all versions of Delphi, or completely stand-alone.

## Introduction
Quite often in my career I was tasked with supporting legacy VCL application, with new-time set of requirements involving automated [unit] test
as part of corporative continious integration process.

Problem stem from the fact that legacy Delphi applications, more often than not, come from prototypes oriented on user actions and 
generally do not have non-visual objects easily permitting independent testing, dependency injecting and other modern approaches.
It is common case you must have VCL TForm object be shown on real screen to get access to application functionality.
(it is not necessary, and you can build applications of modern architecture with Delphi VCL - but it should be decided early in the project)

There are Delphi-oriented testing tools for bridging the gap, most known are products of SmartBear; but they are not cheap 
and not so often available for rank-and-file developers.

There are generic open-source testing tools available, most known, for example, are AutoIt, but it is hard to get match between Delphi names and generic Window.
There is stand-alone completely visual Open Source tool- Sikuli - but it is unpleasant to be so closely tied to screen settings.

proposed solution is aimed to be light-weight, open source, easily managed by developer herself (or himself) testing tool reproducing user actions
(keyboared strokes and mouse clicks), depending of application state, what exactly is displayed to user.

## How it works
Any GUI application can be run as debugged by some external application - test runner, for example - as long as we have access to Window Handles of its internal objects and can 
detect their visibility and screen position. 
**AardvarkTestFrame** presents an inside agent, communicating Window handles and screen positions of Delphi visual components to test runner, 
making it easy to direct mouse clicks and keyboard imput to debugged forms.

That inside agent is simple **TForm** object, which do not have references to any other units of applications, not need to be shown (only created at run-time), 
do not catch any events, do not have any hooks, just process its own timer (in main thread), and each 3 sec (by default), renew single database record 
describing content of topmost form or message (well, and other forms, too, if necessary) so test runner may have full picture and direct testing actions.

That's it. Test runner and debugged app communicate only via that database record. 
Come out it is most simple and reliable way of communications 
with best user experience outcome, even if it is not so fast with tracing updates; 
[Attempts to use application as TCP or UDP server were not so successful - servers consume too many resources]

## Integration with legacy project
it is for developer to decide. Reference to agent form may be guarded by DEBUG conditionals in the body of .dpr file, or be activated by command line option.

## Integration with DUnit and DunitX
Generally, DUnit[X] test object creates counterpart of inside agent, aware of state of debugged VCL application uder test, detect visibility of the forms, established selections, and direct imitation of mouse clicks and key strokes to debugged application.

##Comminication between Test Runner and testee.

By default, inside agent report match between Delphi WinControl name and Window handle, ComboBox items, ComboBox ItemIndex, PageControl captions and their screen positions, Grid contents, SpeedButton positions



