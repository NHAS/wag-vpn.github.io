+++
weight = 1
+++

# Installation

1. Download the latest release of wag:
    ```sh
    curl -L https://github.com/NHAS/wag/releases/latest/download/wag -o wag && chmod +x wag
    ```

2. Enable the wag host to act as a router:
    ```sh
    sudo sysctl -w net.ipv4.ip_forward=1
    ```

3. Generate a configuration file for wag, and start
    ```sh
    sudo ./wag gen-config
    sudo ./wag start -config <generated_config>
    ```


This will start the wag server attached to the console. 