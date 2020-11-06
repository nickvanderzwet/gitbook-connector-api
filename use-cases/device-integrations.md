## Device integrations

In order to communicate with devices on the local hotel network, such as printers, lock systems, PBX, TVs, key cutters or fiscal machines, Mews has introduced the concept of devices and device commands. When a relevant action happens in Mews, a device command is generated and put into the device command queue. Using the API, you can pull the commands from the queue, process them as you find necessary and later mark them as processed. Of course, as many of the actions with devices should happen in real time, you should in most of the cases use this in combination with [Websockets](../websockets.md) to avoid polling for new commands. Whenever a relevant command is created, you would receive a notification about such an event through the opened websocket.

### Processing commands

First of all, you should subscribe your application for receiving device command events by connecting to the [Websockets](../websockets.md) endpoint. From then on, your application will receive all newly created device commands in real time. 

**Please note that you may also need to obtain all of the commands that are already pending in the queue while your application was offline. To do so, you should use the [Get all commands](../operations/integrations.md#get-all-commands) operation. You want to pull the pending commands every time your application reconnects using websockets, no matter how long it was offline to avoid leaving unprocessed commands in your queue.**

Once you are notified via a websocket about the fact that a device command was created or updated, you need to pull the command into your application using [Get all commands](../operations/integrations.md#get-all-commands). Once done, you should mark the command as processed using the [Update command](../operations/integrations.md#update-command) operation.

So, remember
1. Connect to the [Websockets](../websockets.md) to receive newly created device commands
2. Call [Get all commands](../operations/integrations.md#get-all-commands) with the ID
3. Once processed, mark the command as processed using the [Update command](../operations/integrations.md#update-command)


#### Fiscal machine commands

For Fiscal Machines \(no matter whether it is a virtual or a physical one\), you will receive a command containing [Fiscal machine command data](../operations/integrations.md#fiscal-machine-command-data). That includes all data of the related [Bill](../operations/finance.md#bill) including all the payments and revenue items in a form of [Accounting item](../operations/finance.md#accounting-item). 
*NB: Currently, there is no way to send fiscal codes generated by the fiscal machine back to Mews.*

#### Key cutter commands

For Key Cutters you will receive a command containing [Key cutter command data](../operations/integrations.md#key-cutter-command-data).