## FreeRTOS-Libraries-Integration-Tests

### Overview
This repository contains tests that verify an integration of FreeRTOS IoT libraries
running on a specific microcontroller-based development board for robustness and
compatibility with AWS’s published best practices for AWS IoT Core connectivity.
Refer to [FreeRTOS Porting Guide](https://docs.aws.amazon.com/freertos/latest/portingguide/afr-porting.html) for how to port FreeRTOS libraries and test your project.
The tests are used by [AWS IoT Device Tester](https://docs.aws.amazon.com/freertos/latest/userguide/device-tester-for-freertos-ug.html) (IDT)
as part of the [AWS Device Qualification for FreeRTOS](https://docs.aws.amazon.com/freertos/latest/qualificationguide/afr-qualification.html). The tests can also be run locally from your IDE to verify your project, but a test report from IDT is required for AWS Device Qualification.

### Tests

The following test groups are included in this repository:
1. Transport Interface Test validates the implementation of transport interface required by [coreMQTT](https://docs.aws.amazon.com/freertos/latest/portingguide/afr-porting-mqtt.html) and [coreHTTP](https://docs.aws.amazon.com/freertos/latest/portingguide/afr-porting-corehttp.html). The implementation can be plain text or TLS. See [Transport Interface Test](/src/transport_interface) for details.
2. PKCS11 Test validates the implementation of PKCS11 interface required by [corePKCS11](https://docs.aws.amazon.com/freertos/latest/portingguide/afr-porting-pkcs.html).See [PKCS11 Test](/src/pkcs11) for details.
3. OTA Test validates the implementation of Physical Abstract Layer for [Over-the-Air Updates](https://docs.aws.amazon.com/freertos/latest/portingguide/afr-porting-ota.html). See [OTA Test](/src/ota) for details.
4. MQTT Test validates the integration with coreMQTT library.


### Folder Structure
The folder inside the repository is organized as follows:
```
├── config_template
├── src
│   ├── common
|   |── mqtt
|   |── ota
|   |── pkcs11
│   └── transport_interface
└── tools
```
The root of the repository contains following top level folders:
1. `config_template` contains configuration header templates. The templates should be copied into the parent project.
If running the tests with IDT, there is no need to edit the
copied templates. If running the tests without IDT, users need to fill in configuration values in the copied templates.
2. `src` contains source code for the tests. Each test set is contained in a subfolder inside `src`.
Refer to ReadMe in each subfolder for details of the test group, test cases and how to run these tests.
3. `tools` contains utility tools for the tests, such as echo server for Transport Interface Test.

### Prerequisites
1. The tests are ran as an individual test task. You should have a working FreeRTOS project to add test task on.
2. Unity Test Framework is used to run the tests. See [Unity](https://github.com/ThrowTheSwitch/Unity) for integration guide.

### Getting Started
1. Take FreeRTOS-Libraries-Integration-Tests as a submodule in your project.
2. Copy config_template/test_execution_config_template.h and config_template/test_param_config_template.h to a project location in the build path, and rename them to test_execution_config.h and test_param_config.h.
3. Include relevant files into the build system. If using CMake, qualification_test.cmake and corresponding test cmake files in `src/` can be used to include relevant files.
4. Implement platform functions in [src/common/platform_function.h](https://github.com/FreeRTOS/FreeRTOS-Libraries-Integration-Tests/blob/main/src/common/platform_function.h).
5. Implement test specific configurations. This is usually in the form of implementing a parameter setup function, which should fill out the struct of parameters passed into the function. Please refer to the documentation of specific tests.
6. In your application, call `RunQualificationTest()` function to start qualification tests.

For running the tests locally using your IDE:
1. In test_param_config.h, fill out the parameters required by the test.
2. In test_execution_config.h, set `<TEST_NAME>_TEST_ENABLED` to 1.
3. Compile and run the test application in your development environment.

For running tests using AWS IoT Device Tester for Device Qualification: 
See [AWS Qualification Program for FreeRTOS](https://docs.aws.amazon.com/freertos/latest/qualificationguide/afr-qualification.html).

### Contribution
See [CONTRIBUTING](CONTRIBUTING.md) for more information.

### License
This library is distributed under the [MIT Open Source License](LICENSE).

