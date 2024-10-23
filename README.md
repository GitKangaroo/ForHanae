# Photon Counting Algorithm
If you have successfully built the stack, use the following command to run the code in the stack folder:
```
./Panoptes/run gaudirun.py Panoptes/Rich/Panoptes/tests/qmtest/rich_photon_counting_2024_data.qmt
```
You can find my previous presentation about photon counting at LHCb week here:

https://indico.cern.ch/event/1415543/contributions/5978684/attachments/2869247/5023000/LHCbWeek_OnlinePhotonCounting.pdf

It goes through the basics of the photon counting algorithm and current results, and the important code skeleton is shown on page 5.

## rich_photon_counting_2024_data.qmt

The code runs on a QMT file (```rich_photon_counting_2024_data.qmt```).

In that QMT file, you can find the input data (```2024-data.py```),

the minimum track momentum selection (```MinMomentum30GeV.py```),

and the most important photon counting file (```RichPhotonCounting.py```).

## RichPhotonCounting.py & RichPhotonCounting.cpp
This ```RichPhotonCounting.py``` file is where you configure the algorithm. You can, for example, change the ```evt_max``` number, turn on/off the run-by-run histograms, use a quadratic background model (the default is a linear model), etc. In the first few import statements, you can see this: 
```
from PyConf.Algorithms import Rich__Future__Rec__Counting__PhotonCounting as PhotonCounting
```
Basically, the file ```RichPhotonCounting.cpp``` contains the actual algorithm for photon counting, and it (```Rich__Future__Rec__Counting__PhotonCounting```) is imported as ```PhotonCounting```.

The ```isQMTTest``` is set to False by default. What I usually do is change ```options.evt_max``` from -1 to a small number for a quick test.

## photoncounting.py
Furthermore, you need another configuration file called ```photoncounting.py``` to make HLT2 tracks, which is imported into ```RichPhotonCounting.py``` using the line from ```Panoptes.photoncounting import standalone_photon_counting```. 

In this track configuration file, you can see the track version, type, etc., but I don’t think you need to change anything there.

## Output
The output will be a ROOT file called ```RichPhotonCounting.root```, which contains all the run-by-run histograms defined and filled in ```RichPhotonCounting.py``` for both RICH1 and RICH2.

There is also a "summaries" folder that includes two txt files (```Rich1GasYieldRunByRunSummary.txt``` and ```Rich2GasYieldRunByRunSummary.txt```) for a summary of the photon yield and its error per run. This is used as the input file for online real-time monitoring.

You will also find a 2-page PDF plot of the photon counting histograms for each run (the mean value is the photon yield from the previous txt summary file) and a log file (```Task.log```).

You may not see these txt files if ```evt_max``` is too small (less than one run). Setting it to 100,000 would ensure that the txt files appear.

## Summary
In summary, you modify the algorithm in ```RichPhotonCounting.cpp``` and configure it in ```RichPhotonCounting.py```. 

Be sure to rebuild the package to update the library if you make changes to the algorithm itself in ```RichPhotonCounting.cpp```. 

However, if you only change the configuration in ```RichPhotonCounting.py```, there is no need to rebuild the package.

## File directory
Those files are located in different folders. The path of each file is listed below for convenience:
```
/Panoptes/Rich/Panoptes/options/RichPhotonCounting.py
/Panoptes/Rich/RichOnlineCalib/src/RichPhotonCounting.cpp
/Panoptes/Rich/Panoptes/tests/qmtest/rich_photon_counting_2024_data.qmt
/Panoptes/Rich/Panoptes/python/Panoptes/photoncounting.py
/Panoptes/Rich/Panoptes/tests/options/2024-data.py
/Panoptes/Rich/Panoptes/options/MinMomentum30GeV.py
```
