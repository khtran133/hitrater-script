# Battlefield V Hitrater Visualization & Analysis

Monte Carlo simulation and visualization of Battlefield V's weapons, to provide analysis and feedback of balance.

## Introduction:

The “Hitrater” project was done with conjunction with Shan Lin, a former technical designer at EA DICE, and has two primary components - a Monte Carlo simulation created in the Unity game engine, which outputs spreadsheets in `.csv` format, and a script written in R, turning the spreadsheets into easily consumed charts - and aims to visualize weapon performance.

The “Hitrater” project was done for EA DICE’s *Battlefield V*, as a means of providing the most objective analysis possible and providing clear and actionable feedback to the developers. 

As a first-person shooter, the most important variable to measure here is damage output - how quickly a weapon can kill an enemy player - and the “Hitrater” project measures this in “frames”. As the game operates on a 60Hz logic loop, it would be disingenuous to represent this through seconds, or milliseconds, despite the fact that these are measures of times that humans tend to think in. 

Within each weapon type, damage models tend to be constant; each assault rifle has the same damage per bullet at each range, as would each machine gun. Due to this fact, a common misconception is that the faster firing weapon would always reign supreme. A major goal of the “Hitrater” project is to clear this misconception, through visualizing the effects of randomly-induced inaccuracy.

While damage models and rates of fire have an effect on damage output, “spread”, a mechanic introduced to emulate a real-life weapon’s minute-of-angle (MoA) accuracy, is used, alongside horizontal recoil.  Simulating these effects required reverse-engineering the game’s code from the game’s files.

### Spread: 

“Spread” creates a “cone” emanating from the weapon’s barrel, within which a bullet may go. When “zoomed”, bullets fired will be evenly distributed along the radius of the “spread cone”, giving “zoomed fire” a central bias. 

When “unzoomed”, bullets fired have a uniform distribution throughout the area of the cone. 

### Horizontal Recoil: 
Horizontal recoil is perhaps the largest limitation to accuracy; when firing, weapons will kick randomly left and right, with a uniform distribution. As weapons fire with an average of 100 milliseconds, or 6 frames, in between shots, it is impossible to compensate for this effect. This mechanic will constantly shift the yaw of the “spread cone” while firing.

These randomly-induced effects upon accuracy scale with rate of fire; spread increase and recoil have a positive relationship with rate of fire. Despite the fact that the 540 round-per-minute  (“RPM”) German MP 40 fires more slowly than the 770 RPM Finnish Suomi KP/-31, while having an identical damage per bullet, it will be the superior weapon past roughly 25 meters, as it is significantly more likely to land the necessary bullets to kill before its Finnish counterpart. 

