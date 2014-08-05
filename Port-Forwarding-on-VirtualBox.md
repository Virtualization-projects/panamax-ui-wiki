If your application has a GUI, you need a port exposed on the CoreOS host that points to the port running the GUI on the container. For example, if your application runs a GUI on port 80, you need to bound a port from CoreOS to port 80 within Panamax. The following shows the host port, 8080, bound to the container port, 80.

![](http://panamax.ca.tier3.io/panamax_ui_wiki_screens/port_binding.png)

CoreOS can now access the web app via port 8080, however, in order to view the GUI itself, we need to make another hop from our local machine to the CoreOS port. To do this, we need to make a port forwarding rule in VirtualBox to port 8080.

This can be accomplished in two ways: 1) via the terminal windows of your local machine, or 2) via the VirtualBox GUI.

###Port Forwarding via the terminal window

On your local machine, open a terminal window and run the following command:

`$ VboxManage controlvm panamax-vm natpf1 rule1,tcp,,8997,,8080`

This command invokes the VirutalBox command VirtualManage to add a port forwarding rule to the VM named, `panamax-vm`. The rule created is named `rule1`, the protocol is set to `tcp` and the port forwarding is set to `8997` from the local machine to `8080` to match the port on the CoreOS host.

*NOTE*: The empty values between the ports is intentional. Be sure to add the `,,` around the local host port to ensure the mapping works correctly.

By browsing to `http://localhost:8997`, your browser is forwarded to port 8080 on the CoreOS host and then forwarded again to port 80 on the container to render the GUI. You can use any free port on your local machine to forward to the CoreOS host. 

###Port Forwarding via the VirtualBox GUI
The 2nd method to set port forwarding is vis the VirtualBox GUI. Follow these steps to enable port forwarding to your CoreOS host:

1. Open the VirtualBox GUI on your local machine. Should be available in your Application diretory.
2. Find the `panamax-vm` in the list of virtual machines and select Settings from the top nav
3. Click on the Network icon on the top nav

  ![](http://panamax.ca.tier3.io/panamax_ui_wiki_screens/virtualbox_network.png)

4. Select Port Forwarding 
5. Click the Add rule icon on the top right and fill in your Host port, 8997, and your Guest port, 8080.

  ![](http://panamax.ca.tier3.io/panamax_ui_wiki_screens/virtualbox_portforward.png)

6. Click OK and OK and the forward is set

By browsing to `http://localhost:8997`, your browser is forwarded to port 8080 on the CoreOS host and then forwarded again to port 80 on the container to render the GUI. You can use any free port on your local machine to forward to the CoreOS host.
