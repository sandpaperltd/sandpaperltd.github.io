---
title: "2024-10-25 shitfuck - how it's made?"
date: 2024-10-25
layout: post
---

okay. we need to address the elephant in the room - shitfuck. what's that and how it's made? que the discovery channel jingle. <br />

shitfuck (or, as mentioned in one of the previous development blogs - shtfck) consists of two main parts - flightstick and omnithrottle. and while most (most? just like xkcd 2501 - average familiarity i may overcompensate the general knowledge in this topic and i'll try to avoid that and sounding like an obnoxious buffon. or the obnoxious buffon) people may be familiar how the flightstick looks like, the omnithrottle may raise some questions. <br />

the flightstick within the shtfck project is (for now) two axis, gimbaled joystick-like contraption with handle mounted with linear and angular offset. i don't know how to add photos here (yet), so i won't share details, but you can research this on your own. sorry. i've never said it's gonna be fully open sourced. so from the beginning - two axis means that one has control in two planes - pitch (interpreted as up and down), and roll. for a moment i've considered designing addtional pedals for yaw control (left and right control) but decided against it. there are many designs for them and it did not spark joy within my heart. angular and linear offset of the handle in respect to the center of the gimbal (if you know how steadyrest for the camera software looks like, or the camera mount in uav looks like - that's gimbal, the only difference is that in case of flightsticks and omnithrottles it acts as an input, not stabilizing action) means that there's distance between center axis of handle and center of the assembly. reason behind that? ergonomics.  <br />

i understand it all may look a little chaotic, i see it two, but it's my second blog post so please, have some patience. <br />

omnithrottle. in simple terms it's like joystick but laying on the side and has three axis of rotation. similiar to the flightstick it has pitch and roll control (here interpreted as throttling up and down, as for the roll i'm not sure yet?) but also has secret third axis of rotation within the handle. <br />

the handle (called omnithrottle itself at the start of the project, then theta, then phi because of the iso 80000-2) can rotate. i absolutely adore axissymmetric assemblies. <br />

together all of this makes hotas system. or hosas. hands on throttle and stick or hands on stick and stick. it means that you need only those two things to fly plane in the kerbal space program. <br />

of course i know nothing here talks about how it's made per se. so what? you need cad software, ecad software, 3d printer, soldering iron, nuts, bolts, springs, linear rails, retainer rings, screws of various length and diameter, sensor, cables, connectors, software, mounting points. if you know what you want to build and and tools are required - that's it. what i have here is not a tutorial. maybe i'll try to explain different parts of the system, maybe not. i don't know. i can't answer if i'll work on this project next month. i had two months hiatus between rev-d-ish of phi section of the omnithrottle and now. almost a year between first revision of the flight and second. i need some slack sometimes. <br />
