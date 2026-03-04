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

	   
	
  - ### Signal Geometry
     - Average of all Dop values (hdop,pdop,gdop)


   
  - ### Antenna Status
      Depends on the following factors
    	- satellite ratio(Satellite used/ Visible)
    	- Signal quality (Avg Snr /Max(Snr_values))
    	- pdop value(Ideal range <= 2
       
    ​

## TIME & SYNCHRONIZATION

   - ### Active sources
	 
	 -  ### 🔹GPS 
		 - Time received via NMEA (serial data)
		 - Provides UTC timestamp
		 - Lower precision (milliseconds level)
	
	 -  ### 🔹PPS (Pulse Per Second)
		   - Hardware timing pulse from GPS module
		   - Extremely precise (microsecond level)
		   - Used to discipline the system clock



	  ### 1️⃣ Offset (ms) = Difference between system clock and reference source.	
	
	   | Offset    | Meaning         |
	   | --------- | --------------- |
	   | ±0.001 ms | Excellent       |
	   | ±0.01 ms  | Very Good       |
	   | ±0.1 ms   | Acceptable      |
	   | > ±1 ms   | Needs attention |
	
	   - Lower absolute value is better.



	 ### 2️⃣ Delay (ms) = Estimated network or communication delay to the source.

		- For:
		  - PPS → Near zero (local hardware)
		  - GPS (NMEA) → Small serial delay
		  - Network NTP → Network latency included
		
		Lower delay = better accuracy.

		| Source       | Normal Delay |
		| ------------ | ------------ |
		| PPS          | ~0 ms        |
		| GPS (serial) | 1–10 ms      |
		| Internet NTP | 10–100 ms    |




	 ### 3️⃣ Jitter (ms) = Variation in offset over time.

		| Jitter        | Quality   |
		| ------------- | --------- |
		| < 0.01 ms     | Excellent |
		| 0.01 – 0.1 ms | Good      |
		| 0.1 – 1 ms    | Moderate  |
		| > 1 ms        | Unstable  |

		#### For PPS-based systems : Jitter should be extremely low.




	 ### 4️⃣ Reach =
		- Indicates whether the source is reachable and responding.
	    - Displayed as an octal number (0–377).
		- It represents the success of the last 8 polling attempts.

		
			 | Reach | Meaning                             |
			 | ----- | ----------------------------------- |
			 | 377   | Fully reachable (last 8 successful) |
			 | 0     | Not reachable                       |
			 | < 100 | Intermittent communication          |



 
 
  - ### NTP Server Tracking Summary

    -  ### 🔹Reference Name
		 - PPS: Indicates the current primary time source used to discipline the system clock.


	-  ### 🔹Stratum
		- 1 : Indicates the NTP hierarchy level.
		- Stratum 1 means : Your server is directly synchronized to GPS (via PPS).
	
		  | Stratum | Meaning                         |
		  | ------- | ------------------------------- |
		  | 0       | Atomic clock / GPS hardware     |
		  | 1       | Directly connected to Stratum 0 |
		  | 2       | Sync from Stratum 1             |
		  | 3+      | Further downstream              |
​​
   
   -  ### 🔹 Ref Time Epoch
		- The last time the system clock was updated from the reference source.
		- High precision timestamp


   -  ### 🔹 System Time Offset
		- Difference between system clock and reference source (in seconds).


   -  ### 🔹 RMS Offset
		- Root Mean Square offset. Represents average deviation over time.
		- Lower RMS = more stable clock.

   -  ### 🔹 Frequency PPM
		- Clock frequency correction in Parts Per Million.
		- Indicates how much the system oscillator is being adjusted.
			- Higher value = larger correction needed
			- Happens due to crystal drift
		- For embedded devices (like Raspberry Pi), some drift is normal.
		- Chrony continuously corrects this.

   -  ### 🔹 Skew
		 - Estimated error margin of the frequency correction.
		 - Lower skew = higher confidence.
		 - Small skew is good.

   -  ### 🔹 Root Delay (ms)
		 - Total network delay to the ultimate reference clock.
		 - For PPS:
				- Should be near zero
				- 1 nanosecond is extremely small
				- Indicates local hardware reference

   - ### 🔹 Root Dispersion (ms)
		- Maximum estimated error relative to the primary reference.
		- Represents accumulated uncertainty.
		- Lower is better.

   - ### 🔹 Last Offset (ms)
	   - Most recent offset measurement.
	   - 0.000019 sec = 19 microseconds
	   - Very small — healthy system.

   - ### 🔹 Update Interval (s)
		- Polling interval between synchronization updates.
		- Smaller interval = faster correction
		- Longer interval = stable system


  -  ### 🔹 Leap Status

	   | Status | Meaning                     |
	   | ------ | --------------------------- |
	   | Normal | No leap second pending      |
	   | Insert | Leap second will be added   |
	   | Delete | Leap second will be removed |





## SATELLITES

  ### 1️⃣ Fix Mode :
   - Indicates the type of position solution currently computed by the GPS receiver.

   - It shows whether the receiver has enough satellites to calculate location and time.

		| Fix Mode   | Meaning                        | Satellites Required |
		| ---------- | ------------------------------ | ------------------- |
		| No Fix (1) | No valid solution              | < 3                 |
		| 2D Fix (2) | Latitude & Longitude only      | ≥ 3                 |
		| 3D Fix (3) | Latitude, Longitude & Altitude | ≥ 4                 |

  ### 2️⃣ HDOP (Horizontal Dilution of Precision):
   - Measures accuracy of horizontal position (latitude & longitude).

   - Lower value = better satellite geometry.

   - HDOP Ranges
	
		| HDOP   | Quality   |
		| ------ | --------- |
		| 0 – 1  | Ideal     |
		| 1 – 2  | Excellent |
		| 2 – 5  | Good      |
		| 5 – 10 | Moderate  |
		| > 10   | Poor      |


  ### 3️⃣ PDOP (Position Dilution of Precision):
   - Measures overall 3D position accuracy (horizontal + vertical).
     
   - Lower is better.

   - PDOP Ranges

		| PDOP  | Quality   |
		| ----- | --------- |
		| 0 – 2 | Excellent |
		| 2 – 5 | Good      |
		| 5 – 8 | Moderate  |
		| > 8   | Poor      |


 ### 4️⃣ GDOP (Geometric Dilution of Precision):
  - Measures overall solution quality including:
	- Position
	- Altitude
	- Time
   
  - GDOP includes timing accuracy impact.
    
  - It is the most complete DOP value.

  - GDOP ranges 

	| GDOP   | Quality   |
	| ------ | --------- |
	| 0 – 2  | Excellent |
	| 2 – 5  | Good      |
	| 5 – 10 | Moderate  |
	| > 10   | Poor      |





 ### 🛰 Satellite Parameters Explanation


   ### 1️⃣ PRN : Pseudo Random Noise number

   - It is the unique ID number assigned to a satellite.
 
   - Each satellite transmits a unique PRN code so the receiver can identify it.

		📌 Example
		
		- PRN 1
		
		- PRN 12
		
		- PRN 25


   ### 2️⃣ Constellation :

   - Indicates which satellite system the satellite belongs to.

   - Modern receivers support multiple GNSS systems.

   - Multi-constellation improves:

      - Accuracy

      - Satellite availability

      - Lower DOP


  ### 3️⃣ Used in Fix : Indicates whether this satellite is actively used in the position/time calculation.


  ### 4️⃣ SNR (dB) :

   - SNR = Signal-to-Noise Ratio

   - Measures signal strength quality from that satellite.

   - Unit: dB-Hz

   - Higher = stronger and cleaner signal.

   - SNR Quality Table

		| SNR (dB-Hz) | Signal Quality |
		| ----------- | -------------- |
		| > 40        | Excellent      |
		| 35 – 40     | Very Strong    |
		| 25 – 35     | Moderate       |
		| 20 – 25     | Weak           |
		| < 20        | Very Weak      |



  ### 5️⃣ Elevation (°) : 
  - Angle of the satellite above the horizon.
  - Range : 0° (horizon) → 90° (directly overhead)


	| Elevation | Meaning               |
	| --------- | --------------------- |
	| 0°–10°    | Very low, weak signal |
	| 10°–30°   | Moderate              |
	| 30°–60°   | Good                  |
	| 60°–90°   | Excellent             |



### 6️⃣ Azimuth (°):
 - Compass direction of the satellite.

 - Range:  0°– 360°

 -    | Azimuth | Direction |
	  | ------- | --------- |
	  | 0°      | North     |
	  | 90°     | East      |
	  | 180°    | South     |
	  | 270°    | West      |
	
 - Azimuth helps:

	- Visualize sky distribution

	- Detect antenna blockage in certain directions


  
