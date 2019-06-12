---
description: >-
  Code Complete: A Practical Handbook of Software Construction, Second Edition
  by Steve McConnell
---

# Code Complete

#### \( Currently still reading \)

## Introduction

Software construction takes up 65% of software development. It makes sense to do it well.

Metaphors are useful heuristics as long as you treat them like clowns and don't overextend them.

Resource: [Kuhn, Thomas S.The Structure of Scientific Revolutions](https://projektintegracija.pravo.hr/_download/repository/Kuhn_Structure_of_Scientific_Revolutions.pdf)

Some managers are WIMPs \(Why isn't Mary programming?\). Upstream tasks like planning is crucial to a project's success.

Measure twice, cut once.

_Researchers at Hewlett-Packard, IBM, Hughes Aircraft, TRW, and other organizations have found that purging an error by the beginning of construction allows rework to be done 10 to 100 times less expensively than when it’s done in the last part of the pro-cess, during system test or after release \(Fagan 1976; Humphrey, Snyder, and Willis 1991; Leffingwell 1997; Willis et al.1998; Grady 1999; Shull et al.2002; Boehm and Turner 2004\)._

Different good practices for different kinds of software

_Studies at IBM and other companies have found that the average project experiences about a 25 percent change in requirements during devel-opment \(Boehm 1981, Jones 1994, Jones 2000\), which accounts for 70 to 85 percent of the rework on a typical project \(Leffingwell 1997, Wiegers 2003\)._

\_\_

## Architecture

Good software architecture makes construction easy. Bad architecture makes construction impossible.

#### Things to consider:

**Architectural components**: Program organization \(high level overview of how the program's puzzle pieces work together\)

**Major classes** \(responsibility of each class and how it interacts with other classes\)

**Data design** \(modelling describing files and table designs to be used\)

**Business rules** \(identify such rules and how they impact the design\)

**UI design** \(web page format/ GUI/ CLI, separation of concerns from other parts\)

**Resource management** \(for scarce resources like db connections, threads and handles. Memory for embedded systems. \)

**Security** \(code level + design level. Guidelines for handling buffers, untrusted data \(like user input, cookies, config data and external interfaces\), encryption, level of details in error messages, protecting secret data in memory, etc

**Performance** \(speed vs memory vs cost. Custom algorithms/ ways to meet performance goals. \)

**Scalability** \(how the software adapts to growth: no of users, servers, network nodes, db records, transaction volume, etc\)

**Interoperability** \(how the software shares data/ resources with other software/ hardware if that's required\)

**Internationalization** \(I18n,l10n. Especially for interactive systems due to amount of prompts. \)

**I/O** \(look-ahead/look-behind/ just in time reading scheme, error detection\)

**Error processing** \(corrective/ detective, active/ passive detection, error propagation, error messages, error handling\)

**Fault tolerance** \(recovering from errors: back up and try again, auxiliary code, voting algorithm, replace with fake/ default value

**Architectural feasibility** \(spikes/ prototypes on feasibility before large scale construction\)

**Overengineering** \(In software, the chain isn’t as strong as its weakest link; it’s as weak as all the weak links multiplied together. Err on the side of caution.\)

**Buy vs Build** \(Alot of components can be bought off the shelf. If building custom components, it should have benefits over 'store-bought'.\)

**Reuse decisions** \(conforming pre-existing software to specs\)

**Change strategy** \(version numbers in data files, reserve fields for future use, or design files so that you can add new tables, delay commitments, etc\)

**General architectural quality** \(In The Mythical Man-Month, the central thesis is that the essential problem with large systems is maintaining their conceptual integrity \(Brooks 1995\). Avoid duct tape solutions.

Be wary of 'we've always done it that way'.

## Key construction decisions:

### Choosing programming language:

Programmers are 30% faster in a language they are familiar with \(3+ years experience\).

Using higher level languages is more productive.

Define a programming convention to minimize unneeded complexities.

Programming in a language vs programming into a language.

### Decide programming practices based on your project.

Design in construction 

\(to be continued...\)

