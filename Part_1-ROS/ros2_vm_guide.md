# ROS 2 Virtual Machine Setup Guide

This guide provides instructions to create a virtual machine with Ubuntu 24.04, ROS 2 Jazzy, Terminator, and VSCode. This virtual machine will enable you to follow the ROS 2 Crash Course on Windows, Linux or Apple machines.

## 1. Install Virtualization Software

The virtualization software is responsible for running the Ubuntu virtual machine on your computer (the host). The virtualization software will use part of your computer resources (memory, disk, processor etc.) to run Ubuntu as if it was running on an independent computer.

You can install any virtualization software compatible with the OVA standard. Two free options are:

* **VMware Workstation** - runs on Windows or Linux.
* **VirtualBox** - runs on Windows, macOS/Intel, macOS Apple silicon, Linux and Solaris.

> **Note:** If you want to run the Virtual Machine on a macOS computer, then you must install VirtualBox, which is and available at [https://www.virtualbox.org/](https://www.virtualbox.org/). The instructions below were prepared for VMWare Workstation Pro, but they can also be used for VirtuaBox - keep in mind that the menu options might be different than the ones indicated below.

### 1.1 Download the virtualization software

1. Go to [support.broadcom.com](https://support.broadcom.com/) and create or log in to a free Broadcom account.
2. Once logged in, go to the menu **Software → Enterprise Software → My Dashboard → My Downloads → Free Software Downloads**.
3. Search for **VMware Workstation Pro**, select version **26H1**, choose your operating system.
4. Accept the Terms and Conditions checkbox, then click the download icon.

### 1.2 Install

1. **Run the installer as Administrator**.
2. Accept defaults throughout. When asked about **Enhanced Keyboard Driver**, install it (useful for Linux guests).
3. **Skip** the license key screen — leave it blank and click **Continue**.
4. Finish the installation and reboot, if prompted.

> **Hyper-V note:** If you use Docker Desktop, WSL2, or Windows Sandbox, VMware 26H1 is compatible with the Windows Hypervisor Platform. No need to disable Hyper-V.

### 1.3 Import the VM (in case you have the .ova file)

If you already have a virtual machine OVA file, you can follow the instructions below to import and run it on your computer. If not, skip to Section 2.

> ⚠️ **Important!** OVA files are created for specific target hardware. This means that Apple Silicon mac (M series) users cannot import a VM built for x86 (Intel-compatible) processors, and vice-versa. If you do not have a file compatible with your hardware, you need to create your own virtual machine by following the instructions from Section 2 onwards.

1. Open VMware and click **File** → **Open** → select the `.ova` file.
2. Follow the import wizard (accept defaults).
3. Before starting the VM: go to **Settings** and configure it with the following parameters:
    * RAM: 4 GB minimum, 8 GB recommended (maximum half of your computer's RAM)
    * CPUs: 2-4 cores
    * Network: NAT (Bridged Adapter should also work, but many of my students had issues with it)
    * USB: Enable USB 3.0 controller → Network Adapter → set to Bridged
4. Start the VM and log in (the VM provided by Hanze has username `ros2`, password `ros2`).

If you are using **VirtualBox**, the procedure is similar:

1. **File** → **Import Appliance** → select the `.ova` file.
2. Configure the VM with the same settings listed above.
3. Start and log in.

The next steps need to be followed only if you want to create your own virtual machine (`.ova` file). If your VM is already running, you can go back to the [Main page](/readme.md).

## 2. Download Ubuntu 24.04 ISO

Each version of ROS 2 is designed for a specific version of Ubuntu (24.04 in case of ROS 2 Jazzy). Although ROS 2 can run in other operating systems, there are limitations and compatibility issues. To avoid such issues, we are going to install ROS 2 over Ubuntu, so we need to download it:

1. Go to [https://releases.ubuntu.com/24.04/](https://releases.ubuntu.com/24.04/).
2. Download **Ubuntu 24.04.x LTS** (the `.iso` file, ~5.8 GB).
  > **Apple silicon users:** go to [https://ubuntu.com/download/alternative-downloads](https://ubuntu.com/download/alternative-downloads) and get the **ARM 64-bit** version. Maybe you will need a torrent client, like qbittorent.
3. Save the ISO file somewhere you'll remember, e.g. `C:\Users\your_name\Downloads\ubuntu-24.04-desktop-amd64.iso`.

## 3. Create a Virtual Machine for ROS 2 Jazzy

This section guides you through creating a Ubuntu 24.04 virtual machine with ROS 2 Jazzy, VSCode, RViz2 (and optionally Webots). It assumes that you have VMWare Workstation, but it should also work with VirtualBox. All instructions were successfully tested on a Windows 11 machine with VMWare Workstation. 

Estimated total time: **2–3 hours** (most of it is waiting for downloads and installs).

**What you'll have at the end:**

* Ubuntu 24.04 LTS VM with VMware Tools
* ROS 2 Jazzy (desktop-full)
* VSCode with ROS + Python extensions
* (optional) Webots R2025a + webots_ros2 package
* (optional) Bridged networking pre-configured for robot connectivity
* An exportable `.ova` file for distribution

### 3.1 New VM Wizard

1. Open VMware Workstation.
2. Click **Create a New Virtual Machine**.
3. Choose **Typical (recommended)** → **Next**.
4. Select **Installer disc image file (iso)**, browse to your Ubuntu ISO → **Next**.
5. Fill in:
   * Full name: `Hanze Master SSE` (this is just an example: you can select anything - Not required by VirtualBox)
   * Username: `ros2` (keep it simple for this workshop)
   * Password: `ros2` (keep it simple for this workshop)
6. VM Name: `ROS2-Jazzy-Ubuntu2404`
7. Location: choose a drive with at least **50 GB free**.

### 3.2 Disk Size

* Set disk size to **50 GB** (ROS 2 + Webots + packages need ~25–30 GB; leave headroom).
* Select **Store virtual disk as a single file** (easier to move/copy) and click **Next**.

### 3.3 Customize Hardware

Click **Customize Hardware**:

* Memory: 4 GB minimum, 8 GB recommended (maximum half of your computer's RAM)
* Processors: 2-4 cores
* Network adapter: NAT (Bridged Adapter with _Replicate physical network_ should also work, but many of my students had issues with it)
* Display - 3D graphics: Enable Accelerate 3D graphics
* Display - Graphics memory: At least 1024 MB
* USB controller: USB 3.2

Click **Close**, then **Finish**.

### 3.4 Virtual Network Configuration

By default, VMware uses an "Automatic" bridging setting that guesses your active connection. Sometimes, you need to force it to use your actual Wi-Fi or Ethernet card to get internet connection.

1. At the top menu bar of VMware Workstation, click on _Edit_ -> _Virtual Network Editor..._

    > **Note:** If you see a button at the bottom right that says **Change Settings** with an administrator shield icon, click it to unlock full configurations.

2. In the top list, click on **VMnet0** (this is the default virtual switch used for Bridged mode).

3. Look at the bottom section under VMnet Information. Where it says _Bridged to:_, it will likely say _Automatic_. Click the dropdown menu and change it from _Automatic_ to your exact, active network adapter.

    * If you are on Wi-Fi, look for an option that says _Intel Wi-Fi_, _Wireless_, or _802.11_. On my computer those options were not available: it worked with _Microsoft Network Apadter Multiplexor Driver_.

    * If you are on a wired cable, look for _Realtek PCIe_, _Intel Ethernet_, or _Gigabit Network Connection_.

Click **Apply**, then **OK**.

### 3.5 Install Ubuntu

VMware's Easy Install will automate most of the Ubuntu setup. The VM will boot, install Ubuntu, and log in automatically. This takes **15–25 minutes**.

When the desktop appears:

* Skip the Ubuntu welcome wizard (or complete it quickly).
* Open a terminal: press `Ctrl+Alt+T`.

### 3.6 Install Open VMware Tools (or Guest Additions, for VirtualBox)

VMware Tools provides display scaling, clipboard sharing, and drag-and-drop. On Ubuntu 24.04 they install automatically via Easy Install, but verify:

```bash
systemctl status open-vm-tools
```

If not running:

```bash
sudo apt update && sudo apt install open-vm-tools open-vm-tools-desktop -y
sudo reboot
```

After reboot, the VM window should resize automatically when you drag the corner.

In VirtualBox, go to **Features** → **Shared Clipboard (Bidirectional, or Host to Guest)** to allow copy-pasting of the commands.

### 3.7 System Updates

```bash
sudo apt update && sudo apt upgrade -y
sudo reboot
```

### 3.8 Install Terminator

When running ROS, you often need to work with several terminal windows simultaneously. The regular `Terminal` that comes pre-installed with Ubuntu works fine, so you can skip this step if you prefer to work with it. But there are other termianl tools that help organizing the use of multiple terminals in one screen. The one I prefer is called `Terminator` (obviously).

To install `Terminator`, run:

```bash
sudo apt install terminator
```

Once installed, you can find it on the applications menu. I recommend pinning it to the side bar to make it easier: while it is running, right-click on the Terminator icon on the side bar and select "Pin to Dash".

Some useful keyboard commands are:

* `CTRL + Shift + O` --> splits the window horizontally and opens a new terminal
* `CTRL + Shift + E` --> splits the window vertically and opens a new terminal
* `CTRL + Shift + T` --> creates a new tab and opens a new terminal
* `CTRL + Shift + N` --> shifts focus to the next terminal

A list of all commands is available at [https://github.com/gnome-terminator/terminator](https://github.com/gnome-terminator/terminator).

### 3.9 Install ROS 2 Jazzy

First, we need to setup the locale variables:

```bash
sudo apt install locales -y
sudo locale-gen en_US en_US.UTF-8
sudo update-locale LC_ALL=en_US.UTF-8 LANG=en_US.UTF-8
export LANG=en_US.UTF-8
```

Now, add ROS 2 repository:

```bash
sudo apt install software-properties-common curl -y
sudo add-apt-repository universe
sudo curl -sSL https://raw.githubusercontent.com/ros/rosdistro/master/ros.key \
  -o /usr/share/keyrings/ros-archive-keyring.gpg
echo "deb [arch=$(dpkg --print-architecture) \
  signed-by=/usr/share/keyrings/ros-archive-keyring.gpg] \
  http://packages.ros.org/ros2/ubuntu \
  $(. /etc/os-release && echo $UBUNTU_CODENAME) main" | \
  sudo tee /etc/apt/sources.list.d/ros2.list > /dev/null
sudo apt update
```

Finally, install ROS 2 Jazzy Desktop Full. The following command installs ROS 2, RViz2, rqt, demo nodes, and all standard tools. It can take **10–20 minutes** depending on your internet and VM speed.

```bash
sudo apt install ros-jazzy-desktop-full -y
```

To be able to compile code for ROS, you need to install ROS development tools. Install the complete development suite:

```bash
sudo apt install ros-dev-tools python3-pip -y
```

Then, initialize and download the package dependency database:

```bash
sudo rosdep init
```

This will show a deprecation warning and might take a while. **Do not interrupt the process.**

When the cursor is available again, run the update command (which will also show a warning message and will take a lot longer to complete). **Do not interrupt the process.**

```bash
rosdep update
```

### 3.10 Configure and test ROS 2

For the ROS 2 commands to be found, you must source its environment in every new terminal window that you open. The commands below automate this process by including the source command in the `.bashrc` file (which is a script that runs every time a new session is open):

```bash
echo "source /opt/ros/jazzy/setup.bash" >> ~/.bashrc
```

Now, let's verify your installation by running a ROS 2 node. Open a new terminal window and run the command below:

```bash
ros2 run demo_nodes_cpp talker
```

If ROS 2 was installed successfully, you will see the message below being printed every second with an ever increasing number:

```bash
`[INFO] ... [talker]: Publishing: 'Hello World: 1'` 
```

Press `Ctrl+C` to stop.

You can also run a simple simulation by running TurtleSim. First, run the command below to open the simulator screen:

```bash
ros2 run turtlesim turtlesim_node
```

The command you just ran is executing the `turtlesim_node`. A _node_ is how programs are referred to in ROS. Leave the TurtleSim node running and open a new terminal (if you are using Terminator, you can do that with `CTRL + Shift + O`). In the new terminal, run:

```bash
ros2 run turtlesim turtle_teleop_key
```

The second terminal is running another node called `turtle_teleop_key`. Keep the focus on this terminal and use the arrows to control the simulated turtle!

Press `Ctrl+C` on both terminals to stop the execution of both nodes. 

### 3.11 Install VSCode

Now that you verified that ROS 2 was installed properly, let's install VSCode. You will use it later to program your own nodes.

```bash
sudo snap install code --classic
```

> **Apple silicon users**: snap command doesn't work for ARM 64 for VSCode. You need to get the package from [https://code.visualstudio.com/download](https://code.visualstudio.com/download) then run: `sudo apt install /path_to_file/vscode.deb`

Once installed, open VSCode from the Applications menu and install these extensions (search in the Extensions panel, `Ctrl+Shift+X`):

- **Robot Developer Extensions (RDE) for Visual Studio Code** (by Ranch Hand Robotics LLC) - will install ROS specific and related extensions to work with ROS (Python, C++, URDF etc.).
- **CMake** (by twxs) - to get syntax highlighting for `CMakeLists.txt` files.

### 3.12 _Optional_: Install Webots and webots_ros2

Webots is an open-source robotics simulator. Considering mobile robotics, Webots has similar features [[1]](https://ieeexplore.ieee.org/document/9386154) and is more computationally efficient than Gazebo [[2]](https://arxiv.org/pdf/2008.04627).

If you are interested in installing Webots on your VM, follow the instructions available at:
[https://docs.ros.org/en/jazzy/Tutorials/Advanced/Simulators/Webots/Installation-Ubuntu.html](https://docs.ros.org/en/jazzy/Tutorials/Advanced/Simulators/Webots/Installation-Ubuntu.html).

### 3.13 _Optional_: Configure Networking for Robot Connectivity

This section is relevant for connecting the VM to a physical robot - if that's not your case, you can skip it. 

To enable the communication between a physical robot and ROS 2 inside your virtual machine, you must configure its network as bridged (not NAT):

1. With the VM **powered off**, go to **VM → Settings → Network Adapter**.
2. Set it to **Bridged: Connected directly to the physical network**.
3. Click the **Configure Adapters** button and make sure your active Wi-Fi or Ethernet adapter is checked.
4. Power the VM back on.

Verify the VM has its own IP address:

```bash
ip addr show
```

The VM should show an IP address in the same subnet as your host machine (e.g., `192.168.1.x` or `192.168.2.x`), **not** `192.168.x.x` from VMware's NAT range. If you see `192.168.232.x`, networking is likely still on NAT - go back and fix the adapter setting.

Test ROS 2 multicast (DDS discovery):

```bash
ros2 multicast receive &
ros2 multicast send
```

You should see `Received from ...`. This confirms that DDS node discovery will work on the network.

## 4. Export the VM for Distribution

This is an optional step, in case you want to share this virtual machine.

### 4.1 Clean up disk space

```bash
sudo apt autoremove -y
sudo apt clean
```

### 4.2 Shut down the VM cleanly

```bash
sudo shutdown now
```

### 4.3 Export the VM as an OVA file

An `.ova` is a single portable archive that can be imported by VMware on any platform.

1. In VMware Workstation, make sure the VM is **powered off**.
2. Go to **File → Export to OVF…**
3. In the filename, type a name ending in `.ova` (e.g., `ROS2-Jazzy-Ubuntu2404.ova`).
4. Choose a destination with enough free space (~20–25 GB).
5. Click **Save** and wait — this takes 5–15 minutes.

You can **share the `.ova` file** via USB drive, Google Drive, or a file server.

---

## References

[1] J. Collins, S. Chand, A. Vanderkop and D. Howard, "A Review of Physics Simulators for Robotic Applications," in IEEE Access, vol. 9, pp. 51416-51431, 2021, doi: 10.1109/ACCESS.2021.3068769. – Available at: [https://ieeexplore.ieee.org/document/9386154](https://ieeexplore.ieee.org/document/9386154).

[2] A. Ayala, F. Cruz, D. Campos, R. Rubio, B. Fernandes and R. Dazeley, "A Comparison of Humanoid Robot Simulators: A Quantitative Approach," 2020 Joint IEEE 10th International Conference on Development and Learning and Epigenetic Robotics (ICDL-EpiRob), Valparaiso, Chile, 2020, pp. 1-6, doi: 10.1109/ICDL-EpiRob48136.2020.9278116.  - Available at: [https://arxiv.org/pdf/2008.04627](https://arxiv.org/pdf/2008.04627)

---

## Navigation menu

* Go to [Chapter 1](../Part_1-ROS/Chapter-1/readme.md)
* Go to the [Main page](../readme.md)