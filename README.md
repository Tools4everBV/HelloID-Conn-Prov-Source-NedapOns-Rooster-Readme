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

- [Introduction](#introduction)
- [Getting started](#getting-started)
  - [Connection settings](#connection-settings)
  - [Prerequisites](#prerequisites)
    - [Custom Fields]](#custom-fields)
  - [Remarks](#remarks)
    - [Shift assignments](#shift-assignments)
    - [Planned visits](#planned-visits)
    - [Custom fields in the primary source connector](#custom-fields-in-the-primary-source-connector)
    - [Primary Contract calculation, Aggregation and Exclusions](#primary-contract-calculation-aggregation-and-exclusions)
    - [No function data available](#no-function-data-available)
- [Getting help](#getting-help)
- [HelloID Docs](#helloid-docs)

## Introduction

_HelloID-Conn-Prov-Source-NedapOns-Rooster_ is a _source_ connector. It utilizes a set of REST API's that allow you to programmatically interact with Roster information in NedapONS
It collects the Employees from nedap, including their shiftassignments and/or their planned visits

The HelloID connector uses the API endpoints listed in the table below.

| Endpoint     | Description |
| ------------ | ----------- |
| ./t/employees/x-stream-connect/data             | The list of employees  |
| ./t/moves/shift_assignments/starting_between    | The list of shift assigments  |
| ./t/employees/{employee_id}/planned_visits | The list of planned visits for an employee |
| ./t/teams/x-stream-connect/data |The list of teams, to lookup the information for the teams |

## Getting started

### Connection settings

The following settings are required to connect to the API.

| Setting                         | Description                                                                                                 |
| ------------------------------- | ----------------------------------------------------------------------------------------------------------- |
| Environment URL API             | <https://api-staging.ons.io>                                                                                  |
| Certificate (.PFX) Path         | Full path to Certificate Nedap-cert.pfx                                                                    |
| Certificate Password            | Password of the certificate                                                                                 |
| Start Day                       | The number of days in the past to start collecting data. Assignments or visits with an earlier startdate will not be collected |
| Total number of days collected  | Assignments or vistits with a startdate later than this number of days after the "start day", will not be collected |
| CollectShiftAssignments         | Collect the Shift Assignments (toggle)                                                                   |
| CollectPlannedVisits            | Collect the planned visits (toggle)

### Prerequisites

#### Custom fields

The following custom fields should be created before importing the mapping:

- Contract field: [primarySource]

- Contract field: [sourceSystem]

- Contract field: [NedapOnsIdentificationNo] with a combination of the employeeCode and EmploymentCode.

Example:

 ```javascript
  function getValue() {
      return sourceContract.PersonCode + "-" + sourceContract.EmploymentCode
  }
  getValue();
 ```

### Remarks

#### Shift assignments

- Importing of shift assignments can be enabled by setting the configuration toggle CollectShiftAssignments
- Each shift assignment of an employee results in a contract where the field "T4eType" has the value "NedapShiftAssignment".
- Shift assignments for each employee are collected, for startdates as specified in de configuration.
- Warning: Collecting Shift assignments may take a considerable time if the "total number of days collected" is large, as it requires a separate Api call for each hour in the period. Default setting is therefore 3 days.

#### Planned visits

- Importing of planned visits can be enabled by setting the configuration toggle CollectShiftAssignments
- Planned visits are also collected for the period containing Yesterday, today and tomorrow.
- Warning: Collecting planned visits can take a considerable time, as there is one call per user required, which may exceed allowed HelloID limits for larger numbers of emloyees.

#### Custom fields in the primary source connector

- Add the field Custom.primarySource to the primary source connector (ie AFAS) and set the type to fixed, value 1
- Add a the field Custom.sourceSystem to the primary source connector (ie AFAS) and set the type to fixed, value <NameOfSourceConnector> ie: AFAS

#### Primary Contract calculation, Aggregation and Exclusions

- The primary contract calculation should be changed, so that Custom.primarySource is the first custom field in the list, sorted descending. This makes sure that the person records of the primary source connector are always primary.
- The connector is configured to aggregate the person data based on the user's employeeId. A prefix and suffix are added to each employeeId. The aggregation script must be set up in the primary source system before importing the Nedap Ons Roster source. When using employeeId as the aggregation key, suggestions should be disabled.
- Aggregation can also be set up based on birth date, gender, birth name, and initials if this information is recorded in Nedap and matches the data in the primary source system.
- By default, all person records from Nedap Ons Rooster are excluded. When a person is aggregated, their contracts from Nedap Ons Rooster are added to the corresponding person in the primary source system. Person records which exist only in the Nedap Ons Rooster source system are added to the HelloID person database but remain excluded from business rule enforcement.
- Please note that the pre-offboarding notification will still trigger, even if the person is excluded.

#### No function data available

- Since function data is not available, business rules can only be created based on department data. It is not possible to create business rules that combine department and function data.
- The mapping includes a fixed function with ExternalId N01 and the name NedapOnsRooster.

## Getting help

> _For more information on how to configure a HelloID PowerShell connector, please refer to our [documentation](https://docs.helloid.com/hc/en-us/articles/360012557600-Configure-a-custom-PowerShell-source-system) pages_
> _If you need help, feel free to ask questions on our [forum](https://forum.helloid.com)_

## HelloID docs

The official HelloID documentation can be found at: <https://docs.helloid.com/>
