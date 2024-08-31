---
layout: post
title: Cardiac Surgery, Fluid Dynamics and the Fontan Procedure
---

Back in 2018, after finishing my BSc, I took on a project looking at a surgical procedure called the Fontan procedure. 

## Background
Children born with Hypoplastic Left Heart Syndrome have an underdeveloped left side of their heart. This means they struggle to pump oxygenated blood, and need to undergo surgery. 

![Image of the heart of a patient with Hypoplastic Left Heart Syndrome, licence: CC0](/images/Hlhs.jpg)

By Centers for Disease Control and Prevention, CC0, [Source](https://commons.wikimedia.org/w/index.php?curid=29525840)

There are a series of surgical procedures (Norwood procedure, Glenn procedure, Fontan procedure) when the end goal of converting the heart from a two pump system (1 pump to push unoxygenated blood through the lungs, and 1 pump to push the oxygenated blood around the rest of the body) into a 1 pump system (1 pump to push oxygenated blood round the body and through the lungs before returning to the heart). 

To do this, the Inferior Vena Cava and Superior Vena Cava need to be attached to the pulmonary artery to redirect unoxygenated blood from the body towards the lungs. But there are a lot of options with how there are attached (relative positions, angles). 

![Picture of post Fontan (our physiological Fontan), licence: CC-BY](/images/physiologicalFontan.jpg)

Licence: CC-BY, [Source](https://doi.org/10.3389/fped.2019.00196).

## The project
We wanted to look at what we’re the best positions and angles to connect the IVC and SVC to the pulmonary artery, which we evaluated with computational fluid dynamics. 

We looked at two different metrics. The most important metric was energy loss. The heart is only able to provide 1 burst or energy/increase in pressure, and the blood will continuously lose energy as it travels around the body and through the lungs, increasing the strain on the heart, which if left unmanaged will lead to heart failure. So if we can improve the energy loss through the IVC-SVC-PA system, we can improve patient outcomes. 

The other outcome to consider is the distribution of the blood from the IVC, and how much goes left vs right. This is less important but it is necessary to ensure there is some mixing of the blood from the liver (which comes through the IVC). 

So, onto the fluid dynamics. The first thing to do is define a computational mesh. In simple terms, this is just the area where the fluid can go. We won’t focus too much on it here, it’s some work in SOLIDWORKS, and defining mesh resolutions. 

The next aspect to look at are the fluid inlets. We used fixed velocity inlets, with a parabolic profile (done with some custom code; hopefully there are better solutions for this now). The parabolic profile is a common thing in fluid dynamics as it’s the steady state profile for fluid in a pipe obeying the No slip condition. We looked at a variation on this doing a sinusoidal flow rate, a bit more realistic but much more complicated to deal with and data was quite limited, so we ended up deciding against going this way. 

Finally, the last thing to consider are the outlets. These are on the pulmonary artery, separated onto left and right pulmonary artery. We ended up going with a simple choice of fixed pressure, but went through a few variations before we ended up there. 

The first version was a simple 0D model of the lungs. This type of model emulates the flow of blood through the lungs as if it were an electronic circuit. We began by treating the lungs as a simple resistor, forcing a particular pressure drop. 

We then made this slightly more complicated, including a capacitor in the circuit, emulating compliance (or stretch of the blood vessels in the lung). However, when looking at measures of compliance for the lungs, the pressure at the outlet remained mostly fixed. 

Each of these 0D models specified the outlet pressure based on the flux of fluid through that outlet (all hand coded). 

![CFD simulations, licence: CC-BY](/images/cfdSim.jpg)

Licence: CC-BY, [Source](https://doi.org/10.3389/fped.2019.00196).

## Results
We started the work with the idea that the right lung is larger, so directing the IVC flow (more flow than SVC) towards it would be beneficial. To include this in the model, we would need a 0D model of the lungs. But we didn't end up needing this. It was actually beneficial directing the IVC flow in this way purely from the increase in curvature of the SVC (attaching SVC at a gentle angle rather than perpendicularly). 

## Post-mortem
This was the project that got me into mathematical biology, so it’s difficult for me to look at it objectively. But if I try, it still seems like a pretty nice project. I think the biggest weaknesses are probably the idealised geometry (which isn’t necessarily a good representation of the average) and the lack of pulsation flow. It’s nice that it gives a positive result even while reminding conservative (don’t even need to consider the lung size, which only strengthens our point). 

This seems to have become a good area for research since we worked on this. There is currently a group in the US that do patient specific optimisation of the Fontan procedure, using similar ideas. 

[Link to paper](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC6543709/)
