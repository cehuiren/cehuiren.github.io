---
title: Simple example with recording code
date: 2007-03-08
categories: 编程开发 VBA-MicroStation
tags: 宏 录入 en
author: 网络转载
---
* content
{:toc}

Simple Example – recorded VBA macro

There are different development levels to create VBA code create or manipulate and remove data.

One method is to execute commands in MicroStation and record these steps as a macro and save this as VBA code. There are just a view steps required in order to get a recorded macro:

Go to the Utilities Tab of the Ribbon menu and select the “Record” button within the Macros Ribbon:

![](/img/2022/2022-10-29-14-56-02.png)

Please note: with a click on the “Record” button the button changes to “Pause” to let us know the steps from now are recorded.

After executing the desired steps in MicroStation a click on “Stop” ends the recording and a new dialog opens to ask for a project name to save the macro:

![](/img/2022/2022-10-29-14-56-09.png)

Please note: the Button in the lower left corner. This button expands the dialog and allows to select between “Bentley” and “VBA”. If the default “Bentley” is selected, the recording is saved as binary BMR format.

![](/img/2022/2022-10-29-14-56-17.png)

Please select “VBA”, browse to the desired location and press “Save”. This will save the project in a new MVBA project file. The name of the VBA project just used to save the recording is now also added into the Macros Ribbon, please note the entry “Bmrrecordtest”:

![](/img/2022/2022-10-29-14-56-27.png)

A click on the pencil symbol on the right of the name opens the VBA editor with the previous created VBA macro:

![](/img/2022/2022-10-29-14-56-37.png)

For this recording I used the “Place Smartline” keyin in MicroStation and in the recorded VBA code I can find this line:

CadInputQueue.SendKeyin " PLACE SMARTLINE"
This is how a MicroStation keyin is used in a VBA program to start a command. To learn more about the .SendKeyin method click on the word to position the cursor for context sensitive help and press the F1 key for help. This will open the MicroStation VBA help and show the help like this:

![](/img/2022/2022-10-29-14-56-46.png)

If you want to learn more about “CadInputQueue” do the same or search directly in the help:

![](/img/2022/2022-10-29-14-56-57.png)

Please note: Some keywords on the help provide link to code example, like here for CadInputQueue. If you click on “Example”, you may select from a larger number of examples:

![](/img/2022/2022-10-29-14-57-12.png)

If just select the first example to get this complete VBA code example:

![](/img/2022/2022-10-29-14-57-21.png)

Within the MicroStation VBA help there are a lot of code examples to demonstrate the use of the different methods. Please note: Make sure to use a MicroStation keyword before using F1 for sensitive help, otherwise the general VBA help opens, which helps with general VBA keyword, but does not contain help for MicroStation  classes.

The MicroStation VBA helpfile can also be opened outside the VBA Editor. The name of the help file is MicroStionVBA.chm and is stored in the same folder with MicroStation.exe.