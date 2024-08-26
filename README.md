# ESP32-C6-Deep-Sleep-with-External-Wake-Up
```cpp
#if SOC_PM_SUPPORT_EXT0_WAKEUP
/**
 * @brief Enable wakeup using a pin
 *
 * This function uses external wakeup feature of RTC_IO peripheral.
 * It will work only if RTC peripherals are kept on during sleep.
 *
 * This feature can monitor any pin which is an RTC IO. Once the pin transitions
 * into the state given by level argument, the chip will be woken up.
 *
 * @note This function does not modify pin configuration. The pin is
 *       configured in esp_deep_sleep_start/esp_light_sleep_start,
 *       immediately before entering sleep mode.
 *
 * @note ESP32: ext0 wakeup source can not be used together with touch or ULP wakeup sources.
 *
 * @param gpio_num  GPIO number used as wakeup source. Only GPIOs with the RTC
 *        functionality can be used. For different SoCs, the related GPIOs are:
 *          - ESP32: 0, 2, 4, 12-15, 25-27, 32-39;
 *          - ESP32-S2: 0-21;
 *          - ESP32-S3: 0-21.
 * @param level  input level which will trigger wakeup (0=low, 1=high)
 * @return
 *      - ESP_OK on success
 *      - ESP_ERR_INVALID_ARG if the selected GPIO is not an RTC GPIO,
 *        or the mode is invalid
 *      - ESP_ERR_INVALID_STATE if wakeup triggers conflict
 */
esp_err_t esp_sleep_enable_ext0_wakeup(gpio_num_t gpio_num, int level);
#endif // SOC_PM_SUPPORT_EXT0_WAKEUP

#if SOC_PM_SUPPORT_EXT1_WAKEUP
/**
 * @brief Enable wakeup using multiple pins
 *
 * This function uses external wakeup feature of RTC controller.
 * It will work even if RTC peripherals are shut down during sleep.
 *
 * This feature can monitor any number of pins which are in RTC IOs.
 * Once selected pins go into the state given by level_mode argument,
 * the chip will be woken up.
 *
 * @note This function does not modify pin configuration. The pins are
 *       configured in esp_deep_sleep_start/esp_light_sleep_start,
 *       immediately before entering sleep mode.
 *
 * @note Internal pullups and pulldowns don't work when RTC peripherals are
 *       shut down. In this case, external resistors need to be added.
 *       Alternatively, RTC peripherals (and pullups/pulldowns) may be
 *       kept enabled using esp_sleep_pd_config function. If we turn off the
 *       `RTC_PERIPH domain or certain chips lack the RTC_PERIPH domain,
 *       we will use the HOLD feature to maintain the pull-up and pull-down on
 *       the pins during sleep. HOLD feature will be acted on the pin internally
 *       before the system entering sleep, and this can further reduce power consumption.
 *
 * @note Call this func will reset the previous ext1 configuration.
 *
 * @note This function will be deprecated in release/v6.0. Please switch to use esp_sleep_enable_ext1_wakeup_io and esp_sleep_disable_ext1_wakeup_io
 *
 * @param io_mask  Bit mask of GPIO numbers which will cause wakeup. Only GPIOs
 *                 which have RTC functionality can be used in this bit map.
 *                 For different SoCs, the related GPIOs are:
 *                    - ESP32: 0, 2, 4, 12-15, 25-27, 32-39
 *                    - ESP32-S2: 0-21
 *                    - ESP32-S3: 0-21
 *                    - ESP32-C6: 0-7
 *                    - ESP32-H2: 7-14
 * @param level_mode Select logic function used to determine wakeup condition:
 *                   When target chip is ESP32:
 *                      - ESP_EXT1_WAKEUP_ALL_LOW: wake up when all selected GPIOs are low
 *                      - ESP_EXT1_WAKEUP_ANY_HIGH: wake up when any of the selected GPIOs is high
 *                   When target chip is ESP32-S2, ESP32-S3, ESP32-C6 or ESP32-H2:
 *                      - ESP_EXT1_WAKEUP_ANY_LOW: wake up when any of the selected GPIOs is low
 *                      - ESP_EXT1_WAKEUP_ANY_HIGH: wake up when any of the selected GPIOs is high
 * @return
 *      - ESP_OK on success
 *      - ESP_ERR_INVALID_ARG if io_mask is zero,
 *        or mode is invalid
 */
esp_err_t esp_sleep_enable_ext1_wakeup(uint64_t io_mask, esp_sleep_ext1_wakeup_mode_t level_mode);
```
```
