# How to Debug CT Voltage Reference matchup on a kWh meter

<div align="left"><figure><img src="../.gitbook/assets/ecosuite-logo-full.svg" alt="" width="288"><figcaption></figcaption></figure></div>

_v2026.06.18_

***

## Overview

This document describes how to determine which phases of a 3-phase circuit that CTs are wrapped around correspond to the voltage references on a kWh meter.

**Relevant Roles for this document:**

* Asset Manager
* Database Administrator

{% hint style="warning" %}
**Electricians ONLY.** Do not attempt this process unless you are a certified electrician. You also need a Linux Engineer remoted into the Edge Compute Node via SolarSSH.
{% endhint %}

***

## Resources needed

When doing this, all the standard electrical tools are helpful:

* Appropriate PPE - flashsuit, goggles, gloves etc.
* Multimeter
* Clamp meter
* Fluke meter
* SolarSSH access to the Edge Compute Node operated by a remote Linux Engineer
* Mobile phone with video chat application
* kWh meter manual
* kWh meter password
* CT/Rogowski Coil manual

***

## Context

Often due to mislabelling of the actual conductors at the building, CTs get attached to the wrong conductor as far as the voltage reference is concerned. You might see negative watts values, or incorrect watts value measurements and weird powerFactor readings. The reason for this is usually **not** that the meter is malfunctioning but that the CTs are attached to conductors in a way that does not align with the voltage references connected to the meter, and that some or all of the CTs are attached in the "wrong" direction in terms of measuring current flow from source to destination. This is where you need to try a set of permutations to identify that the right CT is around the right conductor.

***

## Topology

\
The setup requires physical access to the kWh meter in question, and someone watching values on the Edge Compute Node logfile in realtime using the Support Laptop.

<figure><img src="../.gitbook/assets/How to Debug CT.drawio (4).svg" alt=""><figcaption></figcaption></figure>

***

## Step By Step

{% stepper %}
{% step %}
## Step 1

Label each of the 3 CTs in the following manner:

* 1, 2 and 3 where the rogowski coil section is
* 1, 2 and 3 where the ferruled ends are

So you are sure that the ferruled ends match the rogowski coil.
{% endstep %}

{% step %}
## Step 2

Label each of the Voltage reference wires going to the meter so that they correspond to the physical conductor they are coming from.

* A
* B
* C
{% endstep %}

{% step %}
## Step 3

Label each of the conductors the CTs are wrapped around

* A
* B
* C

And then record WHICH CT is physically attached around that conductor

| Conductor | CT |
| --------- | -- |
| A         | 1  |
| B         | 2  |
| C         | 3  |
{% endstep %}

{% step %}
## Step 4

Make sure the arrows on the Rogowski coil are pointed in the right direction to measure a positive current for the direction of flow. Often the arrows point toward the LOAD which means the building equipment loads when the meter is measuring loads, and towards the GRID when you are measuring a Solar PV system's generation. Check with the Rogowski coil's manufacturer and manual.
{% endstep %}

{% step %}
## Step 5

Change the terminals on the meter to each of the permutations below and record the following properties:

* **watts**: make sure it is a positive value and of the corresponding scale to what you expect from clamp meter measurements of current at the power supply voltage (ballpark check)
* **powerFactor**: it should be close to but a decimal value just under 1
* **reactivePower**: it will be non-zero but should be a smaller fraction corresponding to the powerFactor

### Permutations

The following is the checklist of values to record - the most "sane" set of data will be the likely mapping of voltage reference to CT assignment.

| Configuration | CT Terminals A | CT Terminals B | CT Terminals C | watts | powerFactor | reactivePower |
| :-----------: | :------------: | :------------: | :------------: | :---: | ----------- | ------------- |
|       1       |        1       |        2       |        3       |       |             |               |
|       2       |        1       |        3       |        2       |       |             |               |
|       3       |        2       |        1       |        3       |       |             |               |
|       4       |        2       |        3       |        1       |       |             |               |
|       5       |        3       |        1       |        2       |       |             |               |
|       6       |        3       |        2       |        1       |       |             |               |
{% endstep %}
{% endstepper %}
