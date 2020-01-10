Example program to use in fipy to measure temperature and humidity.

# DHT Pure Python library for Pycom board using new function pulses_get

This simple class can be used for reading temperature and humidity values from DHT11 and DTH22 sensors on Pycom Board. Thanks to szazo for the original source code. 

# Usage

1. Instantiate the `DHT` class with the pin number and type of sensor (0=DTH11, 1=DTH22) as constructor parameters.
2. Call `read()` method, which will return `DHTResult` object with actual values and error code. 
