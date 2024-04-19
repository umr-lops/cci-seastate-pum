


### ERS-1

**Launch date** : July 17, 1991

---

**End of life date** : March 10, 2000

---

**Agency** : ESA

---

**Orbit** : 
- Orbit type : Sun-synchronous near-circular polar orbit
- Inclination : 98.5 degrees
- Altitude : 782 - 785km
- Nodal Period : approximately 100min (14,3 orbits per day)
- Repeat Cycle : 35 days

---

**Coverage cycle**:
- Reference Orbit (3-day cycle): used during the commissioning phase.
- Ice-Orbit (3-day cycle): similarity to the Reference Orbit, but with a slightly different longitudinal phase.
- Mapping-Orbit: (35-day cycle): cycle allowing complete coverage of the Earth.




### TOPEX

**Launch date** : August 10, 1992

---

**End of life date** : January 18, 2006

---

**Agency** : Cnes/NASA

---

**Orbit characteristics** : 
- Orbit type : Non-heliosynchronous (prograde orbit)
- Inclination : 66 degrees
- Altitude : 1336 km
- Nodal Period : approximately 112min 
- Repeat Cycle : 10 days

---

**Orbit life** :
- From August 2008 to September 2002 : in its original orbit.
- Orbit change in September 2002 : shifted to an orbit midway between its original tracks and those of Jason-1, forming a tandem phase.
- Tandem phase (2002) : operated in tandem with Jason-1, providing measurements from two altimeters on similar orbits with an equatorial separation of 158 km.
- Altitude maintened at 1336 km unntil mid-Septmeber 2002 and then adjusted to a new orbit.





### ERS-2

**Launch date** : April 21, 1995

---

**End of life date** : July 4, 2011

---

**Agency** : ESA

---

**Orbit** : 
- Orbit type : Sun-synchronous polar orbit (retrograde orbit)
- Inclination : 98.5 degrees
- Altitude : 780 km (mean)
- Nodal Period : approximately 100min (14,3 orbits per day)
- Repeat Cycle : 35 days
2000




### GFO (GEOSAT Follow-ON)

**Launch date** : February 10, 1998

---

**End of life date** : November 26, 2008

---

**Agency** : US Navy

---

**Orbit characteristics** : 
- Orbit type : Non-sun-synchronous polar orbit 
- Inclination : 108 degrees
- Altitude : 800 km
- Nodal Period : 101 minutes
- Repeat Cycle : 17 days

---

**Orbit life** :
- From February 1998 to November 2008 : in its original orbit.






### JASON-1

**Launch date** : December 7, 2001

---

**End of life date** : July 1, 2013

---

**Agency** : Cnes/NASA

---

**Orbit characteristics** : 
- Orbit type : Non-heliosynchronous (prograde orbit)
- Inclination : 66 degrees
- Altitude : 1336 km
- Nodal Period : approximately 112min 
- Repeat Cycle : 10 days

---

**Orbit life** :
- Same orbit as Topex/Poseidon
- Orbit change in 2009: At the end of the OSTM/Jason-2 calibration phase in February 2009, the orbit of Jason-1 was changed to be positioned between its original tracks.
- Orbit Reduction in 2012: Due to an anomaly in February-March 2012, Jason-1 was placed in Safe Hold mode, followed by maneuvers to reduce orbit.





### JASON-2

**Launch date** : June 20, 2008

---

**End of life date** : October 10, 2019

---

**Agency** : Cnes/NASA

---

**Orbit characteristics** : 
- Orbit type : Non-heliosynchronous (prograde orbit)
- Inclination : 66 degrees
- Altitude : 1336 km
- Nodal Period : approximately 112min 
- Repeat Cycle : 10 days

---

**Orbit life** :
- From June 2008 to October 2016 : in its original orbit.
- Orbit shifted in October 2016 to join the intercalated orbit previously followed by Topex (2002-2005) and Jason-1 (2009-2012)
- Orbit change in July 2017 : Jason-2 was placed in a lower orbit at around 1309km, called LRO (Long Repeat Orbit).
- Since July 2018, it has been operating in an interspersed orbit called i-LRO (interleaved Long Repeat Orbit).





### JASON-3

**Launch date** : January 17, 2016

---

**End of life date** : Active mission

---

**Agency** : Cnes/NASA

---

**Orbit characteristics** : 
- Orbit type : Non-heliosynchronous (prograde orbit)
- Inclination : 66 degrees
- Altitude : 1336 km
- Nodal Period : approximately 112min 
- Repeat Cycle : 10 days

---

**Orbit life** :
- From January 2016 to April 2022 : in its original orbit.
- Orbit shifted in April 2022 to join the intercalated orbit previously followed by Topex (2002-2005), Jason-1 (2009-2012) and Jason-2 (2016-2017).





### JASON-3

**Launch date** : January 17, 2016

---

**End of life date** : Active mission

---

**Agency** : Cnes/NASA

---

**Orbit characteristics** : 
- Orbit type : Non-heliosynchronous (prograde orbit)
- Inclination : 66 degrees
- Altitude : 1336 km
- Nodal Period : approximately 112min 
- Repeat Cycle : 10 days

---

**Orbit life** :
- From January 2016 to April 2022 : in its original orbit.
- Orbit shifted in April 2022 to join the intercalated orbit previously followed by Topex (2002-2005), Jason-1 (2009-2012) and Jason-2 (2016-2017).





### ENVISAT
### CRYOSAT
### SARAL
### SENTINEL-3A
### SENTINEL-3B
### CFOSAT
### SENTINEL-6 MF


## Bibliographie

https://www.eoportal.org/satellite-missions

https://www.aviso.altimetry.fr/en/missions.html









### BATHYMETRY

Verification of bathymetry (GEBCO14Bathymetry class in ceraux).

We notice that there is a problem with negative longitudes at the L3 product level :



It turns out that the bathymetry display was in the -180°/180° convention, and that in the 0°/360° convention, the problem was resolved :



Updated GEBCO14Bathymetry class : put longitudes between 0° and 360°.

Implement a new development for the new version of GEBCO (in the -180°/180° convention this time).
Creation of the _depth_2023 method in the GEBCO14Bathymetry class which returns the bathymetry values (from the most recent GEBCO file : 2023, which has different dimensions compared to the old GEBCO files)
