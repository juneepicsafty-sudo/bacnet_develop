# bacnet_develop

This document provides information on how to build the BACnet stack for various microcontrollers and operating systems.

## Building for ESP32

This section contains build instructions for different ESP32 projects within this repository.

### Project: `ports/esp32_mstp`

Follow these steps to build the MS/TP example for the ESP32.

1.  **Open the ESP-IDF Command Prompt.**

2.  **Navigate to the project directory:**
    ```
    cd <bacnet-src-dir>\ports\esp32_mstp
    ```

3.  **Set the target MCU and open the configuration menu:**
    ```
    idf.py set-target esp32
    idf.py menuconfig
    ```

4.  **Apply required manual code changes.**

    > **Note:** These changes may be necessary due to API updates in newer versions of the ESP-IDF.

    - In `rs485.c`, replace `portTICK_RATE_MS` with `portTICK_PERIOD_MS`.
    - In `led.c` and `mstimer-init.c`, replace `gpio_pad_select_gpio` with `esp_rom_gpio_pad_select_gpio`.

5.  **Build the project:**
    ```
    idf.py build
    ```

After a successful build, you can flash the firmware to your ESP32 device.

### Project: `ports/esp32`

Follow these steps to build the BACnet/IP example for the ESP32.

1.  **Open the ESP-IDF Command Prompt.**

2.  **Navigate to the project directory:**
    ```
    cd <bacnet-src-dir>\ports\esp32
    ```

3.  **Set the target MCU and open the configuration menu:**
    ```
    idf.py set-target esp32
    idf.py menuconfig
    ```

4.  **Apply temporary workarounds.**

    > **Note:** The following are temporary solutions to resolve compilation issues.

    - Create a new file `datalink/dlbip.c` with empty stub functions.
    - In `bip_init.c`, comment out network-related function calls that are causing errors.
    - In `main.c`, comment out any problematic network-related code.

5.  **Update the build sources.**

    - Add the following source files to your build system's source list (e.g., in `CMakeLists.txt`):
      - `bacnet/datalink/dlbip.c`
      - `bacnet/bacdevobjpropref.c`
      - `bacnet/cov.c`
      - `bacnet/memcopy.c`

6.  **Build the project:**
    ```
    idf.py build
    ```

After a successful build, you are ready to flash the firmware to your device.
