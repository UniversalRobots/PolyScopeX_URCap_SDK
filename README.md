# PolyScope X No Install SDK (Codespace)

This repository contains the PolyScope X SDK and URSim artefacts and is accessible via [GitHub Codespaces](https://docs.github.com/en/codespaces/overview).

## Intention of this repository

This repository is designed to enable users to quickly and easily develop and test URCaps for PolyScope X.
All versions of the SDK and URSim available here can also be found on the primary release site in the PolyScope X Beta
Project.

The release provided here is offered "AS IS", if you would like to be more involved and provide feedback,
please sign up for the [PolyScope X Beta Project](https://ur.centercode.com/EarlyAccessTesterPSX).
You can also connect with other developers by joining
the [Universal Robots Discord server](https://discord.com/invite/sEjRgEf6fp).

**Note**
You can install URCaps from local storage via the system manager in URSim.
However, due to codespace transfer restrictions, there is a limit to the size of URCaps that can be installed this way.
When building and installing URCaps from codespace there are no size restrictions. For more details see "Build and
Deploy URCap" section below.

## Working with samples

It is highly recommended to treat the samples folder as read-only, because rebuilding the codespace container will
remove and recreate this folder.
To preserve changes to samples, simply copy the modified URCap sample to a location outside the samples folder and push
it to your git repository.

## Start Codespace from repository

**NB:** You need to have a (free) GitHub user-account and be logged in, to be able to use Codespaces

1. Open the `Code` drop down, and select the codespaces tab. You can then choose to create a codespace on master (or any
   other repository). ![New Codespace](images/newCodespace.png)

2. A new tab will open, and the codespace will start loading up. You will see a web version of VSCode. It can take a
   couple of minutes to set up a remote connection, and build the codespace. You will know that the workspace has been
   built successfully when you see the `postCreateCommand` and `postStartCommand` are executed, as shown
   below. ![Container Start](images/containerStart.png)

After you do this, you can access this cloud workspace [here](https://github.com/codespaces). Any changes to your space
become a fork from this repository.

## URSim & Port Forwarding

After startup has finished, you can start the simulator by running the command `./run-simulator --port <PORT_NUMBER>`. 
If you don't specify a port, it will default to port 80.

You must manually forward the port being used in order to successfully view the application. This is also how you will 
access the URSim GUI.

1. Open the PORTS tab next to your terminal. ![Forward Port Landing](images/portForward.png)

2. Enter the chosen port into the port number field in the 'Port' column and press enter. 
![Forward Port Landing](images/portForward2.png)

3. You can now open the dynamically generated link (in the 'Forwarded Address' column) to view and use URSim.

**Note:** Codespace has a default timeout of 30 minutes, after which you'll need to forward the ports again. You can
read more about changing the
timeout [here](https://docs.github.com/en/codespaces/setting-your-user-preferences/setting-your-timeout-period-for-github-codespaces).

## URCap SDK

To use the SDK, open a new terminal.

![New Terminal](images/newTerminal.png)

The contents of the SDK are:

- urcap-generator/ - Contains the URCap generator for creating URCap contributions.
- samples/ - Contains URCap samples. Build using: npm install && npm run build
- newurcap.sh - The script for creating a URCap contribution in your current working directory using the generator
- run-simulator - The script for running URSim. This is a linked file. The full URSim directory can be found at the `/`
  level.

### New URCap

You can now create a new URCap by running the newURCap.sh shell script.

Once a new URCap contribution has been created using the script, a folder named after its ID is created.

### Build and Deploy URCap

To create the final zip file for a contribution, go to its folder and run the following commands:

1. Install the node modules.

   `npm install`

2. Generate the URCap file in the target directory.

   `npm run build`

3. Deploy to URSim (ensure that URSim is running and open).

   `npm run install-urcap`

You should now see the URCap in URSim. For more information, refer to
the [PolyScope X SDK Official Documentation](https://docs.universal-robots.com)

## Stopping or deleting a codespace instance

It is possible to close down the codespace instance manually, by pressing the code drop down button, then on the right
side pressing the three dots to open a menu to either select the 

- `Stop codespace`, which stops all running processes, and allows for starting up the same codespace again. 
- `Delete`, which completely removes codespace the instance.

![Closing codespace](images/closingCodespace.png)

There are more information on
the official page about [lifecycle of codespaces](https://docs.github.com/en/codespaces/getting-started/understanding-the-codespace-lifecycle)
which also describes the above in more detail, along with reentering and restarting a codespace.