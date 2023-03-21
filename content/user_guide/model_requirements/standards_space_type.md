---
title: "Standards space type"
date: 2022-09-21T15:05:37-07:00
weight: 312
draft: false
pre: "<b>- </b>"
---
## Standards Space Type

The SpaceType object in OpenStudio has the property StandardsSpaceType which can be used to identify the appropriate lighting space type. The lighting space type is used to determine the baseline lighting power density and lighting control savings based on Table G3.7 of 90.1 Appendix G. The lighting space type is also used for a number of other purposes in the baseline generation, including: determining special requirements for computer rooms and laboratories, identifying residential spaces, and determining applicability of receptacle control measures according to space type.

The lighting space type can also be assigned using the User Compliance Data files for [Space Type](/BEM-for-PRM/user_guide/add_compliance_data/user_data_space_type) and [Space](/BEM-for-PRM/user_guide/add_compliance_data/user_data_space). If the User Compliance Data files contain lighting space type assignments, there is a hierachy for determining the final assignments:

1. user_data_space is first priority
2. user_data_space_type is second priority
3. StandardsSpaceType property of the SpaceType objest is lowest priority

It is possible to have lighting space type identified at the user_data_space level for some spaces and at either the user_data_space_type level or SpaceType:StandardsSpaceType level for other spaces.

Valid entries for StandardsSpaceType are listed below. If the "whole building" options are used, the baseline model will not get credit for occupancy control of lighting.

| StandardsSpaceType |
| --------------------------------------- |
| apartment - hardwired |
| audience seating - exercise center |
| audience seating - gymnasium |
| audience seating - sports arena |
| audience seating - transportation facility |
| audience seating - convention center |
| audience seating - penitentiary |
| audience seating - motion picture theater |
| audience seating - religious facility |
| audience seating - performing arts theater |
| audience seating - auditorium |
| audience seating - all other |
| atrium > 40 ft height |
| atrium <= 40 ft height |
| banking activity |
| classroom/lecture/training - penitentiary |
| classroom/lecture/training - preschool to 12th |
| classroom - laboratory or shop |
| classroom/lecture/training - all other |
| conference/meeting/multipurpose |
| confinement cells |
| copy/print |
| corridor - manufacturing facility |
| corridor - all other |
| corridor - hospital |
| corridor for visually impaired |
| courtroom |
| computer room |
| dining - penitentiary |
| dining - bar/lounge/leisure |
| dining - family |
| dining for visually impaired |
| dining - cafeteria/fast food |
| dining - all other |
| electrical/mechanical |
| emergency vehicle garage |
| kitchen |
| guest room |
| judges chambers |
| laboratory |
| laundry/washing |
| loading dock |
| lobby - motion picture theater |
| lobby - hotel |
| lobby - performing arts theater |
| lobby for visually impaired |
| lobby - elevator |
| lobby - all other |
| locker room |
| lounge/breakroom - healthcare facility |
| lounge/breakroom - all other |
| office - enclosed <= 250 sf |
| office - open |
| office - open with workstation occup sensors |
| parking area, interior |
| pharmacy |
| restroom - visually impaired |
| restroom - all other |
| sales |
| seating area, general |
| stairwell |
| storage < 50 sf - hospital |
| storage 50 to 1000 sf - hospital |
| storage < 50 sf - all other |
| storage 50 to 1000 sf - all other |
| storage >= 1000 sf - all other |
| vehicular maintenance |
| workshop |
| chapel - visually impaired |
| recreation/common living - visually impaired |
| automotive |
| exhibit - convention center |
| dormitory - living quarters |
| firestation - sleeping quarters |
| gymnasium exercise area |
| gymnasium playing area |
| emergency room |
| exam/treatment |
| medical supply |
| nursery |
| nurses station |
| operating room |
| patient room |
| physical therapy |
| recovery |
| library - reading |
| library - stacks |
| detailed manufacuring |
| manufacturing equipment room |
| manufacturing extra high bay |
| manufacturing high bay |
| manufacturing low bay |
| museum exhibit area |
| museum restoration |
| post office - sorting area |
| fellowship hall - religious facility |
| worship/pulpit/choir area |
| retail dressing room |
| retail mall concourse |
| sports arena playing area, class I |
| sports arena playing area, class II |
| sports arena playing area, class III |
| sports arena playing area, class IV |
| baggage/carousel |
| airport concourse |
| transportation ticket counter |
| warehouse - bulk storage |
| warehouse - fine storage |
| attic - unoccupied |
| elevator core |
| plenum |
| automotive facility - whole building |
| convention center - whole building |
| courthouse - whole building |
| dining: bar lounge/leisure - whole building |
| dining: cafeteria/fast food - whole building |
| dining: family - whole building |
| dormitory - whole building |
| exercise center - whole building |
| fire station - whole building |
| gymnasium - whole building |
| health-care clinic - whole building |
| hospital - whole building |
| hotel/motel - whole building |
| library - whole building |
| manufacturing facility - whole building |
| motion picture theater - whole building |
| multifamily - whole building |
| museum - whole building |
| office - whole building |
| parking garage - whole building |
| penitentiary - whole building |
| performing arts theater - whole building |
| police station - whole building |
| post office - whole building |
| religious facility - whole building |
| retail - whole building |
| school/university - whole building |
| sports arena - whole building |
| town hall - whole building |
| transportation - whole building |
| warehouse - whole building |
| workshop - whole building |


