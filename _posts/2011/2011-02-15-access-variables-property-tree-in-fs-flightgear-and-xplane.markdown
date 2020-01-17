---
layout: "post"
title: "Access Variables/Property tree in FS,FlightGear and XPlane"
date: "2011-02-15 14:58"
tags: [fsuipc, fsx]
categories:
- tutorial
---
# How to talk with your simulator ?

Add-ons developper or cockpit builders have to access flight simulator variables, to mange or display them. If coding is not your cup of tea, you can use OpenCockpit software for example, a “simple” GUI to interract with hardware provided by this site.

If you are not affraid by coding, many existing libraries and tools are available to talk with your favorite flight simulator. I will present solutions for MS Flight Simulator, Flight Gear and X-Plane, using C++ language in code examples.

# MS Flight Simulator

MS Flight simulator variables can be asscessed using multiple tools. Peter Dowson’s FSUIPC is a very common interface to read/write Flight Simulator 2000/2002/2004/X and Combat Flight simulators variables. It is well documented, stable and used by a large community of simers and developpers.

Help and support is provided directly by Peter Dowson (many thanks for your job and for your help Pete!). Two versions of FSUIPC are available: Free and registred. A second usefull tool working with FSUIPC is WideFS. WideFS allow to send FS values via the network to another computer (many cockpit builder are using a ‘cluster’ for their simulator: one PC is managing flight model and display, one (or more) is managing intruments panel, and one is managing vocal communications).
Each variable is identified by an Offset (offset is the address of the variable) and a type. Just refer to this offset in your program, and read or write it (some variables are read only).

Variable type is very important. If not set correctly, result will be very strange…
Flight Simulator provides some variables in BCD format (radio frequencies for example) or maked values (radio control box).

First download and uncompress FSUIPC SDK. It contains many examples using Basic, Java and C++, FSUIPC libraries and FSInterrogate, a tool to test variables access, that additionnaly gives variable format description. Playing with FSInterrogate will help you to understand how is works, and what is variables range.

FSUIPC_User.lib has to be included in your library path. FSUIPClib.h contains class headers. FSUIPC has to be connected to FS first (example below comes from SDK example code, you can also parse onePanel source code for concrete example). Program below detects FS version, returns FS clock and provide two functions to read or write values.

```c++
#include <windows.h>
#include <stdio.h>
#include <string>
#include "FSUIPClib.h"

int FSUIPC::connect(void)
{
  char const *pszErrors[] = {
    "Ok",
    "Connexion already opened",
    "Unable to connect FSUIPC or WideClient",
    "Failed to register Windows common message",
    "Fail to create Atom for file name mapping",
    "Failed to create a file mapping object",
    "Failed to open a view to the file map",
    "Incorrect version of FSUIPC, or not FSUIPC",
    "Sim is not version requested",
    "Call cannot execute, link not Open",
    "Call cannot execute: no requests accumulated",
    "IPC timed out all retries",
    "IPC sendmessage failed all retries",
    "IPC request contains bad data",
    "Maybe running on WideClient but FS not running on Server or wrong FSUIPC",
    "Read or Write request cannot be added, memory for Process is full",
  };

  char chMsg[128], chTimeMsg[64];
  char chTime[3];
  BOOL fTimeOk = TRUE;
  static char const *pFS[] = { "FS98", "FS2000", "CFS2", "CFS1", "Fly!", "FS2002", "FS2004" };
  static char chOurKey[] = "IKB3BI67TCHE";

  if (FSUIPC_Write(0x8001, 12, chOurKey, &dwResult))
    FSUIPC_Process(&dwResult);

 (!FSUIPC_Read(0x238, 3, chTime, &dwResult) || !FSUIPC_Process(&dwResult))
    fTimeOk = FALSE;

  if (fTimeOk)
    printf("Time request ok: FS clock = %02d:%02d:%02d\n", chTime[0], chTime[1], chTime[2]);
  else
    printf("Time request failed: %s\n", pszErrors[dwResult]);

  printf("Simulator %s\nFSUIPC version = %c.%c%c%c%c\r%s\n",
    (FSUIPC_FS_Version && (FSUIPC_FS_Version <= 7)) ? pFS[FSUIPC_FS_Version - 1] : "Unknown FS version",
    '0' + (0x0f & (FSUIPC_Version >> 28)),
    '0' + (0x0f & (FSUIPC_Version >> 24)),
    '0' + (0x0f & (FSUIPC_Version >> 20)),
    '0' + (0x0f & (FSUIPC_Version >> 16)),
    (FSUIPC_Version & 0xffff) ? 'a' + (FSUIPC_Version & 0xff) - 1 : ' ', chTimeMsg);
  printf("Linked with FSUIPC.\n") ;
  if (!FSUIPC_Open(SIM_ANY, &dwResult)) {
    FSUIPC_Close();
    return -1;
    }
  return 0;
}

void FSUIPC::close(void)
{
  FSUIPC_Close();
  fprintf(stdout,"Closing FSUIPC connection.\n");
  return;
}

short FSUIPC::readFSValue_int_signed(int offset, int size)
{
  short FSUIPCValue_int_signed = 0;
  FSUIPC_Read(offset, size, &FSUIPCValue_int_signed, &dwResult);
  FSUIPC_Process(&dwResult);
  return short(FSUIPCValue_int_signed);
}

void FSUIPC::writeFSValue_int(int offset, int size, int value = 0)
{
  FSUIPC_Write(offset, size, &value, &dwResult);
  FSUIPC_Process(&dwResult);
  return;
}
```
# Fligh Gear

[FlightGear Wiki](http://wiki.flightgear.org/index.php/Property_Tree)

# XPlane

[XPlane6](http://www.jefflewis.net/XPlaneUDP_6.html)

[XPlane8](http://www.jefflewis.net/XPlaneUDP_8.html)

[XPlane9](http://www.jefflewis.net/XPlaneUDP_9.html)
