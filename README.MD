# Experimental Analysis of Heart Measurements

> *This is an attempt to remove some of the noise from the
> measurements that medical devices give, and to come to
> something a little more consistent and meaningful.*

**This is not common medical practice.**  Rather, this is a
nerd's and a model maker's response to doctors shrugging and
saying "these figures tend to be all over the place, we cannot
really deduce meaningful conclusions from them".  Technicians
would apply noise reduction techniques to filter out the signal,
but the medical practice seems to miss that opportunity.

**Keep a diary.** The best use of this information is to keep
a diary describing measurement values and a short description
of how you feel.  You can then look back at these values and
see if they relate to anything you found.  (The program should
maybe read back such diaries from a text file that would be
formatted as `[DATE[TIME]\t]SYS\tDIA\tBPM\tNOTES` lines.)


## Systems Model of a Pounding Heart

Heart pressure drops at an exponential rate, if we may
believe [the graphs](https://en.wikipedia.org/wiki/Blood_pressure)
and what seems to be common sense in a system without memory
or further non-linearities.

I am not a medical doctor, but have some grasp of physiology
[Guyton and Hall, Textbook of Medical Physiology]
and a thorough understanding of analog electronics,
regulation circuits and the design of distributed protocols.
Translating the former to the latter is really helpful
to allow meaningful calculations, but it is important to be
cautious and only make translations when they match as a
structure.  This follows the approach of
[systems theory](https://en.wikipedia.org/wiki/Systems_theory)
rather than mere
[metaphors](https://en.wikipedia.org/wiki/Metaphor)
that are only safe for purposes like explanation or inspiration.

The pounding heart maps to electronics as a discharging
[RC circuit](https://en.wikipedia.org/wiki/RC_circuit);
its capacitor receives an initial charge *Q* that is output
from the heart's chamber when it pumps, which leads to an
initial voltage *U_sys*, causing a current *I* to flow through
the blood circulation *R*.  The voltage drops until *U_dia*, at
which point the next charge *Q* is added and the game begins
anew.  The frequency *F_bpm* marks how often *Q* is added,
and is measured in beats per minute rather than per second.


## Average Blood Pressure

The average blood pressure is to be taken over a dropping
exponential curve.  This can be done by mapping it to a
logarithmic domain, determining the average of the straight
line found there, and mapping back.  The result is

> P_avg = sqrt ( P_sys * P_dia )

This value will be as jumpy as your systolic and diastolic
pressures, of course.

**Interpretation.**
The body is full of capillaries that distribute blood to
the tissues surrounding them.  Small-enough molecules flow
in and out in a turbulant motion, but the effective flow
is caused by

 1. the average liquid pressure *P_avg* relative to the
    liquid pressure in the tissues;
 2. the average osmotic pressure relative to the osmotic
    pressure in the tissues.

You cannot measure most of these values, but a rising
*P_avg* may be caused by one of the other values.  The
blood that is effectively "lost" to the tissues is mopped
up by the lymph system and passed through sterilisation in
lymph nodes before it returns to the blood circulation.


## Pressure Halvation Frequency

The halvation frequency of blood pressure is fairly constant;
it depends on the capacity *C* and resistance *R* of the
blood circulation and the heart chamber capacity *Q* of
charging.  All these are reasonably constant, at least for
a given state of well-being.  The value of *R* may change
as arterioles open or close; these respond to the need for
more oxygen in nearby tissues, and so are a sign of cellular
activity (though fat and sugar are not burnt equally).

A normal blood pressure meter yields a high and low point,
and a repeat frequency.  The rise of blood pressure due to
a heart beat is fast, so we can probably ignore that and
consider the period of a heart beat to be solely devoted
to the exponential drop of the heart pressure.  The number
of halvations per minute now is

> F_hpm = F_bpm * 2log ( P_sys / P_dia )

This probably removes the noise present in systolic and
diastolic values and heart rate.  It therefore upgrades
the noisy raw measurements to a value that can be of
better practical use.

My values tend to be within a very tight range when I
feel well, and they seem to go well outside of that range
when I feel poorly.  I estimate 5-10 sigma's as an example.
This is stuff that can be really helpful during analysis.

**Interpretation.**
Changes in *F_hpm* are probably due to change in *R*,
the resistance of the blood circulation (including the
branch into the lymph system that returns to the blood
before the heart pumps it around again).  Although the
system branches out and then rejoins the branches, it
might be considered by looking at average parameters,
and otherwise considered an electronic resistor:

> R_avg = rho * l_avg / A_avg

Lengths being constant, the area *A_avg* is the most
interesting one to look at as a rising/lowering
factor.  Better still, diameter or circumference,
which as a factor would be the square root of the
former.

A useful presentation might be a percentage relative
to the lowest measured, assuming that to represent
a resting state.  Other reference points must be
selectable, because people always mess up, forgetting
that they are multiplications dressing up like
additions, so we there are unexpected properties like

> ( x + 10% ) - 10% = x - 1%

or

> (x + 100% ) - 100% = 0

