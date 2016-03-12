You can [build a CLAM network by hand](Constructing and playing a simple network "wikilink") but you can also load an XML definition.

The XML definition of a network contains which processings are on the network, their configurations and how are they connected. Such definition could be built using the NetworkEditor or building one by hand, by doing:

`   XMLStorage::Dump(network, "mynetwork.clamnetwork", "Network");`

NetworkEditor is distributed with some interesting network examples.

main.cxx
--------

`// Updated to 1.1 (svn)`
`#include `<CLAM/Network.hxx>
`#include `<CLAM/PANetworkPlayer.hxx>
`#include `<CLAM/XMLStorage.hxx>
`int error(const std::string & msg)`
`{`
`   std::cerr << msg << std::endl;`
`   return -1;`
`}`
`int main(int argc, char ** argv)`
`{`
`   if (argc!=2) return error ("needs a network filename.");`
`   CLAM::Network network;`
`   try`
`   {`
`       CLAM::XMLStorage::Restore(network, argv[1]);`
`   }`
`   catch (CLAM::XmlStorageErr & e)`
`   {`
`       return error("Could not open the network file");`
`   }`
`   // Set the audio backend to PortAudio`
`   network.SetPlayer(new CLAM::PANetworkPlayer);`
`   network.Start();`
`   sleep(4); // TODO: until?`
`   network.Stop();`
`}`
