# About

Fork of [tado-assistant](https://github.com/BrainicHQ/tado-assistant), modified for **Tado X**.

The following modifications has been made:

- Supports **Tado X** instead of **Tado**
- Support for **new Tado authentication method**
- Revised  **State Monitoring**: It will be taken into account if geo tracking is enabled for any device.
  Therefore, geo tracking now not only be controlled by the respective script setting, but based on your device settings.
  Instead of tracking devices at home, its now checked for devices away for a more conservative state switching.
- Improved **logging**: Duplicated messages are not written to the log file. Logging is now on by default. 
  
Thanks to 
- [Brainic](https://github.com/BrainicHQ)  for the [original code](https://github.com/BrainicHQ/tado-assistant)
- [Vincent Cox](https://github.com/vincentcox) for his [code](https://github.com/vincentcox/tado-assistant) regarding the new authentication method


# 🏡 Tado X Assistant: Your User-Friendly, Free Tado Auto-Assist Alternative

Discover the ultimate free alternative to Tado's Auto-Assist with Tado Assistant! This innovative utility enhances your
Tado smart home experience by seamlessly integrating with the Tado API, offering advanced features like mobile
device-based home state monitoring, open window detection in various zones, and customizable settings for open window
duration. Now with added support for multiple accounts, it's ideal for those managing several Tado devices across
different locations. Tado Assistant provides an efficient and cost-effective way to automate and optimize your home
environment. It's designed to be user-friendly and accessible, requiring minimal dependencies, making it a perfect
choice for both technical and non-technical users.

## 🚀 Key Features - Free Tado Auto Assist

- **Multi-Account Support**: Manage multiple Tado accounts seamlessly, perfect for users with devices in different
  locations.
- **State Monitoring**: Tado Assistant vigilantly tracks your home's status (HOME or AWAY) in real-time, offering a free
  alternative to Tado's Auto-Assist feature.
- **Smart Adjustments**: Detects discrepancies, such as no devices at home but the state is set to HOME, and adjusts
  accordingly.
- **Open Window Detection**: Recognizes open windows in different zones and activates the appropriate mode.
- **Customizable Open Window Duration**: Set your preferred duration for the 'Open Window' detection feature, allowing
  for personalized energy-saving adjustments.

## ⚠️ **Disclaimer**

This project is an independent initiative and is not affiliated, endorsed, or sponsored by Tado GmbH. All trademarks and
logos mentioned are the property of their respective owners. Please use this software responsibly and at your own risk.

## 🛠 Prerequisites

- A Unix-based system (Linux distributions or macOS).
- `git` installed to clone the repository.
- Root or sudo privileges for the installation script.
- `curl` and `jq` (Don't worry, our installer will help you set these up if they're not present).
- Ensure both scripts (`install.sh` and `tado-assistant.sh`) reside in the same directory.

## 📥 Installation

1. Clone this repository to dive in:

   ```bash
   git clone https://github.com/christophschwarz/tado_x-assistant.git
   ```

   ```bash
   cd tado-assistant
   ```

2. Grant the installation script the necessary permissions:

   ```bash
   chmod +x install.sh
   ```

3. Kick off the installation with root or sudo privileges:

   ```bash 
   sudo ./install.sh
   ```

During the installation, the script will:

- Set up the required dependencies.
- Prompt you for your Tado credentials and other optional configurations.
- Initialize `tado-assistant.sh` as a background service.
- Introduce a new configuration option for the 'Open Window' feature. You will be prompted to enter the maximum
  duration (in seconds) that the system should wait before resuming normal operation after an open window is detected.
  You can specify a custom duration or leave it empty to use the default duration set in the Tado app.

## 🔄 Updating

To ensure you're running the latest version of Tado Assistant, follow these steps:

1. Navigate to the `tado-assistant` directory:

    ```bash
    cd path/to/tado-assistant
    ```

2. To update normally, run the installation script with the `--update` flag:

    ```bash
    sudo ./install.sh --update
    ```

   This will check for the latest version of the script, update any dependencies if necessary, and restart the service.

3. If you need to force an update (for instance, to revert local changes to the official version), use
   the `--force-update` flag:

    ```bash
    sudo ./install.sh --force-update
    ```

   This option will update Tado Assistant to the latest version from the repository, regardless of any local changes.
   It's useful for ensuring your script matches the official release.

### Note on Local Changes

- When updating, the script automatically detects and backs up any local modifications. These backups are stored as
  patch files, allowing you to restore your changes if needed.
- In case of conflicts during a normal update, the script will halt and prompt you to resolve these manually, ensuring
  your modifications are not unintentionally overwritten.

## 🔧 Configuration

The Tado Assistant can be configured to handle multiple Tado accounts, with each account having its own set of
environment variables:

- `NUM_ACCOUNTS`: Number of Tado accounts you wish to manage. This should be set to the total number of accounts.

For each account (replace 'n' with the account number, e.g., 1, 2, 3, ...):

- `TADO_ACCESS_TOKEN_n`: Will be generated when running sudo ./install.sh.
- `TADO_REFRESH_TOKEN_n`: Will be generated when running sudo ./install.sh.
- `CHECKING_INTERVAL_n`: Frequency (in seconds) for home state checks for the nth account. Default is every 15 seconds.
- `ENABLE_GEOFENCING_n`: Toggle geofencing check for the nth account. Values: `true` or `false`. Default is `true`.
- `ENABLE_LOG_n`: Toggle logging for the nth account. Values: `true` or `false`. Default is `true`.
- `LOG_FILE_n`: Destination for the log file for the nth account. Default is `/var/log/tado-assistant.log`.
- `MAX_OPEN_WINDOW_DURATION_n`: Define the maximum duration (in seconds) for the 'Open Window' detection feature to be
  active for the nth account. Leave this field empty to use the default duration set in the Tado app.

These variables are stored in `/etc/tado-assistant.env`. Feel free to tweak them directly if needed. Ensure to adjust
the variable suffix 'n' to match the corresponding account number.

## 🔄 Usage

After successfully installing the Tado Assistant, it will run silently in the background, ensuring your home's
environment is always optimal. Here's how you can interact with it:

1. **Checking Service Status**:
    - **Linux**:
   ```bash
   sudo systemctl status tado-assistant.service
    ``` 
    - **macOS**:
   ```bash
   launchctl list | grep com.user.tadoassistant
    ``` 

2. **Manual Adjustments**: If you ever need to make manual adjustments to your Tado settings, simply use the Tado app.
   Tado Assistant will recognize these changes and adapt accordingly.

3. **Logs**: To understand what Tado Assistant is doing behind the scenes, refer to the logs. If logging is enabled, you
   can tail the log file for real-time updates:
    ```bash
    tail -f /var/log/tado-assistant.log
    ```

4. **Adjusting 'Open Window' Duration**: The 'Open Window' detection feature's duration can be customized to suit your
   preferences. To modify this setting:
    - Edit the `/etc/tado-assistant.env` file.
    - Locate the `MAX_OPEN_WINDOW_DURATION` variable.
    - Set its value to the desired number of seconds. For example, `MAX_OPEN_WINDOW_DURATION=300` for a 5-minute
      duration.
    - Save the changes and restart the service for them to take effect.
        - For Linux:
          ```bash
          sudo systemctl restart tado-assistant.service
          ```
        - For macOS:
          ```bash
          launchctl unload ~/Library/LaunchAgents/com.user.tadoassistant.plist
          launchctl load ~/Library/LaunchAgents/com.user.tadoassistant.plist
          ```
   This setting defines how long the system should wait before resuming normal operation after an open window is
   detected, allowing for energy-saving adjustments tailored to your needs.

Remember, Tado Assistant is designed to be hands-off. Once set up, it should require minimal interaction, letting you
enjoy a comfortable home environment without any fuss.

## 📜 Logs

If you've enabled logging (`ENABLE_LOG=true`), you can peek into the log file (default
location: `/var/log/tado-assistant.log`) for real-time updates and messages.

## 🗑️ Uninstallation

Currently, a dedicated uninstallation script is not provided. To manually uninstall:

1. Stop the service.
    - For Linux:
   ```bash 
   sudo systemctl stop tado-assistant.service
    ```
    - For macOS:
   ```bash 
   launchctl unload ~/Library/LaunchAgents/com.user.tadoassistant.plist
    ```

2. Remove the service configuration.
    - For Linux:
   ```bash 
   sudo rm /etc/systemd/system/tado-assistant.service
    ```
    - For macOS:
   ```bash 
   rm ~/Library/LaunchAgents/com.user.tadoassistant.plist
    ```

3. Remove the main script:
   ```bash 
   sudo rm /usr/local/bin/tado-assistant.sh
    ```

4. Remove the environment variables file:
   ```bash
   sudo rm /etc/tado-assistant.env
    ```

5. Optionally, uninstall `curl` and `jq` if they were installed by the script and are no longer needed.
