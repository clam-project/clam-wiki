The application analyzes, transforms and synthesizes back a given sound. For doing so, it uses the Sinusoidal plus Residual model (sometimes referred to as SMS but also known as HILN in the context of MPEG4).

For SMS analysis and synthesis block diagrams, see [1](http://xavier.amatriain.net/Thesis/html/node125.html#fig:SMS-Tools-block-diagram)

For more information on SMS, see [2](http://mtg.upf.edu/node/251)

If you need more information about the signal processing details involved in the process you may want to visit the [ SMS](http://mtg.upf.edu/technologies/sms) webpage.

Introduction
------------

The application has three different versions: SMSTools - which has a FLTK graphical user interface-, SMSConsole- which is a command-line based version-, and SMSBatch - which can be used for batch processing a whole directory. All you have to do to compile one or the other is to compile the file SMSTools.cxx (if you want GUI), SMS.cxx (if you want the Command Line versrion) or SMSBatch (if you want to batch process). It is strongly recommended that you start with the graphical SMSTools version and only use the others only if you have more specific requirements. The rest of this document will suppose that you are using the graphical version and only mention some differences with the other versions where strictly necessary.

The application has a number of possible different inputs:

1.  A **configuration xml file**. This configuration file is used to configure both the analysis and synthesis processes and may include the path to the **input sound file** to be analyzed.
2.  An **analysis xml file**. This file will be the result of a previously performed and stored analysis. The xml parser (xerces) features include a rather slow parsing/storing of documents. On the other hand, xml is a text-based format and thus you can expect a 1 MB sound file become 10 MB of xml analysis data. For all those reasons the storing/loading of analysis data, although fully working, is not recommended unless you want to have a textual/readable representation of your analysis result, else you will be better off using the SDIF format (see next paragraph). Or An **analysis SDIF file**. This file will be the result of a previously performed and stored analysis. This format is as complete as the XML passivation, but it is dramatically more compact (i.e. smaller files), since data is not stored in a "human readable" format . See XII for more details about SDIF file format.
3.  A **Transformation score in xml format**. This file includes a list of all transformations that might be applied to the result of the analysis and the configuration for each of the transformations.

Note that all of them can be selected and generated on run-time from the user interface if you use the SMSTools version.

From these inputs, the application is able to generate the following outputs:

1.  **Analysis data** xml or sdif file.
2.  Output sound: **global** sound, **sinusoidal** component, **residual** component

The following picture illustrates the main blocks of the application:

![](http://www.clam.iua.upf.edu/doc/ReleaseDocumentation-html/images/image044.gif "http://www.clam.iua.upf.edu/doc/ReleaseDocumentation-html/images/image044.gif")

Building the application
------------------------

At this point you might be able to checkout the latest SMSTools binary version from the [CLAM webpage](http://www.clam.iua.upf.edu). Building the application might still be necessary in some cases but that will be handled [elsewhere](Manual_CompilingApps).

An SMSTools walkthrough
-----------------------

Once you have built the Application you may follow the steps below to both get a first

impression of the application capabilities and check that everything is right:

1.  First you have to configure the workspace. For doing so you have two options: (1) Load an xml configuration file (like the one located in <clam sources dir>/build/Examples/SMS) or (2) directly edit the default configuration. Let's consider this second option. Go to File-\>Configuration-\>Edit. There are many fields you can edit and modify (see Section on Configuration File) but for the sake of simplycity, let us just select an input sound file (.wav, .aiff or .raw) by clicking on the button next to the InputSoundFile field. Your interface should look something like:
2.  If the sound file name you entered is correct, we are now in the position to continue and analyze the sound. Click on SMSAnalysis-\>Analyze and wait for the progress bar to finish.
3.  Once the sound is analyzed, we are ready to look at the analysis results. In the View menu you will see active all the available data: original sound, sinusoidal tracks, fundamental frequency, and frame related data (sinusoidal spectrum and peaks and residual spectrum). When you select a frame related view, you can browse through the different frames by clicking on any of the non-frame views or by using the frame browser at the bottom of the interface. You can also listen to the sound by clicking on the play button on the bottom-right of the wave display. Note that the sliders on any view act like a zoom control when clicking on any of the extremes.
4.  Apart from displaying, the result of the analysis can be stored for later uses. It can be stored in xml format or sdif format. Xml is a textual tagged format that can be convenient for debugging and studying results of some analysis but it is very verbose and slow (you should expect an xml file 4 times the size of the original audio). On the other hand SDIF is a binary format that for most uses will be much more convenient. For storing the result of the analysis, choose File-\>SMS Analysis-\>Store Analysis File. The application will switch from SDIF format to XML depending on the file extension you choose.
    Note that the result of an analysis can be later loaded selecting File-\>SMS Analysis-\>Load Analysis Data.
5.  Before synthesizing, we can transform the sound. For doing so, just as it happened with the configuration, we can load an xml transformation score or edit the default one. We will do the latter. Choose File-\>SMSTransformation-\>Edit Score. You will see a list of available transformations. Clicking on any of them will open a brief description of the transformation and its usage:
    1.  Select any transformation, click on Add Transformation to Score and edit the Parameters on the right. In the break point function editor, duble click to add a point and click to select a point or draw a selection box. Repeat this step for any number of transformations you want to connect one after the other.
    2.  Click on Apply Changes to Score. Now we can transform the sound by selecting SMSTransformation-\>Apply.

6.  We can now synthesize the resulting sound. Select SMSSynthesis-\>Synthesize. Now visit the View menu again and select the output component you want to display and listen to: Output Sound, Output Sinusoidal and Output Residual.
7.  Finally you can store the results of the synthesis by choosing File-\>SMS Synthesis-\>Save...

Analysis Output
---------------

So, the output of the analysis is a CLAM::Segment that contains an ordered list of CLAM::Frames. Each of these frames has a bunch of attributes, but the most important are: a CLAM::SpectralPeakArray that models the sinusoidal component (including information about sinusoidal track), a residual spectrum and the result of the pitch estimation (or rather fundamental detection).

[i2] The output of this analysis can be (1) stored in xml format, (2) stored in sdif binary format, (3) transformed and (4) synthesized back.

The xml format is rather verbose so the process of storing the result of the analysis in xml can be time-consuming and can result in a large file (you can expect an expansion factor of about 10:1 comparing it to the original sound file). On the other hand, it can be easily read and can be used for debugging purposes. This is a partial example of what your xml file will look like:

<Analyzed_Segment>

`   `<BeginTime>`0`</BeginTime>
`   `<EndTime>`0.766258`</EndTime>
`   `<prHoldsData>`1`</prHoldsData>
`   `<FramesArray>

`       `<Frame>

`           `<CenterTime>`0.00290249`</CenterTime>
`           `<Duration>`0.00580499`</Duration>
`           `<SpectralPeakArray>

`               `<Scale>`Log`</Scale>
`               `<nPeaks>`2`</nPeaks>
`               `<nMaxPeaks>`50`</nMaxPeaks>
`               `<MagBuffer>`-30.733 -9.25596e+061`</MagBuffer>
`               `<FreqBuffer>`343.559 714.887`</FreqBuffer>
`               `<PhaseBuffer>`1.5182 -0.0999764`</PhaseBuffer>
`               `<BinPosBuffer>`4.00429 8.33224`</BinPosBuffer>
`               `<BinWidthBuffer>`5 6`</BinWidthBuffer>
`               `<IndexArray>`0 1`</IndexArray>
`               `<IsIndexUpToDate>`0`</IsIndexUpToDate>

`           `</SpectralPeakArray>
`           `<Fundamental>

`               `<nMaxCandidates>`1`</nMaxCandidates>
`               `<nCandidates>`1`</nCandidates>
`               `<CandidatesFreq>`343.559`</CandidatesFreq>
`               `<CandidatesErr>`-1`</CandidatesErr>` `

`           `</Fundamental>
`           `<ResidualSpec>

`               `<Scale>`Linear`</Scale>
`               `<SpectralRange>`22050`</SpectralRange>
`               `<prSize>`257`</prSize>
`               `<MagBuffer>`0.00729711 0.00896791 0.0119354 0.0189819 0.000319389 0.0183698 0.00916167 0.0161299 0.0269896 0.0244382 0.0129489 0.00634384 0.00610172 0.00875066 0.00763111 0.00742003 0.00559243 0.000849196 0.00317517 0.00519037 0.00528536 0.00224068 0.00154297 0.00469072 0.00733866 0.00677861 0.00294284 0.000809419 0.00223092 0.00265733 0.00162793 0.000550603 0.000858541 0.00125168 0.00068071 0.000105767 0.00117828 0.00236095 0.00198881 0.00090993 0.000564566 0.00125519 0.00127909 0.00127066 0.00120286 0.00124451 0.00119085 0.000941523 0.000348921 0.000462266 0.000758022 0.000818371 0.000709333 0.000946096 0.000769061 0.000493843 0.0004842 0.000491863 0.000382826 0.000379278 0.000367886 0.000472623 0.000445563 0.000435359 0.000409167 0.000435712 0.000289131 0.000230931 0.00029841 0.000469285 0.000420821 0.000506603 0.000552621 0.000943916 0.000950009 0.000439976 0.000162634 0.000268673 6.66906e-005 0.000417807 0.000419127 0.000346079 0.000245963 0.000310977 0.000329851 0.000336672 0.000337432 0.000148063 0.000154738 0.000438211 0.000522947 0.000425414 0.000180047 0.000117523 0.000177102 0.000208794 0.000144707 7.62237e-005 9.28201e-005 0.000287346 0.000310371 0.000266661 0.000149645 0.00021754 0.00030985 0.000314559 0.000192857 3.32129e-005 0.000167097 0.000250963 0.000203181 0.000134031 0.000102491 0.000243036 0.000254449 0.000275495 0.000234674 0.000215736 0.000161505 0.000161531 0.000172264 0.00023892 0.000160434 0.000127822 0.000190916 0.000242277 0.000186513 0.000215995 0.000218083 0.000244236 0.000200369 0.000238023 0.000236179 0.000275808 0.000191796 0.000101178 9.95931e-005 0.00016034 0.000175327 0.00027087 0.000222175 0.000191879 0.000384598 0.000499003 0.000365438 0.000181958 0.000113119 0.000311989 0.000328654 0.000263493 0.000163289 0.000115733 4.07026e-005 0.000189727 0.000172219 0.00011008 0.000147785 0.000168118 0.000111638 0.000161761 0.000193647 0.000201768 0.000156759 0.000175221 0.00012852 0.000159159 0.000136581 0.000142458 0.000104311 0.000137866 0.000153131 0.000194957 0.000151109 0.000133012 0.000112813 0.000154199 0.000165131 0.00019192 0.000141609 0.000130629 0.00013765 0.000185932 0.00016096 0.000129 0.000100294 0.000173186 0.000153843 0.000152076 0.000109613 0.000140937 0.000133521 0.000153002 0.000131424 0.000147679 0.000123386 0.000139856 0.000116689 0.000141218 0.000121856 0.00014494 0.000127773 0.000145369 0.000121058 0.000142542 0.000129803 0.000148939 0.000122527 0.00013909 0.000116454 0.000140474 0.000120404 0.000129426 0.000107437 0.000138172 0.000119912 0.000137334 0.000111384 0.000126984 0.000110148 0.000136653 0.000119516 0.00013837 0.000118661 0.000135258 0.000111201 0.000129452 0.000109565 0.000124679 0.000108109 0.000133821 0.000113002 0.000129722 0.000110658 0.000133673 0.000114235 0.000132992 0.000111121 0.000130611 0.000112623 0.000130159 0.000108879 0.00012951 0.000112183 0.000130109 0.00010963 0.000128652 0.000106292 0.00012649 0.000109276 0.000129228 0.000108783 0.000128619 0.000111234 0.000132835 0.000113863 0.000131056 0.000109608`</MagBuffer>
`               `<PhaseBuffer>`3.14159 3.08208 -2.84775 -2.84702 0.222501 -0.357415 -0.192005 0.857515 0.164266 -0.717075 -1.34692 -1.25306 -0.80304 -1.03352 -1.4255 -1.67493 -2.42337 2.28937 -0.463914 -1.0696 -1.81997 -2.63472 -0.265465 -0.803979 -1.55456 -2.49373 2.90535 -2.02756 -2.19864 -2.99324 2.16832 1.08515 0.158611 -1.07551 -1.91136 -1.9366 0.120491 -0.980804 -2.03769 -3.00178 1.04362 -0.356165 -0.979226 -1.18441 -1.39887 -1.52026 -1.83433 -2.18476 -2.52041 -0.899889 -1.20572 -1.48061 -1.48561 -1.60454 -2.08082 -1.92976 -1.82558 -1.80127 -1.87773 -1.57999 -1.50824 -1.43029 -1.62481 -1.58055 -1.61722 -1.70472 -1.89816 -1.36964 -0.93364 -1.11818 -1.2283 -1.20535 -1.13643 -1.42734 -2.23122 -2.95545 -1.89245 -2.28797 0.792653 -0.796647 -1.49208 -1.64987 -1.6976 -1.35185 -1.64026 -1.63719 -2.06527 -2.53459 -0.410569 -1.05411 -1.79081 -2.55591 2.60431 -0.288706 -1.10392 -1.49526 -1.97181 -2.15656 0.612444 -0.468561 -1.25125 -1.95891 2.80796 0.563956 -0.425114 -1.15636 -1.96053 2.55262 0.369696 -0.400003 -0.842419 -1.04522 0.108873 -0.174641 -0.470307 -0.682711 -0.872543 -0.952344 -0.925881 -0.755265 -0.494537 -0.745278 -1.03808 -0.533305 -0.283193 -0.606887 -0.655821 -0.569462 -0.580754 -0.722371 -0.719113 -0.685507 -0.751142 -0.97438 -1.3813 -1.14374 -0.315467 -0.331636 -0.143219 -0.349771 -0.678538 -0.204147 -0.216003 -0.837882 -1.42846 -1.64832 -0.382795 -0.614104 -1.17802 -1.55154 -1.80173 -1.9215 1.68532 -0.0644832 -0.821967 -0.637113 -0.409722 -0.694729 -0.596954 -0.316125 -0.470586 -0.732607 -0.744035 -0.781622 -0.776868 -0.647869 -0.750809 -0.719915 -0.622058 -0.367594 -0.35117 -0.553486 -0.79245 -0.674985 -0.452145 -0.342092 -0.397661 -0.599614 -0.773513 -0.555672 -0.3547 -0.493991 -0.741259 -0.803371 -0.285476 -0.357441 -0.587013 -0.65025 -0.577342 -0.39602 -0.443145 -0.473738 -0.505712 -0.503574 -0.511424 -0.479682 -0.442623 -0.40107 -0.404894 -0.38004 -0.408274 -0.418635 -0.415458 -0.360539 -0.375894 -0.421767 -0.44643 -0.414257 -0.397415 -0.363917 -0.427264 -0.405524 -0.307071 -0.282502 -0.335477 -0.343677 -0.364971 -0.292669 -0.240198 -0.227494 -0.262238 -0.266188 -0.290855 -0.294307 -0.299723 -0.254931 -0.263727 -0.224682 -0.161851 -0.174198 -0.203569 -0.185546 -0.155451 -0.151441 -0.168252 -0.167166 -0.170164 -0.139481 -0.147306 -0.145664 -0.130857 -0.102859 -0.108977 -0.113821 -0.103976 -0.100063 -0.0870527 -0.0498648 -0.0388209 -0.0417596 -0.0324963 -0.0127574 0.00388132 -0.0023968 -0.016122 -0.0216873 0`</PhaseBuffer>` `

`           `</ResidualSpec>` `

`       `</Frame>` ... `

If you prefer to have a more compact representation of your analysis you should choose the SDIF format. SDIF (Sound Description Interchange Format) is a non-propietary format used for exchanging analysis data especially between research teams.

Configuration
-------------

In order to make the application work, you first need to load a configuration xml file or edit the default one through the graphical interface. This configuration includes all the different parameters for the analysis/synthesis process. If you load an xml file that includes non-valid parameter values (like a path to a non-existing input sound file), the analysis process will remain disabled. (Note: if you try to load an xml file that does not comply to the Configuration schema, the result is unpredictable and may even produce a program crash).

Here is an example of a valid xml configuration:

<SMSAnalysisSynthesisConfig>

`   `<Name />
`   `<InputSoundFile>`c:/1_brief.wav`</InputSoundFile>
`   `<OutputSoundFile>`c:/1_out.wav`</OutputSoundFile>
`   `<OutputAnalysisFile>`c:/analysis.sdif`</OutputAnalysisFile>
`   `<InputAnalysisFile>`c:/analysis.xml`</InputAnalysisFile>
`   `<AnalysisWindowSize>`513`</AnalysisWindowSize>
`   `<AnalysisHopSize>`256`</AnalysisHopSize>
`   `<AnalysisWindowType>`Hamming`</AnalysisWindowType>
`   `<ResAnalysisWindowSize>`513`</ResAnalysisWindowSize>
`   `<ResAnalysisWindowType>`BlackmanHarris92`</ResAnalysisWindowType>
`   `<AnalysisZeroPaddingFactor>`0`</AnalysisZeroPaddingFactor>
`   `<AnalysisPeakDetectMagThreshold>`-120`</AnalysisPeakDetectMagThreshold>
`   `<AnalysisPeakDetectMaxFreq>`-120`</AnalysisPeakDetectMaxFreq>
`   `<AnalysisSinTrackingFreqDeviation>`20`</AnalysisSinTrackingFreqDeviation>
`   `<AnalysisReferenceFundFreq>`1000`</AnalysisReferenceFundFreq>
`   `<AnalysisLowestFundFreq>`40`</AnalysisLowestFundFreq>
`   `<AnalysisHighestFundFreq>`6000`</AnalysisHighestFundFreq>
`   `<AnalysisMaxFundCandidates>`50`</AnalysisMaxFundCandidates>
`   `<AnalysisHarmonic>`0`</AnalysisHarmonic>
`   `<DoCleanTracks>`0`</DoCleanTracks>
`   `<SynthesisFrameSize>`256`</SynthesisFrameSize>
`   `<SynthesisWindowType>`Triangular`</SynthesisWindowType>
`   `<SynthesisHopSize>`-1`</SynthesisHopSize>
`   `<SynthesisZeroPaddingFactor>`0`</SynthesisZeroPaddingFactor>

</SMSAnalysisSynthesisConfig>

Let us comment the different parameters involved:

Global Parameters:

<Name>: Particular name you want to give to your configuration file. Not used for anything except the xml parsing.

<InputSoundFile>: path and name of the input sound file you want to analyze (depending if you are running the application in GNU/Linux or Windows that will be a unix path or an msdos path).

<OutputSoundFile>: path and name of where you want to have your output synthesized sound file. The application will add a "\_sin.wav" termination to the Sinusoidal component and a "\_res.wav" termination the residual file name. In the graphical version of the program (SMSTools) though, this parameter is not used as when the output sound is to be stored, a file browser dialog pops-up.

<OutputAnalysisFile>: path and name of where you want your output analysis data to be stored. The extension of the file can be .xml or .sdif. The application will choose the correct format depending on the extension you give.Not used in the GUI version as it is obtained from the dialog.

<InputAnalysisFile>: path and name of where you want your input analysis data to be loaded from. Not used in the GUI version as it is obtained from the dialog.

(Note to users of previous versions: Sampling Rate is no longer used as it is automatically extracted from the input sound file).

Analysis Parameters

<AnalysisWindowSize>: window size in number of samples for the analysis of the sinusoidal component. (Note: if the value entered is not odd, the program will internally add +1 to it)

<ResAnalysisWindowSize>: window size in number of samples for the analysis of the residual component. (Note: if the value entered is not odd, the program will internally add +1 to it)

<AnalysisWindowType>: type of window used for the sinusoidal analysis. Available: Hamming, Triangular, BlackmannHarris62, BlackmannHarris70, BlackmannHarris74, BlackmannHarris92, KaisserBessel17, KaisserBessel18, KaisserBessel19, KaisserBessel20, KaisserBessel25, KaisserBessel30, KaisserBessel35.

<ResAnalysisWindowType>: type of window used for the residual analysis. Available: Same as in sinusoidal. Recommended: as sinusoidal spectrum is synthesized using the transform of the BlackmannHarris 92dB, it is necessary to use that window in the analysis of the residual component in order to get good results.

<AnalysisHopSize>: hop size in number of samples. It is the same both for the sinusoidal and residual component. If this parameter is set to -1 (which means default), it is taken as half the residual window size. Recommended values range from half to a quarter of the residual window size.

<AnalysisZeroPaddingFactor> Zero padding factor applied to both components. 0 means that zeros will be added to the input audio frame till it reaches the next power of two, 1 means that zeros will be added to the next power of two etc...

<AnalysisPeakDetectMagThreshold>: magnitude threshold in dB's in order to say that a given peak is valid or not. Recommended: depending on the window type and the main-to-secondary lobe relation and the characteristics of the input sound, a good value for this parameter may range between -80 to -150 dB.

<AnalysisPeakDetectMaxFreq>: Frequency of the highest sinusoid to be tracked. This parameter can be adjusted, for example, if you are anlyzing a sound that you know only has harmonics up to a certain frequency. Recommended: It depends on the input sound but, in general, a sensible value is 8000 to 10000 Hz.

<AnalysisSinTrackingFreqDeviation>: maximum deviation in hertz for a sinusoidal track.

<AnalyisReferenceFundFreq>: in hertz, reference fundamental.

<AnalyisLowestFundFreq>: in hertz, lowest fundamental frequency to be acknowledged.

<AnalyisHighestFundFreq>: in hertz, highest fundamental frequency to be acknowledged.

<AnalyisMaxFundFreqError>: maximum error in hertz for the fundamental detection algorithm.

<AnalyisMaxFundCandidates>: maximum number of candidate frequencies for the fundamental detection algorithm.

<AnalysisHarmonic>: if 1, harmonic analysis is performed on all segments that have a valid pitch. In those segments the track number assigned to each peak corresponds to the harmonic number. On unvoiced segments, inharmonic analysis is still performed. Set to 1 if the type of transformation you want to perform depends on this harmonic characteristic. It is highly recommended that if you use harmonic analysis you set a low threshold for AnalysisPeakDetectMagThreshold\> (i.e. -120). This means that many peaks (more than necessary) will be detected in the peak detection process but will then be removed in the harmonic tracking process.

Synthesis Parameters

<SynthesisFrameSize>: in number of samples, size of the synthesis frame. If set to -1, it is computed as (ResAnalysisWindowSize-1)/2. If any other number is used you are bound to get synthesis artefacts.

<SynthesisWindowType>: type of window used for the residual analysis. Available: Same as in sinusoidal.

Morph Parameters

<MorphSoundFile>: Optional name of the second file to do a morph on. Only necessary if you want to do a morphing transformation afterwards. Note that the file to morph will be analyzed with the same parameters as the input sound file and that it must have the same sampling rate.

112 Synthesis

Apart from storing the result of your analysis, you can do more interesting things. The first thing you may like to do is synthesize it back, separating each component: residual, sinusoidal, and the sum of both. The following picture illustrates the SMS synthesis algorithm:

For invoking the synthesis procedure you just have to call the 'synthesize' option (either from the GUI or the menu) and then look or store each of the components. 113 Transformation

To transform your sound you first have to load an xml transformation score or use the graphical transformation editor available in SMSTools. It is strongly recommended that you use the graphical interface in order to generate the transformation chain you want to apply to your sound. Click on File-\>Transformation Score-\>Edit Score. A new dialog like the one depicted in the next figure will pop-up.

On the left you have a list of available SMS transformations. By clicking on any of them you will get a brief description on the right panel. This description is just intended to give you a very concise explanation on how the transformation works. If you should need a more detailed explanation the recommended source is the book DAFX - Digital Audio Effects, edited by Udo Zoelzer and published by Wiley and Sons Ltd. Once you have decided what transformation you want to use you have click on the "Add to Score" button. The transformation will be added to the central panel, where the list of used transformations is located. Now you are ready to set the parameters for the transformations. Select the "Parameters" tab on the right-hand panel.

Once you have edited a transformation, you can repeat the previous steps to add a new transformations. You may add as many transformations as you need (even more than one instance of the same transformation). Note though that the way the transformations are sorted affects the output (i.e. it is not the same to apply a pitch shift and then a spectral shape shift than doing it the other way around).

Although some transformations have simpler controls, most of them are configured using a break-point-function (or envelope-like) control like the one shown in the previous picture. Note that this break point funtion may represent an evolution over time or an spectral envelope, depending on the type of transformation. On the bpf grid, click to select and move and double click to add a point. You can select a point or a set of points by drawing a selection box. Then you can delete them pressing Ctrl X, you can move them with the mouse or the cursor keys, you can mirror them with the '\*' key or increase or decrease the difference to the mean value with '+' and '-' keys.

But if you don't want to use the graphical editor or you are using the non-gui version you will need to understand how the xml score works.This configuration file is basically a composition of concrete configurations that affect specific transformations. You may change the order, activate/deactivate or configure any of them.The way the configuration affects a concrete transformation may vary, you should read the comments before each transformation for learning the particularities of every transformation.All transformations share some common fields. First they need to define the kind of concrete transformation in the ConcreteClassName attribute. Then you need to specify the values for the transformation. This can usually be done in two different ways: using a single-value Amount attribute or a breakpoint function BPFAmount attribute in which you declare a function as a set of points and an interpolation type (note: this function may represent a time envelope or a spectral envelope depending on the transformation). In any transformation chain there need to be two special transformations called SMSTransformationChainIO as first and last transformation to the chain. These transformations act as input and output, are mandatory but do not affect the result.After the list of transformations there is an 'OnArray' where you have the same number of 1's or 0's as transformations in the Chain. 1 means active and 0 means bypassed (SMSTransformationChainIO's are not affected by this).

Here is a complete commented example of how a transformation score looks like:

<SMSTransformationChainConfig>

`   `<Configurations>

`       `<Config>
`       `<ConcreteClassName>`SMSTransformationChainIO`</ConcreteClassName>
`       `</Config>

`       `<Config>
`       `<ConcreteClassName>`SMSPitchShift`</ConcreteClassName>
`       `<BPFAmount>
`       `<Interpolation>`Linear`</Interpolation>
`       `<Points>`{0 1.5} {1 1.5}`</Points>
`       `</BPFAmount>

`       `</Config>
`       `
`       `<Config>
`       `<ConcreteClassName>`SMSGenderChange`</ConcreteClassName>
`       `<Amount>`0`</Amount>
`       `</Config>

`       `<Config>
`       `<ConcreteClassName>`SMSPitchDiscretization`</ConcreteClassName>
`       `</Config>

`       `<Config>
`       `<ConcreteClassName>`SMSOddEvenHarmonicRatio`</ConcreteClassName>
`       `<Amount>`12`</Amount>
`       `</Config>

`       `<Config>
`       `<ConcreteClassName>`SMSFreqShift`</ConcreteClassName>
`       `<Amount>`100`</Amount>
`       `</Config>

`       `<Config>
`       `<ConcreteClassName>`SMSSineFilter`</ConcreteClassName>
`       `<BPFAmount>
`       `<Interpolation>`Step`</Interpolation>
`       `<Points>`{0 6} {1 0} {2 0} {100 0} `</Points>
`       `</BPFAmount>
`       `</Config>

`       `<Config>
`       `<ConcreteClassName>`SMSResidualGain`</ConcreteClassName>
`       `<Amount>`6`</Amount>
`       `</Config>

`       `<Config>
`       `<ConcreteClassName>`SMSSinusoidalGain`</ConcreteClassName>
`       `<Amount>`3`</Amount>
`       `</Config>

`       `<Config>
`       `<ConcreteClassName>`SMSHarmonizer`</ConcreteClassName>
`       `<BPFAmount>
`       `<Interpolation>`Step`</Interpolation>
`       `<Points>`{-3 1.3} {-3 1.5} {-3 1.7}`</Points>
`       `</BPFAmount>
`       `</Config>

`       `<Config>
`       `<ConcreteClassName>`SMSSpectralShapeShift`</ConcreteClassName>
`       `<Amount>`50`</Amount>
`       `</Config>

`       `<Config>
`       `<ConcreteClassName>`SMSMorph`</ConcreteClassName>
`       `<HybBPF>
`       `<Interpolation>`Linear`</Interpolation>
`       `<Points>`{0 0} {1 1}`</Points>
`       `</HybBPF>
`       `</Config>

`       `<Config>
`       `<ConcreteClassName>`SMSTransformationChainIO`</ConcreteClassName>
`       `</Config>

`   `</Configurations>

`   `<OnArray>`1 0 0 0 0 0 0 0 0 0 0 1 1`</OnArray>

</SMSTransformationChainConfig>

Implementing your own transformation
------------------------------------

If you want to implement a particular transformation (other from the very simple frequency shift included as a sample) you will have to get into a little coding (don't panic, it is very simple!).

These are the steps you have to follow:

`  1. Look at any of the basic already implemented transformations located at clam/src/Processing/Transformations/SMS (SMSFreqShift will do fine as a first step, do not wander into time SMSTimeStretch and SMSMorph as they are much more complicated. Yeah, I bet now that is the first thing you are planning on doing).`
`  2. Write your own MyTransformation.hxx and MyTransformation.cxx files following the same structure. In the header, note that your class MyTransformation must derive from a template class named SMSTransformationTmpl. As the template argument, you must use the part of the analyzed frame you want to transform: SpectralPeakArray if you want to transform the sinusoidal component, Spectrum if you want to transform the residual component or Frame if you want to transform both. Then you need to declare the following methods: GetClassName that must return your classes name, a default constructor, a constructor that takes in an SMSTransformationConfig as argument, and a Do(T,T) method where T is the concrete type on which the transformation is applied (SpectralPeakArray, Spectrum or Frame). Here is how the header of MyTransformation.hxx would look like if we decided we wanted to transform only the residual component:`

`       class MyTransformation: public SMSTransformationTmpl`<SpectralPeakArray>
`       {`
`               const char *GetClassName() const {return "MyTransformation";}`
`       public:`
`               MyTransformation(){}`
`               MyTransformation(const SMSTransformationConfig &c):SMSTransformationTmpl`<SpectralPeakArray>`(c){}`
`               bool Do(const SpectralPeakArray& in, SpectralPeakArray& out);     `
`       };`
`  `

`  3. Now go to MyTransformation.cxx file and implement the code for your transformation.`

`     So, if MyTransformation only gets every spectral peak and multiplies its frequency by two, I should only have to write the following code:`

bool MyTransformation::Do(const SpectralPeakArray& in, SpectralPeakArray& out) {

`  TSize nPeaks=in.GetnPeaks()`
`  DataArray& inBuffer=in.GetFreqBuffer();`
`  DataArray& outBuffer=out.GetFreqBuffer();`
`  for(int i=0;i`<nPeaks;i++)
      outBuffer[i]=inBuffer[i]*2;
   return true;
}

      Note that the only difference with the code you will find in the SMSFreqShift.cxx file is that here we are using a constant multiplying factor while there it takes the value out of a control by doing mAmountCtrl.GetLastValue(). This CLAM control returns holds the current value from the transformation and it is updated automatically from the SMS application
   4. After adding the necessary includes in your files, you are ready to compile your new SMS transformation. It is now ready to use. But, in order to make it automatically available from the SMS application a few more things need to be done to integrate your transformation into what we call the SMSTransformationChain. The steps that follow are in some cases a bit of a hack and we are working on simplifying them in a next release:
          * First we must register the MyTransformation class in the Processing Factory (implementation of the Factory Method pattern for those of you who are into software engineering). For doing so, you must go to the MyTransformation.cxx file and add the following lines:

        typedef CLAM::Factory<CLAM::Processing>` ProcessingFactory;`
`       static ProcessingFactory::Registrator`<CLAM::SMSFreqShift>` regtMyTransformation( "MyTransformation" );`

`         * Then you need to make the ProcessingChain class that a new transformation has been added and the type of configuration it uses. Open ProcessingChain.cxx file, search for the InstantiateConcreteConfig method and you will see a long and ugly "if" that includes most transformations. Help us make it uglier by adding your transformation over there:`

if(type=="SMSDummyTransformation"||type=="SMSFreqShift"||type=="SMSPitchShift"||

`        type=="SMSOddEvenHarmonicRatio"||type=="SMSSineFilter"||type=="SMSResidualGain"||`
`        type=="SMSHarmonizer"||type=="SMSSinusoidalGain"||type=="SMSPitchDiscretization"||`
`        type=="SMSSpectralShapeShift"||type=="SMSGenderChange"||type=="SMSTransformationChainIO"`
`                ||type=="MyTransformation")`

`  5. Finally, you may want to add a widget to the graphical interface in order to configure your transformation. We recommend you to just follow the example of a pre-existing configurator in /examples/sms/tools/gui. The SMSFreqShiftConfigurator, for instance, will do fine for a simple configuration.`

You are now ready to build the program again and used your configuration from the SMS application. If you are the of curious kind you have surely already wandered into the SMSMorph and SMSTimeStretch transformations. These transformations are much more complex because the (1) use a different configuration instead of the default SMSTransformationConfig, (2) override some of the behaviour of the base SMSTransformation class because they have a more complex logic (SMSTimeStretch, for example processes at a non-constant rate), (3) They are based on other existing Processings. We will leave the exercise of implementing such transformation to the advanced reader (remember you may always get support through the CLAM mailing list clam@iua.upf.es).

Internal class structure and program organisation
-------------------------------------------------

You should only read this section if you are particularly interested in learning 'what's inside' the example or you want to use its structure as a base for another application. Otherwise, if you are using the SMS as an aplication on its own and could not care less about programming details you better skip this part (for your own sake!).

The rest of this section will deal with the main structural aspects with the application. All these aspects are summarized in the following UML diagram:

The main class of our application is the SMSBase class. This is an abstract class (thus cannot be instantiated), but contains the core of the process flow. The two derived classes, SMSTools and SMSStdio are the GUI and Standard I/O versions of the base class.

So let us briefly mention what this base class holds inside. All the methods illustrated in the diagram (LoadConfig, Analyze,...) correspond to functionalities of the program that, in the case of the GUI version, are mapped directly to menu options. Of course the class has other methods but are used for internal convenience but are less important.

The boolean (mHaveConfig, mHaveInputAudio,....) attributes of the class hold important values to control the flow of the program because they inform of whether a previous action has taken place and the desired operation can then be invoked.

The class has two Processing Composite attributes, instances of the SMSAnalysis and SMSSynthesis classes. These Processing Composites are configured when the global configuration is loaded and then run from the Analyze and Synthesize methods. Some intermediate Processing data (a Segment, a Melody and different Audio objects) are used to hold the input/output data generated during the process. These data is then stored/played using the corresponding method (i.e. StoreAnalysis or PlayOutputSound).

You will see that, although this class concentrates most of the functionality of the application and has a great deal of operations, these methods are fairly simple and rarely need more than 10/20 lines of code (as a matter of fact, the longest operation is the Melody analysis, which should not be included in this class but rather in a separate Processing class). Much as the complex logic of some method delegation (as the one existing in the methods Analyze, DoAnalysis, AnalysisProcessing...) is introduced by the needs of the graphical interface, where callbacks need to be assign to GUI commands and methods need to be called on separate threads. But, as an example, let us take a look at the AnalysisProcessing method that implements the actual process for the analysis:

1. void SMSBase::AnalysisProcessing() {

2. Flush(mOriginalSegment);

3. int k=0;

4. int step=mAnalConfig.GetHopSize();

5. GetAnalysis().Start(); 6. while(GetAnalysis().Do(mOriginalSegment)) { 7. k=step\*(mOriginalSegment.mCurrentFrameIndex+1); 8. mCurrentProgressIndicator-\>Update(float(k)); } 9. GetAnalysis().Stop(); }

In line 1 we call the Flush method, which initializes the member Segment and deletes previously existing data. In the next two lines we declare and initialize the variables that will be used in the analysis loop: k is the counter for updating the ProgressIndicator and step is the analysis hop size. In line 5 we Start our SMSAnalysis Processin. In line 6 the loop begins. Note that the output condition is the return value of the call to the Do in the SMSAnalysis. In lines 7-8 we update the progress indicator and finally, in line 9, we Stop the SMSAnalysis Processing.

Note that, if we remove the support for the Progress Indicator the resulting method would only be 5 lines long:

1. void SMSBase::AnalysisProcessing() {

`  2. Flush(mOriginalSegment);`
`  3. GetAnalysis().Start(); `
`  4. while(GetAnalysis().Do(mOriginalSegment)) {} `
`  5. GetAnalysis().Stop(); }`

If you follow a similar approach you can fairly easily understand all the members of out main application class.

SMSSynthesis and SMSAnalysis
----------------------------

We have seen how simple the processing part of our application is: basically a call to the Do method of either SMSAnalysis or SMSSynthesis classes. That is only possible because these Processing Composites hide all the processing complexity.

If we take a look again to the UML diagram we see that these classes contain inside a great deal of other Processing classes. Let us enumerate them and their basic functionality.

Inside the SMSAnalysis we have:

SpectralAnalysis: Performs an STFT of the sound. For doing so, it holds a number of Processing objects inside, namely a WindowGenerator, an AudioMultiplier, a CircularShift (for zero-phase buffer centering) and an FFT. Note that the SMSAnalysis has two instances of this class: one for the sinusoidal component and another one for the residual. This Processing Composite is quite complex in itself but we won't go into details.

SpectralPeakDetect: Implements an algorithm for choosing the spectral peaks out of the previously computed spectrum.

FundFreqDetect: Processing for computing the fundamental frequency.

SinTracking: This Processing performs sinusoidal tracking or peak continuation from one frame to the next one. It implements an inharmonic and harmonic version of the algorithm.

SynthSineSpectrum: Once we have analyzed the sinusoidal component and we have the continued peaks we have to synthesize it back to spectrum in order to compute the residual component. This is the Processing in charge of this synthesis of the sinusoidal component.

SpectrumSubstracter2: Once we have the sinusoidal synthesized spectrum and the original one (coming out from the residual Spectral Analysis), we can subtract them in order to obtain the residual spectrum.

The SMSSynthesis Processing Composite contains:

PhaseManagement: This Processing is in charge of managing phase of spectral peaks, from one frame to the next one.

SynthSineSpectrum: As already commented in the SMSAnalysis, this object is in charge of creating a synthetic spectrum out of the array of spectral peaks.

SpectrumAdder2: Is used to add the spectrum of the residual and the synthesized spectrum.

SpectralSynthesis: This processing composite implements the inverse STFT. That is, is the object in charge of computing an output audio frame from an input spectrum.The SMSSynthesis class has three instances of this class: one for the global output sound, one for the residual and one for the sinusoidal component. The SpectralSynthesis Processing Composite has the following processing inside: an IFFT, two WindowGenerators (one for the inverse Analysis window and one for the Synthesis Triangular window), an AudioProduct to actually perform the windowing, a CircularShit to undo the CircularShift introduced in the Analyisis and an OverlapAdd object to finally apply this process to the output windowed audio frames. As you can see, it is fairly complex in itself and we would need to go into too many signal processing details in order to explain it completely.

So, that is about all. If you feel you still need to know more about the application you can read the above mentioned classes. If you think you'd rather learn more about signal processing details you can browse through the MTG bibliography.