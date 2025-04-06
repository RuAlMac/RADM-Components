# RADM-Components
This repo contains the part schematic and footprint files I use with my Altium projects.

## Generic vs. Non-Generic Parts
This library includes two categories of parts: generic and non-generic. A generic part is generally a passive component that may have a variety of size package options and physical properties. A generic part has one schematic symbol, but may have multiple PCB footprints. The user selects which PCB footprint to use based on what size the generic component used is (e.g., placing a `[Cap] SMD Nonpolarized` that uses a 1210 part package). Generic parts included in this library are listed below.

- `[BJT] SMD NPN`
- `[Cap] SMD Nonpolarized`
- `[Cap] SMD Polarized`
- `[Cap] SMD Polarized, Electrolytic`
- `[Cap] Thru-Hole Nonpolarized`
- `[Cap] Thru-Hole Polarized`
- `[Diode] SMD Rectifier`
- `[Diode] SMD Schottky`
- `[Diode] Thru-Hole Schottky`
- `[Diode] TVS Diode`
- `[DIP IC] 14pin` (include IC and IC socket footprint)
- `[DIP IC] 16pin` (include IC and IC socket footprint)
- `[DIP IC] 24pin` (include IC and IC socket footprint)
- `[Inductor] SMD Inductor`
- `[Inductor] SMD Varistor`
- `[Res] Axial Resistor`
- `[Res] SMD Resistor`

A non-generic part is a part that has a distinct PCB footprint (i.e., there are no other parts that (1) do exactly the same thing as this part and (2) fit in the exact same footprint as this part). Most parts in this library are non-generic (e.g., `[Device] Raspberry Pi Zero 2W`).

## Schematic.schlib: Conventions
The Schematics.schlib file contains the schematic symbols for the parts in my library. The following are requirements and guidelines for how the names and properties of each schematic symbol should be configured.

### Design Item ID (Part name)
A part's Design Item ID is essentially its part name. When a user goes to place a part from their list of components, the Design Item IDs are what they see. To help with placing parts, each part should be named as `[<category>] <Part name>`. 

The first part of this format is the part category. A part's category defines what kind of component it is (e.g., capacitor, resistor, etc.). A list of part categories included in this library is provided below. New categories can be created if needed.

| Type | Description |
|------|-------------|
|7seg  |A seven-segment display.|
|BJT   |BJT transistors.|
|Cap   |Capacitors.     |
|Device|Another PCB or small device that plugs into the PCB being designed.|
|Diode |Diodes.|
|DIP IC|A standard DIP IC package. Pin pitch is 2.54mm. Includes footprints of the ICs themselves or the IC sockets.|
|Ferrite|Ferrite beads.|
|FET   |Field-Effect Transistors.|
|Fuse  |Fuses.|
|IC    |Integrated Circuits. Non-DIP packages.|
|Inductor |Inductors|
|JTS   |JST Connectors. Different pitch options.|
|Jump  |Jumpers on PCB.|
|LED   |LED indicators, lamps, etc.|
|MCU   |Microprocessor ICs.
|Port  |Standardized connectors.|
|PTC   |Positive Temperature Coefficient (temperature-dependent, resettable) fuses.|
|Res   |Resistors.|
|Screw Terminal |Screw terminals. Different pitch options.|
|Switch |Physical switches.|
|Transformer | On-board transformers.|
|XTAL  | External crystal oscillator.|


The second part of this format is the part name. This name should be short and meaningful. If appropriate, the part name should include "thru-hole" or "SMD" to indicate if a certain part is a through-hole component or a surface mount component. If there are multiple distinct parts that are functionally the same, then they can share the same name, but must include a `(1)`, `(2)`, etc. to avoid naming issues. For example:

`[IC] 5V Regulator (1)`\
`[IC] 5V Regulator (2)`

These two parts are both 5V Regulators, but have differnent footprints and typical application diagrams. They have similar Design Item IDs, but have different part numbers, indicated in their Comment and Description.

### Designators
The format for a certain part's designator is defined within its schematic symbol. A designator consists of two parts: type, and number. For example, the designator for a diode might be `D1`, where  `D` corresponds to the part type (diode) and `1` is a unique number identifying that specific diode on the PCB. When defining a designator for a part in this library, its designator must be `<type>?`. Use of `?` allows Altium to automatically assign different unique numbers for different parts with the same type. Part designator types generally follow the definitions defined [here](https://en.wikipedia.org/wiki/Reference_designator). However, a part's designator type can differ from what the document indicates it should be if a different type would help with clarity. A part's designator is visible on a schematic and on a PCB.

### Comments
A part's comment is visible on a schematic. For a generic part (e.g., `[CAP] SMD Nonpolarized` or `[Diode] Rectifier`), its comment should be left as "Generic" and can be hidden. However, for a non-generic part (e.g., `[IC] 5V Regulator (1)` or `[FET] N-Channel MOSFET (2)`), its comment should be its manufacturer part number (not a Mouser, Digikey, etc. reference number), and must be visible on the schematic.

### Description
A part's description is not visible on a schematic, but is visible alongside Design Item IDs in lists of parts when a user is placing a part. For a generic part, its description should be left as "Generic". For a non-generic part, its description should be its manufacturer part number, same as its comment.

### Parameters
A part can have parameters, which function as miscellaneous descriptors for it and can be used in scripting, simulations, and other complex tools in Altium. However, for this library, part parameters are only used to help with describing parts.

Generic parts need to have a couple parameters to help the user differentiate between similar parts. Common parameters generic parts should have include `Size Package` and `<unit value>`. Size package refers to the part package following inches or ANSI/ISO standards. For resistors and capacitors, possible size package options may include 1812, 1210, etc. For diodes and other parts with standardized part packages
