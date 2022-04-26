# WinBle

A set of classes for enumerating and interacting with Bluetooth low energy devices on the windows platform.

## Requirements

* Windows 10
* Devices must be paired and enumerated by the windows ble driver

## Example Code

### Read a Characteristic

``` cpp
// The characteristic must have IsReadable set to true or an exception will be thrown
BleGattCharacteristicValue value = characteristic->getValue();
```

### Write a Characteristic Value

``` cpp
// The characteristic must have IsSignedWritable, IsWritable, IsWritableWithoutResponse set to true or an exception will be thrown
UCHAR values[] = { 'H', 'I', '\r', '\n' };

characteristic->setValue(values, 4);
```

### Subscribe to a Characteristic Notification

``` cpp
// The callback for the characteristic notification
void HandleCallback(BleGattNotificationData& data)
{
    cout << "Received callback data: " << data.getDataSize() << endl;
}

// Enumerate characteristic descriptors
characteristic->enumerateBleDescriptors();

// Get the list of descriptors
list<BleGattDescriptor *> descriptors = characteristic->getBleDescriptors();

if (descriptors.size() > 0)
{
    // Get the characteristic descriptor
    BleGattDescriptor *descriptor = descriptors.front();

    // The callback function
    const std::function<void(BleGattNotificationData&)> callback = HandleCallback;

    // Enable notifications for the characteristic IsNotifiable or IsIndicatable must be true for the characteristic or an exception will be thrown
    characteristic->enableNotifications(callback);

    // Enable the subscription notification for the characteristic
    descriptor->setIsSubscribeToNotification();

    // Disable the notification
    characteristic->disableNotifications();

    // Clear the subscription notification
    descriptor->clearIsSubscribeToNotification();
}
```

## 编译

### Visual Studio

直接打开WinBle.sln即可。

### MSYS2 Mingw32/Mingw64

使用CMake编译,需要使用最新版的MSYS2。由于MSYS2的gcc暂时不支持BLE(截至编辑时)，故从Windows10 SDK中复制了相应的库及头文件。
