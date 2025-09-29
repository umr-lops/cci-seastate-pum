---
jupytext:
  formats: md:myst
  text_representation:
    extension: .md
    format_name: myst
mystnb:
  execution_timeout: 360
---

(l2p)=
# L2P

The altimeter L2P products are along-track files, usually corresponding to a 
satellite pass, processed from each mission data provider’s L1B product 
(usually referred to as SGDR) containing the waveforms. A retracking 
algorithm is applied to retrieve significant wave height from the instrument 
waveforms.
