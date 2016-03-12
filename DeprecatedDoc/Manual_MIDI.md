[Back to Index](DeprecatedDoc/CLAMUserManual "wikilink")

The MIDIIO approach has several similarities with the AudioIO, and it is recommended to read the AudioIO documentation first. Basic knowledge of the MIDI protocol is required.

The following documentation reflects the MIDIIO implementation since CLAM 0.6.1. The main difference with earlier releases is the addition of MIDI output, a simplification of the MIDIInControl (no more ChannelMasks and MessageMasks - you'll have to create a MIDIInControl for each channel (or one for all) / message type)

The MIDIManager
===============

The core of CLAM midi input is the MIDIManager class. TheMIDIManager takes care of all administrative tasks concerning the creation and initialization of midi input streams, using the internal, system dependent MIDIDevice class.

The first thing you need to do in order to use MIDI, is to create a MIDIManager object. This object will be a singleton, and all subsequent MIDI I/O objects created will use it.

MIDIManager midiManager;

MIDI I/O Processings and their configuration
============================================

The MIDIIn and MIDIInControl class
----------------------------------

The actual MIDI input class, called MIDIIn, can be used to parse incoming MIDI data, and handle it in any way. But more useful, in the CLAM context, is the derived class, MIDIInControl. A MIDIInControl has one or more OutControls, and can be used to convert the incoming MIDI data to ControlData. MIDIIn and MIDIInControl objects have to be configured with a MIDIIOConfig object.

The MIDIOut and MIDIOutControl class
------------------------------------

TODO: Example MIDIOut

The MIDIIOConfig class
----------------------

Both MIDIIn, and derived MIDIInControl, and MIDIOut, and derived MIDIOutControl, have te be configured with MIDIIOConfig. This ProcessingConfig contains the following fields: Name Set with Meaning for MIDIIn Meaning for MIDIOut Observations Device SetDevice Specifies the midi device to use. Specifies the midi device to use. See section 64.1. Channel SetChannel Specify channel (1-16) to listen to for incoming MIDI messages. If set to 0, the MIDIInControl will listen to all channels, and create an OutControl (the first) to output for each message what channel it appeared on Specify channel (1-16) for outgoing MIDI messages. If set to 0, the MIDIOutControl will create an InControl (the first), that can be used for specify the channel for each message. Message SetMessage Specify message type to listen to for incoming MIDI messages. Specify message type for outgoing MIDI messages. MIDI message types are specified in src/Tools/MIDI/MIDIEnums.hxx, see section 65 FirstData SetFirstData Specify first data byte to listen to for incoming MIDI messages of the specified message type. If left unconfigured, or set to 128 (default), the MIDIInControl will have an OutControl that outputs the first data value of each incoming message. Specify first data byte for outgoing MIDI messages. If left unconfigured, or set to 128 (default), the MIDIOutControl will have an InControl the control it. This is especially usefull for control change messages, where the first data byte specifies the type of control change. (For example, 1 is modulation, 11 is breath/expression).

On the input side, the MIDIManager and MIDIDevices use this information to create a very efficient MIDI parsing table.

Dynamically created InControls and OutControls
----------------------------------------------

The number of InControls / OutControls on a MIDIOutControl / MIDIInControl resp., depends on the configuration used. This is reflected in the table above. Resuming, in case of the MIDIInControl, you will only get OutControls for those fields that you do not specify a specific filter value for. In the case of the MIDIOutControl, you will only get InControls for those fields that you do not specify a specific default value for. In both cases, this is done through the MIDIIOConfig

The name of each InControl / OutControl is set to "<MESSAGE:FIELD>" automatically, for example "NoteOn:Key", "NoteOn:Vel", etc. You can obtain this with the method

`const std::string& OutControl::GetName()`
`const std::string& InControl::GetName()`

The MIDIDevice class
====================

Specifying the MIDI device
--------------------------

The device is referred to with a string that has the following form:

`"ARCHITECTURE:DEVICE"`

Currently, the implemented architectures are alsa and portmidi, and the "virtual" MIDI file devices file (input only) and textfile (output only). The available devices depend on your hardware and system configuration. You can use the class MIDIDeviceList, to obtain a list of available devices for the platform you use:

`MIDIManager::FindList(arch)->AvailableDevices()`

This returns a const std::vector<std::string>&

However, if you don't specify the device, or use the string "default:default", the MIDIManager will choose the device that seems most adequate for your architecture. Similar, it is possible to obtain a list for the default architecture, passing "default" to the MIDIDeviceList::FindList method.

Clocking the MIDI device
------------------------

MIDIEnums.hxx defines the following: When the MIDI input comes from a device, typically live input, MIDI messages gets delivered through the controls as soon as they come in. In the case of the special "virtual" FileMIDIDevice, this situation is slightly different, and you will have to add a MIDIClocker to control the sequencing of the data in the MIDI file. Please look at the MIDI\_Synthesizer\_example for info.

MIDI Enums
==========

MIDIEnums.hxx defines the following:

`  eNoteOff = 0,`
`  eNoteOn = 1,`
`  ePolyAftertouch = 2,`
`  eControlChange = 3,`
`  eProgramChange = 4,`
`  eAftertouch = 5,`
`  ePitchbend = 6,`
`  eSystem = 7`
