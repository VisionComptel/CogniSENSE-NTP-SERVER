# CogniSENSE-NTP-SERVER

## DASHBOARD

   - ### Local Time
      Local Time comes from the Raspberry Pi system clock.
   
   - ### Discipline Source 
     PPS-Pulse Per Second
   
     - A hardware signal from a GPS module
     - Sends one precise electrical pulse every second
     - Extremely accurate (microsecond or even nanosecond precision)
   
   - ### GPS Receiver Status
     - ### Satellites-
        - ≥ 6 = Good
        - 4-5 = Moderate
        - ≤ 3 = Poor

   - ### Average SNR (Signal-to-Noise Ratio)
     - Average SNR is the mean signal strength of all satellites currently used in the fix.
     - It indicates overall signal quality and antenna reception performance.
     - Higher value = better signal quality.
       
        | SNR (dB-Hz) | Quality     |
        | ----------- | ----------- |
        | > 40        | Excellent   |
        | 35 – 40     | Very Strong |
        | 25 – 35     | Moderate    |
        | 20 – 25     | Weak        |
        | < 20        | Very Poor   |
	
	
  - ### Signal Geometry
     - Average of all Dop values (hdop,pdop,gdop)
   
  - ### Antenna Status
      Depends on the following factors
    	- satellite ratio(Satellite used/ Visible)
    	- Signal quality (Avg Snr /Max(Snr_values))
    	- pdop value(Ideal range <= 2
       
    ​

## Time & Synchronization 
   - ### Active sources 
	   ####🔹GPS
		 - Time received via NMEA (serial data)
		 - Provides UTC timestamp
		 - Lower precision (milliseconds level)
	
	   🔹#### PPS (Pulse Per Second)
		   - Hardware timing pulse from GPS module
		   - Extremely precise (microsecond level)
		   - Used to discipline the system clock

   1️⃣### Offset (ms) - Difference between system clock and reference source.	

   | Offset    | Meaning         |
   | --------- | --------------- |
   | ±0.001 ms | Excellent       |
   | ±0.01 ms  | Very Good       |
   | ±0.1 ms   | Acceptable      |
   | > ±1 ms   | Needs attention |

   - Lower absolute value is better.

​

​​
   
   
