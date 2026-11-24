---
title: "36:1 Cycloidal macro sized actuator"
github: "https://github.com/Isla-Technologies/Macro-Actuator.git"
description: "A high-torque, open-source robotic actuator featuring a 36:1 cycloidal reduction drive paired with low-side inline current-sensing FOC control, using Eaglepower 8308 90KV BLDC and base motor"
created_at: "2026-11-01"
total_time: "46h 30m"
---

# November 1, 2026: Initial design using planetary gearbox
<!-- fabricate:entry 1777 -->

- **Reduction mechanism:** Selected initial 9:1 planetary gearbox reduction stage.
- **CAD Setup:** Generated gear teeth profiles in Fusion 360 using the GF Gear Generator plugin.
- **Parameters Defined:**
1. Gear module: 1mm
2. Sun gear tooth count: 12
3. Planet gear tooth count: 42
4. Ring gear tooth count: 96
- **Actuator dimensions:** 128mm * 128mm * 72mm

![](https://fabricate.hackclub-assets.com/7f106e03286f4a4712134be7b15840b69f87ceddaecf4a99f8a5b8211dcb0bf9/video-quicktime/gear.mov)
![](https://fabricate.hackclub-assets.com/e16ed23932c382bc2d78bf01e3be1d0baf0d203137160462c362e4f8ca8b19dd/image-png/Screenshot%202026-11-01%20at%203.54.53%E2%80%AFPM.png)

**Total time spent: 3h**

# November 1, 2026 (entry 2): Chose suitable FOC controller
<!-- fabricate:entry 1823 -->

* Starting with Odrive S1 [https://shop.odriverobotics.com/products/odrive-s1](https://shop.odriverobotics.com/products/odrive-s1) as first option for FOC control, however Odrive S1 is comparatively too expensive compared to other FOC controllers
* Changed to MKS XDrive Mini by Makerbase [MKS XDrive Mini](https://makerbase3d.com/product/makerbase-xdrive-mini-high-precision-brushless-servo-motor-controller-based-on-odrive3-6-with-as5047p-on-board/?srsltid=AYCX9vI95OXizozKxqpECL1vfgYh3g0SjMKBW2FSuFrmvEnDDSSdLHcK)
* Continued to model the motor enclosure with mounting holes for FOC controller, as well as encoder magnet

![](https://fabricate.hackclub-assets.com/c2fc4190bd5cfef3bea58be323fc988b4c69b57f643588f5853694ea8849cc47/image-png/Screenshot%202026-11-02%20at%201.01.39%E2%80%AFPM.png)
![](https://fabricate.hackclub-assets.com/55d524b9526db6c4927e41ecd61370e9b87542e77b52c932ef624a7c132dd6f6/image-png/Screenshot%202026-11-02%20at%201.02.29%E2%80%AFPM.png)

**Total time spent: 2h**

# November 2, 2026: Changed reduction mechanism due to insufficient torque
<!-- fabricate:entry 2060 -->

* This actuator is meant to be used for high torque joints such as hips and knees on a full-scale biped, a 9:1 reduction drive using Eaglepower 8308 90KV is simply not enough, 9:1 reduction only produces 16.36Nm of peak torque, according to what had been tested by Aaed Musa, [Aaed Musa Open QDD](https://www.aaedmusa.com/projects/openqdd)
* Did some research on reduction drive mechanisms, discovered options such as harmonic drive and cycloidal drive
* Chose cycloidal drive as final option because of better shock load, ease to manufacture compared to harmonic drive
* Setting 36:1 reduction ratio
* I used `cycloidaldrive` plugin from [Plugin](https://github.com/woodenCaliper/CycloidalDrive), and generated the cycloidal profile<br/>
* Specifications of my design are as followed:  
**Reduction ratio:** 36:1  
**Eccentricity:** 2mm  
**Ring pin pitch diameter:** 92mm  
**Ring pin diameter:** 5mm  
**Output pin number:** 8  
**Output pin diameter:** 16mm (I used 625ZZ bearing as output pin)  
**Cycloidal disc thickness:** 4mm<br/>
* I designed fully plastic ring pin housing and cycloidal discs
* Finished CAD assembly including motor enclosure and output section

![](https://fabricate.hackclub-assets.com/423ece3923255c176d54bfc8d3fa70fb273fdf72d0cb6a044fa36c6120a994ef/image-png/Screenshot%202026-11-03%20at%208.25.29%E2%80%AFAM.png)
![](https://fabricate.hackclub-assets.com/1e88aa76551b3bda9e8a7ea656117891c40e692f5f56fbc00d8d27d138e3c4d3/image-png/Screenshot%202026-11-03%20at%208.25.57%E2%80%AFAM.png)

**Total time spent: 6h**

# November 3, 2026: Prototyping
<!-- fabricate:entry 2118 -->

* 3D-printed individual components such as motor enclosure, ring pin housing, output block, etc. with PLA
* Bought bearings and fasteners (screws and inserts) online
* Used PLA to print cycloidal disc and ring pin housing to determine  printing tolerance
* Tolerance validated and proceeded to use PLA-CF to print cycloidal discs for higher stiffness
* **PLA-CF** is used only for prototyping as it provides higher stiffness than regular PLA, but quite abrasive against any surface

![](https://fabricate.hackclub-assets.com/6818852c612417469e171b689c78fbb2eb76b2a359ca1c7f41965279f743e188/image-jpeg/WhatsApp%20Image%202026-08-02%20at%203.26.31%20PM.jpeg)
![](https://fabricate.hackclub-assets.com/19f1361b48c81224683865e0bd8c8472d9d662229fb35f9dd461ba75b2d1930c/video-quicktime/motor%20bracket.mov)
![](https://fabricate.hackclub-assets.com/8b80d87987bdfe98bf293c7dc4fac95fbb19dd316aeee00cacea98ebffbaf90e/video-quicktime/pin%20housing.mov)

**Total time spent: 9h**

# November 5, 2026: Identifying and fixing root cause of efficiency loss
<!-- fabricate:entry 2189 -->

* Fully 3D-printed ring pins together with the housing is not rigid enough, and tolerance may vary time-to-time
* Calibrated encoder and motor using `odrivetool` in terminal
* *MKS XDrive Mini runs on Legacy version of Odrive firmware (v3.6), so Odrive official GUI is not supported
* Cycloidal discs were moving upwards vertically due to axial thrust
* I added 3D-printed retainer on the top of the eccentric shaft
* I also added bottom flanges on the cycloidal discs, overall thickness increased to 4.6mm for better stress handling
* I modified the ring pins housing into a bracket with slots for steel pins
* Ordered bearing steel pins online and adjusted slot tolerance, made sure the pins are held tightly and not loose at all
* Tip: Use a strong magnet to pull the pins out if the tolerance is a little bit too tight

![](https://fabricate.hackclub-assets.com/396396949e827ffe4e9f255d5e822a8dc77a8fc0a556ee8c0cffa1f3687854ab/image-png/Screenshot%202026-11-05%20at%204.52.32%E2%80%AFPM.png)
![](https://fabricate.hackclub-assets.com/ef20ffccbf19b807827327983428cfa3ccc98432d66c623dd380ea5bb2e6ce4d/image-jpeg/WhatsApp%20Image%202026-11-05%20at%204.56.34%20PM.jpeg)
![](https://fabricate.hackclub-assets.com/28af0bad090c66920c5ffdce94e4b5efb4e0d97b32754d843e4903344d49471b/video-mp4/one.mp4)
![](https://fabricate.hackclub-assets.com/bf9e4eb54d06b0c9fe17b4aa03734b9dd0a249014556e5f8f3123d7539cbc2d0/video-mp4/two.mp4)

**Total time spent: 6h**

# November 5, 2026 (entry 2): Full assembly and calibration
<!-- fabricate:entry 2224 -->

* Cloned and modified [ODrive3.6 Web GUI](https://github.com/MoonLighTingPY/odrive3.6_web_gui/releases/tag/titanium_2.0.0) by refering to the source code, allowing the tuning to be carried out easier
* For your information, the GUI featured in the video attached is a quick-test software, the official GUI software is shared via GitHub at [foc-actuator-bench](https://github.com/Isla-Technologies/foc-actuator-bench)
* Secured every component with bolts
* Added threaded inserts to the mounting holes

![](https://fabricate.hackclub-assets.com/eec5aade957561d458a252e1a69833e7cb3b09c969049b63be4750653d0cc3e3/video-mp4/foctest.mp4)

**Total time spent: 5h 30m**

# November 6, 2026: Bench test
<!-- fabricate:entry 2291 -->

* Printed a rig to hold the actuator in place
* Printed a 0.2m and a 0.1m lever arm, the files may be accesssed through [Google Drive Folder](https://drive.google.com/drive/folders/13UCqfWXMDXIuy7Pkf7KfhRHC6S6z7wQJ?usp=sharing)
* Tested actuator position gain, velocity and torque control
* Measured output torque and calculate overall actuator efficiency
* For my torque output measurement setup, I used a lever arm to press against a force meter at perpendicular angle, and use the formula `τ = Fd`
* I tested both lever arms with different length and obtained a similar result
* Measured no-load speed: **10.47 rad/s** (approximately 100 RPM)
* Measured output torque per amp ratio: **2.1Nm/A**
* Max continuous output torque (I'm using **Eaglepower 8308 90KV**, max continuous current is **22A**) will be `22 × 2.1 = 46.2Nm`  
Theoretically for perfect condition (100% efficient), the output torque per amp ratio should be **3.44Nm/A**, which means the current actuator design has a **61%** efficiency, root cause of efficiency loss is still being investigated

![](https://fabricate.hackclub-assets.com/51742c65591b1e45d31a84cb0c49fa801e58cae55423ddd2e797ceab9fb247fc/image-jpeg/torque%20test.jpeg)
![](https://fabricate.hackclub-assets.com/cfc71aca059b84df741e70352977accc8aa26f10e2f59baeb5ab8c1efca9493a/video-mp4/motion.mp4)

**Total time spent: 4h 30m**

# November 7, 2026: Choosing new material for cycloidal discs
<!-- fabricate:entry 2377 -->

* PLA-CF is too abrasive due to the chopped carbon fiber additives
* No grease was used, friction against the steel pins is very large
* Did research on CNC materials (POM, Aluminium 6061, 7075, 304 Stainless steel, 17-4PH, etc.)
* I chose Aluminium 7075 alloy for its most balanced characteristics
* I ordered custom CNC aluminium cycloidal discs, with ISO 2768-mK tolerance (I suggest IT5 or IT6 if affordable) and H7 tolerance for the center bore
* I also chose Mobilux EP0 as the grease
* Cycloidal gearbox produces immense stress on the ring pins and cycloidal discs, extreme pressure grease is recommended for this mechanism, lower NLGI grade also ensures the gearbox can operate more smoothly

![](https://fabricate.hackclub-assets.com/0eb56636ad051a7d139921da7ac54f9440e994a52c62075aba3f68587521faee/video-mp4/cnc.mp4)

**Total time spent: 3h 30m**

# November 8, 2026: Final assembly and bench test
<!-- fabricate:entry 2469 -->

* Assembled all iterated components, including CNC aluminium cycloidal discs
* ISO 2768-mK is a bit too generous, the tolerance was a bit off, so I modified the ring pitch diameter to accomodate the tolerance
* Applied grease to the gearbox
* Bench test with the new assembly
* Recorded test data  
**Gear ratio:** 36:1  
**Torque constant (output side):** 2.98Nm/A  
**No-load speed:** 10.47 rad/s (100 RPM)

![](https://fabricate.hackclub-assets.com/035ee3e87b5dbe5ea722555c1c60a377d5f9ef91efb9d9714657b0e7befedb43/image-jpeg/aluminium.jpeg)
![](https://fabricate.hackclub-assets.com/1a1652c28dd94a165702065e3ddeaa659408d286f9b9443efc489bbe556e62c0/video-mp4/bench%20test.mp4)

**Total time spent: 4h 30m**

# November 24, 2026: Compile and published on GitHub
<!-- fabricate:entry 3841 -->

* Compiled all performance data after calibration and bench tests
* *Will continue to publish any updates in the Github repo
* Recorded physical data:  
**Weight:** 1.11kg  
**Size:** 124mm * 124mm * 81.4mm
* Updated latest BOM
* Arranged all files and data, then uploaded to GitHub

![](https://fabricate.hackclub-assets.com/ad904a0ab8fdc637c81c75a399e81785154b4c6b0261154653460b4e443014e3/image-png/Screenshot%202026-11-24%20at%2010.49.23%E2%80%AFPM.png)
![](https://fabricate.hackclub-assets.com/dbbd22122ac5ebc9dadf59df84e3bb020fda14f601a292302cd4703aab67c52b/image-png/Screenshot%202026-11-24%20at%2010.49.47%E2%80%AFPM.png)

**Total time spent: 2h 30m**
