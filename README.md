# Pulse processing program based on the ROOT framework


- Writes a parser to read data that is compatible with Compass binary data output.
- Plots the pulse height distribution, pulse integral distribution and performs pulse shape discrimination (PSD).
- Implements the constant fraction discrimination (CFD) algorithm based on interpolation and plots the time difference histogram and its fit.

## Before runnning

- Make sure you have installed ROOT and your OS is linux. We use ROOT 6.14/04 and Ubuntu 16.04 LTS.
- Download the `input.txt` file and `main` file in `\bin` folder. Put them in the same directory.
- Create a folder named `output` in your current directory.

## Data
- Adjust the parameters in `input.txt`.
    - `Directory`, `Floders` and `Channels` together specify the file path;
    - Use `MaxNumPulses` to specify the numer of pulses you want to process. This can be set small when you want to take a look at the data but don't want to spend too much time on processing.
    - By default the program accepts data generated by CoMPASS, with the format shown in the figure below [1]. You need to change the `NSamples`, `DynamicRange`, `Resolution` and `Delt` based on the digitizer model.
    <!-- - ![CoMPASS data format](https://uofi.box.com/shared/static/bkirjoga6zxpk2qzyw7li9ih18lnp5ae.png) -->
    - <img src="https://uofi.box.com/shared/static/bkirjoga6zxpk2qzyw7li9ih18lnp5ae.png"  width="400">

- Save Headers.
    - The program allows to extract the timestamp, energy(ch), energy short from the header and save them in a ASCII file. Use `SaveHeaders` to do so.
- Save pulses.
    - Use `SavePulses` to save good pulses.
    - Use `Save bad` to save bad pulses.
- Plot pulses.
    - 10 pulses, the long gate and short gate will be plotted for debugging purposes.
    <!-- - ![Example pulses](https://uofi.box.com/shared/static/1oxvh3skhzwmnccj3wgvu9nhvrt1d8ra.png) -->
    - <img src="https://uofi.box.com/shared/static/1oxvh3skhzwmnccj3wgvu9nhvrt1d8ra.png"  width="400">

## Pulse integral distribution (PID) plot
- In `input.txt`, make sure `PID` is 1 if you want to plot the PID in the program.
    - Some options are available, such as integral range (`PreTrig`,`Pregate`, `LongGate`), plot range (`PImin`, `PImax`) and number of bins.
    - Use `CalibrationPID` to perform energy calibration.
    <!-- - ![PID](https://uofi.box.com/shared/static/2en70aap2l79unm1bem8536yt8nlewrp.png) -->
    - <img src="https://uofi.box.com/shared/static/2en70aap2l79unm1bem8536yt8nlewrp.png"  width="400">
- Use `SavePH` to save the pulse heights, `SavePHD` to save the PHD.
## Pulse height distribution (PHD) plot
- Similar to PID.
    <!-- - ![PHD](https://uofi.box.com/shared/static/maf7m8ltk1s749xn8l9uaw1hrd1egdwe.png) -->
    - <img src="https://uofi.box.com/shared/static/maf7m8ltk1s749xn8l9uaw1hrd1egdwe.png"  width="400">
## Pulse shape discrimination (PSD)
- Use `PreTrig`, `PreGate`, `ShortGate` and `LongGate` to set the gates. These gates are defined in the same way as in CoMPASS [1].
    <!-- - ![Explanation of the gates](https://uofi.box.com/shared/static/e7xruxshxei5b5kmv659zlfzjnb7mpqp.png) -->
    - <img src="https://uofi.box.com/shared/static/e7xruxshxei5b5kmv659zlfzjnb7mpqp.png"  width="400">
- Set `PSD` to `1` to plot PSD. 
    <!-- - ![PSD](https://uofi.box.com/shared/static/dlapqd8dpmrcopiyq5bw9ghwmafm64n9.png) -->
    - <img src="https://uofi.box.com/shared/static/dlapqd8dpmrcopiyq5bw9ghwmafm64n9.png"  width="400">
- Use `SaveIntegrals` to save the tail and total integrals.
- Use `SavePSD` to save PSD as a 2d hitogram in txt.

## Constant-fraction discrimination (CFD)
- Set `Time stamp` to `1` to enable CFD [2].
    - `TRise` is approximately 2* pulse rise time (ns).
    - `Interpolation` set the number of interpolation points to add between two adjacent samples.
    - `Time Delay` and `Fraction` are the CFD parameters.

## Pile-up rejection
- Set `Filter piled-up` to `1` to enable piled-up rejection.
    - `PUwindow` is approximately the width of the rising edge in unit of ns.
    - `PUfraction` is typically between 0 and 0.2. Samller fraction, more pulses will be rejected.
    - `PUthreshold` is the noise level.
    - Use `Save piled-up` to save the piled-up pulses.
## Run
- Sometimes the file is too lagre to be processed at one time (a few GB). In this case, you can split the file into several parts and process them seperately. Or you can use a small `MaxNumPulses`.
- Open a terminal, type `bin/main` to run.
- Use `Ctrl`+ `c` to quit.

## Use
If you use part of this software, please cite the following paper
Fang, M., Bartholomew, N., Di Fulvio, A. Positron annihilation lifetime spectroscopy using fast scintillators and digital electronics
(2019) Nuclear Instruments and Methods in Physics Research, Section A: Accelerators, Spectrometers, Detectors and Associated Equipment, 943, art. no. 162507.

## Reference
- [1]  CAEN S.pA. _CoMPASS Multiparametric DAQ Software for Physics Applications_. CAEN S.pA.
- [2] Fang, Ming, Nathan Bartholomew, and Angela Di Fulvio. "Positron annihilation lifetime spectroscopy using fast scintillators and digital electronics." _Nuclear Instruments and Methods in Physics Research Section A: Accelerators, Spectrometers, Detectors and Associated Equipment_ 943 (2019): 162507.
