+++
title = "Overview"
date = 2022-07-11T16:26:38-07:00
weight = 1
chapter = true
+++

### Chapter 1

# What is OSSTD-PRM?

The purpose of this documentation is to serve as a comprehensive reference for OSSTD-PRM, a tool that can facilitate the process of complying with ASHRAE Standard 90.1. It can help eliminate unintentional errors, intentional "cheating" and allow the modelers to spend more time and project budget evaluating energy-efficiency design and operation strategies. 

<!-- Reference: https://openstudio.net/ and https://github.com/NREL/openstudio-standards -->
**OpenStudio Standards (OSSTD)** is a Ruby Gem library that extends the OpenStudio SDK. It is a collection of measures and resources that automate the construction of typical building models in OpenStudio format, provide higher level methods to create OpenStudio models from existing geometry as well as the transformations associated with *ASHRAE Standard 90.1 Appendix G Performance Rating Method (PRM)*, a commonly exercised transformation that supports code compliance, green certifications, and utility incentive calculations. 

**OSSTD-PRM** is an umbrella term or a package created to automate the generation of a code baseline model (and potentially a proposed model in future) from an existing geometry. It currently includes *“Create ASHRAE 90.1-2019 PRM Model”* measure to automatically generate the baseline model according to the rules of the 2019 version of the ASHRAE 90.1 Appendix G Performance Rating Method. 

The following sections provide a. An *Introduction* to ASHRAE 90.1 Appendix G (PRM) that OSSTD-PRM is based on along with the benefits and use-cases of OSSTD-PRM, b. *Getting Started* resources and c. the *User Guide*.