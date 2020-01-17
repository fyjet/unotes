---
layout: "post"
title: "Howto access and publish addon specific variables with FSUIPC"
date: "2014-08-01 14:44"
tags: [fsuipc, fsx]
categories:
- programming
---
Flight Simulator addons often have specific internal variables that are not published in FSUIPC standard offsets. These values could be interresting for cockpit building. For example, A2A P47 addon does not publish turbo RPM value, but this parameter is essential for engine management in high altitude flight.

Variable should be first identified by its name, and then published in FSUIPC using a free offset. A LUA script is used to publish variable. You can also use variables to compute values or do anything else with LUA program.

# Identify L:varname

- Run Flight Sumilator
- Select the plane you want to extract variable, and fly
- Open FSUIPC GUI (from Flight Simulator menu > Add-on > FSUIPC…
- Select “Key Presses” tab , then set “ctrl + $” key with action “List Local Panel Vars”
- Select “Logging” tab, check “button and key operation” then “ok”
- When returned to simulator, while running, press “Ctrl + $”. It will then dump all addon internal variables into FSUIPC log file
- Open FSUIPC log file “FlightSimulatorInstallDir”/Modules/FSUIPC4.log with a text editor
- Find line “Panel includes these local variables:”, following lines contain “L:varname” and value

For example, P47 turbocharger RPM is “L:Turbocharger1Rpm”

# Publish variable in FSUIPC

Create a LUA script with name P47.lua (extention .lua is mandatory) contaiing actions:
```lua
while 1 do
  -- Read desired LVars
  rpm = ipc.readLvar("L:EngRPM_1")
  boost = ipc.readLvar("L:Turbocharger1Rpm")
  manpress = ipc.readLvar("L:ManifoldPressure1") * 10

  -- Write values to free FSUIPC offset for further use
  ipc.writeSD(0x66c0, rpm)
  ipc.writeSD(0x66c8, boost)
  ipc.writeSD(0x66d0, manpress)

  ipc.sleep(68)
end
```

# Access variable value

Variable value can be accessed via FSUIPC as usual (programmatically with FSUIPC API, or IOCP server and Gauge Composer).

# Variables size
Data type Data length FSUIPC osffsets
- unsigned 8 bit byte UB 8 1
- unsigned 16 bit word UW 16 2
- unsigned 32 bit dword UD 32 4
- signed 8 bit byte SB 8 1
- signed 16 bit word SW 16 2
- signed 32 bit dword SD 32 4
- signed 64 bit value DD 64 8
- 64 bit double floating point DBL 64 8
- 32 bit double floating point FLT 32 4
- n characters string STR n n/8
