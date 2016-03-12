[Back to Index](DeprecatedDoc/CLAMUserManual "wikilink")

SDIF or Sound Description Interchange Format is a binary format defined and supported by various research teams. It was created with the goal of having a common format for exchanging synthesis samples, usually spectral domain data coming from a previous analysis.

The mapping of CLAM data to a SDIF File is fairly simple; it is always done from a CLAM::Segment (see section 51.6). The Segment internal structure can very easily be mapped to SDIF as it basically holds inside an array of time-ordered frames. Out of the different data inside a frame, only the necessary for the synthesis process is stored into SDIF. That is, residual spectrum, sinusoidal peaks with track number and fundamental frequency. Due to the SDIF specification, all magnitude data needs to be stored in linear (as opposed to what is usual in CLAM, where data is in dB).

All this is done using two CLAM Processing: SDIFIn and SDIFOut. SDIFIn takes a Segment in its output port because it needs a single reference where to store the created frames. SDIFOut takes frames in its output port and enables storing frames even from different segments.

As an example, this is the way the Analysis/Synthesis Example application handles the loading and storing of SDIF data.

Loading:

`SDIFInConfig cfg;`
`cfg.SetMaxNumPeaks(100);`
`cfg.SetFileName(inputFileName);`
`cfg.SetEnableResidual(true);`
`SDIFIn SDIFReader(cfg);`
`SDIFReader.Output.Attach(mSegment);`
`while(SDIFReader.Do()) {}`

and Storing:

`SDIFOutConfig cfg;`
`cfg.SetSamplingRate(mGlobalConfig.GetSamplingRate());`
`cfg.SetFileName(mGlobalConfig.GetOutputAnalysisFile());`
`cfg.SetEnableResidual(true);`
`SDIFOut SDIFWriter(cfg);`
`int nFrames=mSegment.GetnFrames();`
`for(i=0;i<nFrames;i++)`
`{`
`  SDIFWriter.Do(frames[i]);`
`}`

In order to add SDIF support to an application you might be developing, you also have to add to your project/meakefile the files included in the /src/Tools/SDIF. These files implement the necessary classes for mapping the sdif file content, namely the definition that the specification gives of the following concepts: Frame, Matrix, Stream and File.
