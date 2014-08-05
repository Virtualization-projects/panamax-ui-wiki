If your application has a GUI, you need a port exposed on the CoreOS host that points to the port running the GUI on the container. For example, if your application runs a GUI on port 80, you need to bound a port from CoreOS to port 80 within Panamax. The following shows the host port, 8080, bound to the container port, 80.

Screenshot 1 - port binding

CoreOS can now access the web app via port 8080, however, in order to view the GUI itself, we need to make another hop from our local machine to the CoreOS port. To do this, we need to make a port forwarding rule in VirtualBox to port 8080.

This can be accomplished in two ways: 1) via the terminal windows of your local machine, or 2) via the VirtualBox GUI.

###Port Forwarding via the terminal window

On your local machine, open a terminal window and run the following command:

`$ VboxManage controlvm panamax-vm natpf1 rule1,tcp,,8997,,8080`

This command invokes the VirutalBox command VirtualManage to add a port forwarding rule to the VM named, `panamax-vm`. The rule created is named `rule1`, the protocol is set to `tcp` and the port forwarding is set to `8997` from the local machine to `8080` to match the port on the CoreOS host.

*NOTE*: The empty values between the ports is intentional. Be sure to add the `,,` around the local host port to ensure the mapping works correctly.

By browsing to `http://localhost:8997`, your browser is forwarded to port 8080 on the CoreOS host and then forwarded again to port 80 on the container to render the GUI. You can use any free port on your local machine to forward to the CoreOS host. The following illustrates the forwarding hops:

screenshot 2 - visio of the forwarding

