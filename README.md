![Terraria Logo](/images/terraria_logo.png)

# Hosting a dedicated Terraria server with static routing and port forwarding

### Overview

This guide will walk you through setting up a dedicated Terraria server that can be connected to by other people. This will be achieved by setting a static IP for your router, forwarding a few ports, and using Terraria's server setup wizard.

> **NOTE**: The instructions and screenshots in this readme are specific to the software/hardware listed in [specs](#specs). Different routers/operating systems will have different interfaces and terminology. However, the concepts herein apply universally, and can be translated to fit your set up.

### Table of contents

- [Specs](#specs)
- [Step 1: Gather Required Information](#step-1-gather-required-information)
- [Step 2: Static Routes](#step-2-static-routes)
    - [Why](#why)
    - [How](#how)
- [Step 3: Port Forwarding](#step-3-port-forwarding)
    - [Why](#why)
    - [How](#how)
- [Step 4: Start A Terraria Server](#step-4-start-a-terraria-server)
- [Step 5: Share With Your Friends](#step-5-share-with-your-friends)

---

### Specs
* **Operating System**: Windows 10 (64 bit)
* **Router**: Netgear R6120 Nighthawk
* **Steam Client Package Version**: `1591251555` (Built: Jun 3, 2020)
* **Terraria Version**: `1.4.0.5`

---

### Step 1: Gather Required Information

In order to set up a static route and port forward, you will need four key items
- IPv4 Address
- Subnet Mask
- Default Gateway
- Public IP Address

`IPv4 Address`, `Subnet Mask`, and `Default Gateway` can be obtained using the Windows Command Prompt:

1. Click the Windows Icon <img src="/images/windows_10_icon.png" width="25"/> on your desktop, or push the button on your keyboard

2. Type `cmd` and hit `Enter` to open up the Command Prompt Application
    - Alternatively, you can find the application in

    `C:\Users\<your_username>\AppData\Roaming\Microsoft\Windows\Start Menu\Programs\System Tools\Command Prompt`

3. Type `ipconfig` in the Command Prompt and hit `Enter`

  ![ipconfig](/images/win10_command_prompt_ipconfig.PNG)
<br/>

`Public IP Address` can be obtained using whatsmyip.com

1. Open a browser, navigate to `whatsmyip.com`, and take note of `My Public IPv4`

  ![Public IP Address](/images/whatsmyip_public_ip.PNG)

> **NOTE**: Be careful with whom you share your public IP address with, as it can be used to determine your physical location, as well as open you up to hacking attempts

<br/>

---

### Step 2: Static Routes

##### Why

In general, your computer is set by default to use dynamic routing. This means that when you boot up your computer, your router will assign it an available IP to identify it in the local network. Each time you start your computer, it will be assigned a ***new*** IP. While this is useful, it makes it difficult for friends to connect to your server if the IP they need to connect to keeps changing.

Setting up a static route ensures your computer always uses the same IP address, meaning your friends can always connect to your server with the same IP each time **(be aware of the dangers of sharing a public IP address!)**.

##### How

1. Open up your router's UI by opening up a browser and entering the `Default Gateway` number you obtained in [Step 1](#step-1:-gather-required-information) into the address bar (Commonly, the default gateway will be `192.168.1.1`, but your router IP may vary and can be found written on the router).
    - You will most likely be asked to enter a username and password. If unsure what that is, it can usually be found written on the router itself.

    ![Netgear R6120 Home Screen](/images/netgear_r6120_router_home_screen.PNG)

2. Click the `Advanced` tab, select `Advanced Setup`, select `Static Routes`, and click the `Add` button.

  ![Netgear R6120 Static Routes Page](/images/netgear_r6120_router_static_routes_page.PNG)

3. Fill in the form fields with the information you gathered from [Step 1](#step-1:-gather-required-information)
    - `Route Name` is the name you want to give the route, for example `My desktop`
    - `Destination IP Address` is the `IPv4 Address` from step 1
    - `IP Subnet Mask` is the `Subnet Mask` from step 1
    - `Gateway IP Address` is the `Default Gateway` from step 1
    - `Metric` defaults to 2. You can leave it as is.

  ![Netgear R6120 Completed Static Route](/images/netgear_r6120_router_complete_static_route.PNG)

4. Click `Apply` to complete the static route setup.

---

### Step 3: Port Forwarding

#### Why

In computing, a port is an address where an application or process lives on a computer. In Terraria's case, the developers have set the default port for the game to run on port `7777`. If your router wants to "talk" to Terraria, it knows where it lives based on the IP address of the computer Terraria is running on, and the port where it's running.

In the previous step, we set a static route for our computer, which might look like `192.168.1.100`. Now when you start Terraria, it will be on port `7777`, and your router knows the full address, which looks like `192.168.1.100:7777`.

This is great, but your friends still don't have access to your computer; they can't connect to that `192.168.1.100` internal IP address (unless they are actually on your wifi or ethernet). You still need to broadcast your computer and Terraria's location on it to the public.

***Port forwarding*** allows remote computers (computers on the Internet [your friends]) to connect to a specific computer or service within a private local-area network. Setting a port to forward to in your router allows it to direct other computers to the application/process running on the defined port. In this case, you will forward your friend's computers to port `7777` where the Terraria server will be running.

<br/>

#### How

1. In your router's UI (see [here]() for router connection instructions), click the `Advanced` tab, select `Advanced Setup` in the left navigation, select `Port Forwarding / Port Triggering`, and click the `Add Custom Service` button.

  ![Netgear R6120 Port Forwarding Page](/images/netgear_r6120_router_port_forward_page.PNG)

2. Fill in the form fields:
    - `Service Name` describes what application is being forwarded to, for example `Terraria Server`
    - `Service Type` should be `TCP/UDP`
    - `External port range` is the ports you want to tell your router to forward to. If you plan on only hosting one Terraria server, you can fill this in with just `7777`. If you plan on hosting multiple servers, you can specify a range of ports, e.g. `7777-7779`. This means that you can have one world running on port `7777`, a second world on port `7778`, and a third world on port `7779`.

    > **NOTE**: It's completely fine to speicfy a wide range of ports, but the more servers you end up running, the more computer resources you will be using, and the more likely you are to experience issues like lag and computer slowness.

    - `Internal port range` will fill in automatically based on what you put in the `External port range` field.
    - `Internal IP address` is the `IPv4 Address` from step 1

  ![Netgear R6120 Completed Port Forwarding](/images/netgear_r6120_router_complete_port_forward.PNG)

3. Click `Apply` to complete the port forwarding setup.

---

### Step 4: Start A Terraria Server

Now that you've created a static route and set up port forwarding, you can start up a Terraria server using Terraria's server setup wizard.

1. Locate the Terraria server setup wizard in `C:\Program Files (x86)\Steam\steamapps\common\Terraria` and double click to open `TerrariaServer` application.
    - For easier access in the future, right click `TerrariaServer` and either click:
      - `Create shortcut` to create an application shortcut on your desktop
      - `Pin to taskbar` to create an application shortcut on your taskbar
      - `Pin to Start` to create an application shortcut in your Start menu

      ![Terraria Directory](/images/terraria_directory.PNG)

2. Terraria Server will open a Command Prompt and list your available worlds
    - You can choose to start an existing world, create a new world, or delete a world
    - Just type the corresponding number or letter and press enter to move to the next step

  ![Terraria Server](/images/terraria_server.PNG)

3. Decide the maximum number of players you would like to allow to join the server at a time
    - Defaults to 16 if not specified

  ![Terraria Max Players](/images/terraria_server_select_max_players.PNG)

4. Select a server port to start your server on
    - Defaults to `7777` if not specified
    - Make sure to provide one of the ports you set up in the `External port range` from [Step 3](#step-3:-port-forwarding)

  ![Terraria Server Port](/images/terraria_server_port.PNG)

5. The next step asks whether to automatically port forward
    - Select **no** by typing `n` and pressing `Enter`

  ![Terraria Auto Port Forward](/images/terraria_server_auto_port_forward.PNG)

6. The final step is to set a password for your server
    - Leaving it blank means there is no password to enter the server
    - If you enter a password, be sure to let your friends know so they can connect

  ![Terraria Server Password](/images/terraria_server_password.PNG)

7. After setting (or not setting) a password, the server will start up

  ![Terraria Server Started](/images/terraria_server_started.PNG)

8. There are some further options provided by the server after it has been started. Just type help to see all the available commands

  ![Terraria Server Help Commands](/images/terraria_server_help_commands.PNG)

---

### Step 5: Share With Your Friends

1. Have them start Terraria through their Steam client.
2. Select `Multiplayer`
3. Select `Join via IP`
4. Select player
5. In `Enter IP Address:`, put the `Public IP Address` which you obtained in [Step 1](#step-1:-gather-required-information)
6. Click `Accept`
7. In `Enter Server Port:`, put the port you used to start your server in [Step 4](#step-4:-start-a-terraria-server)
8. Click `Accept`
9. Start collecting ducks to your heart's content! <img src="/images/mallard.png" width="30"/>

![Look At All Those Chickens](/images/look_at_all_those_chickens.gif)
