# Libreraies to include in Arduino I2CScanner
The I2CScanner library implements a scanner to locate I2C devices, and determine if a device is connected.ado.

More information https://www.luisllamas.es/libreria-arduino-i2cscanner/

## Instructions for use
The I2CScanner library is instantiated through its constructor. The object contains as variables the range of addresses for scanning 'Low_Address and 'High_Address. The object is initialized through the 'Init() method.

Three families of methods are available. The 'Scan' methods work like the traditional I2CScanner Sketch, displaying the results on the screen. The 'Check methods check for the existence of devices, but do not display any output. Finally, the Execute functions receive a Callback function as a parameter, and execute it only if the device is connected.

All three families of functions have overloads. If they don't receive any parameters, they act on the range defined by 'Low_Address and 'High_Address. If they receive an address, they act on it. If they receive a vector of directions they act on the same.

### Builder
```c++
I2CScanner();
```

### Using I2CScanner
```c++
	// Devices found in the last scanner
uint8_t Devices_Count
	
	// Range for the scanner without parameters
uint8_t Low_Address = 1;
uint8_t High_Address = 127;
	
	// initialization
	void Init();

	// Scanner functions, show result by serial
	bool Scan();
	bool Scan(byte address);
	bool Scan(byte addreses[], uint8_t length);

	// Check functions, do not show result by serial
	bool Check();
	bool Check(byte address);
	bool Check(byte addreses[], uint8_t length);

	//execute functions, execute the callback function if the device exists
	void Execute(I2C_Callback callback);
	void Execute(byte address, I2C_Callback callback);
	void Execute(byte addreses[], uint8_t length, I2C_Callback callback);
```

## examples
he I2CScanner library includes the following examples to illustrate its use.

* Scanner: Example that shows the use of I2CScanner showing results by Serial.
```c++
 #include "I2CScanner.h"

I2CScanner scanner;

void setup() 
{
	Serial.begin(9600);
	while (!Serial) {};

	scanner.Init();
}

void loop() 
{
	scanner.Scan();
	delay(5000);
}
```

* Check: Example that shows the use of I2CScanner, storing the check results in an array, which we would then use in the code.
```c++
#include "I2CScanner.h"

I2CScanner scanner;

const uint8_t num_addresses = 4;
const byte addresses[num_addresses] = { 0x20, 0x21, 0x40, 0x41 };
bool results[num_addresses] = { false, false, false, false};


void setup() 
{
	Serial.begin(9600);
	while (!Serial) {};

	scanner.Init();
}

void loop() 
{
	for (uint8_t index = 0; index < num_addresses; index++)
	{
		results[index] = scanner.Check(addresses[index]);
	}
	
	for (uint8_t index = 0; index < num_addresses; index++)
	{
		if (results[index])
		{
			Serial.print("Found device ");
			Serial.print(index);
			Serial.print(" at address ");
			Serial.println(addresses[index], HEX);
		}
	}
	delay(5000);
}
```

* Execute: Example showing the use of I2CScanner executing callback functions.
```c++
#include "I2CScanner.h"

I2CScanner scanner;

const byte address;

void debug(byte address)
{
	Serial.print("Found at 0x");
	Serial.println(address, HEX);
}

void setup() 
{
	Serial.begin(9600);
	while (!Serial) {};

	scanner.Init();
}

void loop() 
{
	scanner.Execute(debug);
	delay(5000);
}
```
