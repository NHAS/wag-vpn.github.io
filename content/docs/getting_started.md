+++
weight = 1
+++

# Installation

1. Check requirements
   ```
   Glibc 2.31+
   Linux Kernel 5.3+
   ```

2. Download the latest release of wag:
    ```sh
    curl -L https://github.com/NHAS/wag/releases/latest/download/wag -o wag && chmod +x wag
    ```

3. Enable the wag host to act as a router:
    ```sh
    sudo sysctl -w net.ipv4.ip_forward=1
    ```

4. Generate a configuration file for wag, and start
    ```sh
    sudo ./wag gen-config
    sudo ./wag start -config <generated_config>
    ```


This will start the wag server attached to the console. 