![image text](https://imgur.com/ksSRwBN.png)

![image text](https://imgur.com/Lhiwx1r.png)

A further explanation of mechanics may be found on the [Symthic website](https://sym.gg/), the leading community forum for in-depth explanations of the *Battlefield* franchise's mechanics. I was also a significant contributor to the Symthic website, and assisted in creating visuals and textual explanations of game mechanics. 

### Simulation:

The “Hitrater” measures these effects of inaccuracy through a Monte Carlo simulation. A set of ranges are designated (e.g. 5, 10, 15, 20 meters, etc.), and each gun is fired 100,000 times at each range to measure the overall effects of spread and horizontal recoil. While a bullet missing or a gun failing to kill is a binary event, the weapon design is ultimately deterministic. With a low number of samples, an unacceptable amount of bias is induced, and 100,000 samples per range was the minimum number of samples necessary to eliminate sampling bias. 

The “Hitrater” then shows the upper-bound of possible player performance at each range, with each weapon. While it disregards humanly-controllable ease-of-use factors, such as vertical recoil (which is fixed) or bullet velocity (which determines how much a player will need to lead their target), it presents a perfect image of weapon performance that players may strive towards.

In conclusion, this project not only aims to demystify the game’s mechanics to its community, but it also aims to provide meaningful quantitative feedback to the game’s developers. The results of the “Hitrater” project are interpreted with actual gameplay in mind, and present suggestions as to which weapons may be under or overperforming.

### How to Interpret the Charts:

**Data Overview:**
* The charts show the data obtained from 100,000 samples of 15 round bursts across a variety of ranges.
* If a gun does not have 15 rounds in the magazine, it assumes a burst length equal to magazine size.

**Chart Overview and Assumptions:**
* Each picture has four charts which are concatenated into one.
* The top two charts are for aimed down sights fire at center mass, and the bottom two are for hipfire at center mass\*.
* The left two charts are using the “left-side” customization specializations, generally geared towards close-quarters combat.
* The right two charts are using the “right-side” customization specializations, generally geared towards long-ranged combat.
* As the hitrater is a simulation, it assumes perfect control of vertical recoil and recoil patterns, which are player-controlled variables that can be mastered.
* \*Medium machine gun (“MMG”) charts show “zoomed” bipod (aimed-down-sights [“ADS”] while bipoded) on the ADS charts, and “zoomed hipfire” (ADS from the hip) on the hipfire charts.
* An MMG’s “unzoomed hipfire” essentially cannot kill at all (less than a 0.0001% probability of killing a 100 health target at 5 meters), and is useless data.

**Variables / Guide:**
* FTK: Frames to kill a full-health target at the given range. To get TTK (time-to-kill) in milliseconds, multiply this value by 16.66. FTK is represented in colors, and designated on the right side by a color legend.
* E[FTK]: Expected frames to kill. This is a value factoring in both the average frames to kill and the probability of the 15 round burst killing the simulated target.
* U[FTK]: Average frames to kill. A mean value of all the instances where the gun managed to kill the simulated target.
* Frequency: The number of times the gun killed the simulated target, out of the 100,000 samples.

## History:

The “Hitrater” project began its inception shortly following *Battlefield V’s* Closed Alphas, and was introduced to the community with the game’s Beta. Below are several key updates covered by the “Hitrater” project.

### Beta:

In the Beta, the light machine guns and assault rifles required four bullets to kill to 11 meters, five bullets to 49 meters, and six bullets onwards. This made them ideal for medium-range combat. However, the light machine guns had too much horizontal recoil to be truly competitive, and the Sturmgewehr 44 (“StG 44”) was a better choice in nearly every scenario.

The submachine guns featured required four bullets to kill to 11 meters, five bullets to 24 meters, six bullets to kill to 49 meters, seven bullets to kill to 69 meters, and eight bullets onwards. While superior handling allowed them to remain relevant in close quarters, their anemic damage and relatively low accuracy prevented them from being competitive on *Battlefield V’s* large maps.

The semi-automatic rifles were incredibly dominant in the Beta. With no spread or horizontal recoil, the accuracy of these weapons were only limited by the player’s input, allowing for incredible damage output at range. While their performance at close range (11 meters and below) was not competitive, *Battlefield V’s* frequent long sightlines often exceeding 100 meters allow players to frequently avoid close-quarters and dominate the more frequent medium and long-range engagements. 

### Launch:

As expected, the launch of *Battlefield V* introduced more weapons and more refinements to weapon balancing. 
Due to a community overreaction, the StG 44 and its faster-firing cousin, the Sturmgewehr 1-5 (“StG 1-5”), were heavily nerfed, and could no longer kill in four bullets up close. Along with their relatively high rates of fire, this made them very uncompetitive at all ranges, as they did not have leading performance at any range.

Due to their relatively anemic performance in the Beta, light machine guns received a buff to horizontal recoil. This allowed the SIG KE7 to supercede the StG 44 as the versatile weapon of choice. It performed almost identically to the StG 44 did in the game’s Beta.

The semi-automatic rifles received rate-of-fire improvements, making them even more overpowering at medium and long ranges. In hindsight, my analysis of these weapons was too narrow in the game’s Beta, as I did not consider how difficult these weapons were to use for the majority of players. Most players are incapable of aiming well enough or clicking their mouse quickly enough to reap the full benefits of semi-automatic rifles. 

Submachine guns received no change, and continued to be underwhelming.

### December 2018 Time-to-Kill Changes:

In December 2018, developer EA DICE introduced a sweeping time-to-kill change, adding a 0.85x damage multiplier to the majority of guns. The developer claimed that this change was implemented as an experimental method in dealing with networking issues, where players felt that they were dying too quickly. However, the complex nature of networking in online multiplayer games means that time-to-kill and time-to-death are often independent, and extending one does not necessarily result in the extension of the other. 

The “Hitrater” analysis of this change resulted in a large community uproar, where the developer withdrew changes within two weeks of implementation. The time-to-kill change not only failed to address the developer’s stated goals, but it aggravated issues within weapon balance. Increasing the necessary bullets-to-kill compounds with weapons’ increasing levels of “spread” with sustained fire, and made faster-firing weapons overly dominant in close-quarters, while unnecessarily hurting the performance of other weapons.

### January 2019 “*Lightning Strikes*” Update

In conjunction with a previous update that extended the five bullet-to-kill range for submachine guns to 30 meters, “*Lightning Strikes*” heavily improved the viability for these weapons by improving “unzoomed” or “hipfire” accuracy. With “hipfire” allowing competitive performance to 15 to 20 meters (coinciding with 25th percentile and 50th percentile engagement ranges, according to telemetry), and the new five bullet-to-kill range allowing submachine guns to be competitive to 30 meters (equivalent to 75th percentile engagement ranges), submachine guns were made to be far more competitive, and could adequately compete with other weapon classes. 
The four bullet-to-kill ranges for the StG 44 and StG 1-5 were previously restored, and with minor changes to horizontal recoil and various ease-of-use factors (e.g. reload time, vertical recoil, bullet velocity) for other weapons, weapon balance for *Battlefield V* was in a good place. Semi-automatic rifles were still incredibly overpowering at range, but as aforementioned, they were not problematic in the hands of the majority of the playerbase.

### December 2019 Time-to-Kill Changes

In December 2019, EA DICE introduced another sweeping time-to-kill increase, akin to their poorly-received attempt from the previous year, displaying an incredible lack of community awareness. *Battlefield V* was already the only game within the franchise to not reach the top 10 highest-selling games of its respective launch year, and poor gameplay design choices and poor hardware optimization further degrading player retention. The game’s “gunplay” was universally regarded to be the only enjoyable aspect of the game. Despite having initial imbalances, the weapon balance for *Battlefield V* steadily shaped itself into an industry-leading state, and players universally enjoyed the satisfying recoil and lethality of the weapons. 

Here, the “Hitrater” project aimed to dispute the misinformation spread by the developer, where it was claimed that a significant increase in necessary bullets-to-kill would not affect the overall time-to-kill, despite a significant decrease in recoil. The project provided an objective analysis of the changes to the community, and proved the developer’s assertions to be incorrect. 

While weapon balance in the preceding November 2019 “*Pacific*” Update could be objectively considered to be the best weapon balance of any first-person-shooter, the December 2019 Update reversed the work of the leading weapon designer, and resulted in the destruction of the game’s sole redeeming quality.

## Developer Feedback:

“Very good content and a great analysis (and with mostly good points considered). Some of your comments are matching my expectations… Very keen to see how the data compares on our end as [I’m] sure we will be making a bunch of tweaks in the future!”

“It’s difficult to get a good opinion without having all of the elements presented at first. It’s also not very easy for the majority of players to get a good understanding of the intricate gunplay systems (that) come into play to measure how a given weapon performs.”

\- Florian le Bihan, Core Gameplay Designer at EA DICE

“[The Hitrater project] is probably the best analysis of [*Battlefield V’s*] weapon design. It really shows a complete understanding of the game’s mechanics, and displays them in an easy to understand format. This is the definitive way to visualize weapon performance in Battlefield.”

\- Julian Schimek, Lead Weapons Designer at EA DICE

“I wish I had this sooner.”

\- Shan Lin, (Former) Technical Designer at EA DICE

“It’s an outstanding post”

\- Adam Freeman, Community Manager at EA DICE

## Limitations:

The Unity simulation itself presents a technical limitation; the simulation is unable to output data in a manner that would allow the R script to analyze distribution and variance properly. Variance in particular is important, as it is a measure of consistency in hitting best-case FTK - average and expected values cannot tell the whole story.

A more qualitative limitation lies in how to measure consistency itself - *what level of consistency* is valued, and at what point is a weapon consistent enough to outperform what the expected values suggest? 

For example, in the November 2019 “*Pacific*” Update, the 514 RPM Ribeyrolles and the 770 RPM M1907 had the same expected FTK at 50 meters. I would consider the Ribeyrolles to be the superior weapon at this range, as it is more consistent and is considerably easier to use, and the M1907 is more likely to kill slower than the Ribeyrolles at this range. However, there is also a significant probability of the M1907 killing *faster* than the Ribeyrolles at this range as well. The value of weapon consistency may be subjective from player to player; how willing should a player be to trade a chance of a faster time-to-kill for a higher lower bound of performance and greater ease-of-use? It is difficult to quantify the distribution of FTK instances at each range in a meaningful manner, as different players will always have different priorities. 

![image text](https://imgur.com/hlQjVNh.png)

![image text](https://imgur.com/sfKOaYy.png)

The next step in the “Hitrater” project would be in the development of a web-based tool, ideally with either Power BI or RShiny. An accessible and interactive tool where users may customize the visualization and easily sort through various weapons without having to resort to shuffling through dozens of `.png` files would be a considerable improvement.
