# HelloID-Conn-Prov-Source-NedapOns-Rooster-Readme


| :warning: Private repository |
|:---------------------------|
| This is a private repository due to the complexity of the connector and requirements such as Tools4ever's generated certificate |

| :information_source: Information |
|:---------------------------|
| This repository contains the connector and configuration code only. The implementer is responsible to acquire the connection details such as username, password, certificate, etc. You might even need to sign a contract or agreement with the supplier before implementing this connector. Please contact the client's application manager to coordinate the connector requirements. |

<p align="center">
  <img src="https://user-images.githubusercontent.com/68013812/94918899-c672c700-04b3-11eb-9132-7125bbf77fa5.png"
   alt="drawing" style="width:300px;"/>
</p>

## Table of contents

- [Introduction](#Introduction)
- [Getting started](#Getting-started)
  + [Connection settings](#Connection-settings)
  + [Prerequisites](#Prerequisites)
  + [Remarks](#Remarks)
- [Setup the connector](@Setup-The-Connector)
- [Getting help](#Getting-help)
- [HelloID Docs](#HelloID-docs)

## Introduction

_HelloID-Conn-Prov-Source-NedapOns-Rooster_ is a _source_ connector. It utilizes a set of REST API's that allow you to programmatically interact with Roster information in NedapONS
It collects the Employees from nedap, including their shiftassignments and/or their planned visits


The HelloID connector uses the API endpoints listed in the table below.


| Endpoint     | Description |
| ------------ | ----------- |
| ./t/employees/x-stream-connect/data             | The list of employees  |
| ./t/moves/shift_assignments/starting_between    | The list of shift assigments  |
| ./t/employees/{employee_id}/planned_visits | The list of planned visits for an employee |
| ./t/teams/x-stream-connect/data |The list of teams, to lookup the team name |

## Getting started

### Connection settings

The following settings are required to connect to the API.

| Setting                         | Description                                                                                                 |
| ------------------------------- | ----------------------------------------------------------------------------------------------------------- |
| Environment URL API             | https://api-staging.ons.io                                                                                  |
| Certificate (.PFX) Path         | Full path to Certificate Nedap-cert.pfx                                                                    |
| Certificate Password            | Password of the certificate                                                                                 |
| CollectShiftAssignments         | Collect the Shift Assignments (toggle)                                                                   |
| CollectPlannedVisits            | Collect the planned visits (toggle)

### Prerequisites

### Remarks
 Each shift assignment of an employee results in a contract where the field "T4eType" has the value "NedapShiftAssignment".
 Each planned visit of an employee results in a contract where the field "T4eType" has the value "NedapPlannedVisit"

 ShiftsAssignments for each employee are collected, for startdates of  Yesterday, today and tomorrow only.

 Planned visits are also collected for the period containing Yesterday, today and tomorrow.

 Warning: Collecting planned visits can take a considerable time, wich may exceed allowed HelloID limits for larger numbers of emloyees.

## Getting help

> _For more information on how to configure a HelloID PowerShell connector, please refer to our [documentation](https://docs.helloid.com/hc/en-us/articles/360012557600-Configure-a-custom-PowerShell-source-system) pages_

> _If you need help, feel free to ask questions on our [forum](https://forum.helloid.com)_

## HelloID docs

The official HelloID documentation can be found at: https://docs.helloid.com/
