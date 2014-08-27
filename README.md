wfdb-scaling
============

Improving the scalability of the WFDB Toolbox for Matlab and Octave


Abstract
--------
The PhysioNet WFDB Toolbox for MATLAB and Octave is a collection of functions for reading, writing, and processing physiologic signals and time series used by PhysioBank databases. Using the WFDB Toolbox, researchers have access to over 50 PhysioBank databases consisting of over 3TB of physiologic signals. These signals include ECG, EEG, EMG, fetal ECG, PLETH (PPG), ABP, respiration, and others. 

The WFDB Toolbox provides support for local concurrency; however, it does not currently support distributed computing environments. Consequently, researchers are unable to utilize the additional resources afforded by popular distributed frameworks. The present work improves the scalability of the WFDB Toolbox by adding support for distributed environments. Further, it aims to supply several best-practice examples illustrating the gains that can be realized by researchers leveraging this form of concurrency.
