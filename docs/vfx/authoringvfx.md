# Authoring VFX

In this section you will not find recipes but instead advice and methods to comprehend and be able to set up your own effects by yourself. Treat this as a compilation of techniques and workflow elements that you can mix together in various cases.

## Asking the good questions

So, what’s the method in order to find out what’s best for the effect that you will be working on?

<u>Factors for decision can include:</u>

- Is the effect is Gameplay? Cinematic?
- Is it intended for strong feedback? Or totally cosmetic?
- Will It be concurrently played along with other effects?
- Will there have multiple occurrences at the same time?
- Will It be played into a performance-demanding gameplay sequence?

Asking ourselves these kind of questions will start inducing constraints on the effect, but will also remove other constraints.

For instance, a visual effect that will have to run within a cinematic, (e.g. a gunshot) will have exactly the same point of view since It is filmed at a fixed Point of View.  Meanwhile, you will have to make sure randomness is locked or slight in order that your effect looks the same every time the cinematic will be played.

## Effects and their underlying intents: Feedback

During your journey making effect you will have to work in a meaningful way and carefully think about what effects you will create and their intent. Something true about creating a visual, is it has always an effect on the spectator. So you better prepare yourself for a reaction to you effects as they will act as feedback to the player in every situation.

Feedback are the main intent of any visual effect, just as sound effects, light effects, post-processes, etc. If you do correctly the job, every effect you’ll design will be targeted at something specific : It can vary from gameplay feedbacks such as Low-Life, negative or positive effects on player or enemies, level design hints to the player, to guide him or repel him, or even cosmetic effects that will give personality to a place that’s lacking some life.

Reactions can vary depending on what kind of effect you are doing : from wow-effect to scary stuff, or even cool stuff, the initial intent is not what will drive the reaction of the player. This being said, you will often be misled into adding cool VFX stuff in places where your effect will ruin the situation’s intent so think carefully before putting anything confusing in the scenes. If any doubt, ask a level designer about the idea.



##### An example : Fire on critical path.

A common example that explains a lot is the way to add fire on the critical path of the player : placing burning stuff on the critical path of the player will often trigger a reaction of careful advance in the level, because of the threat level of the flames (unless proven harmless in some tutorial of course). Fire will be always considered as a threat, and that's just cultural.

<video loop="true" autoplay="true" ><source  src="../img/fireblock.mp4" type='video/mp4' /></video>

> in this example, the fire is too intense and bleeds on the critical path of the player, inducing a negative feedback : "Stay away"

So the question here is what kind of fire do I put if I do not want to pose a threat to the player, still give him an impression of stuff burning (or that have already been burnt). I could not think about having everything burning around him except the critical path, because of the credibility of the situation, if there is fire in a zone, you don't expect to have a safe corridor. So you would just add smaller flames, cindered burning debris that does expel some flying, and burning ashes.

At some point, you will have to reduce the burn intensity of your flames to make them fainter, but still present.

<video loop="true" autoplay="true" ><source  src="../img/firenoblock.mp4" type='video/mp4' /></video>

> in this example, the fire is faint and scarse on the ground so It does convey less negative feedback, depending on the realism of the game, this can be assumed as harmless.

However sometimes, it is not possible to avoid the presence of a strong fire, so the intensity of the fire is not only our only option in this example. The main issue in the first example is that the fire is gushing towards the critical path and blocks the way.

<video loop="true" autoplay="true" ><source  src="../img/fireaway.mp4" type='video/mp4' /></video>

> By adjusting the flux of the flames and directing it towards the wall, the critical path now becomes clear. In order to reinforce the feeling of danger and the credibility, we also added flying specs that fly a bit towards the critical path. But their size is conveying a harmless feedback.

## When Feedback becomes a Code

When a feedback becomes repeated enough during a whole game, it becomes a rule of understanding situations : consciously or unconciously, the player will learn that this feedback is associated with a kind of situation.

Often, the feedback is not necessarily the same but instead embeds a common denominator such as a color, a symbol, a shape or even a particular motion. The good example of Code is Mirror's edge color codifications for critical elements along the running path (Doors, pipes,…) that becomes the convention in the game and help reading the environment by just assuming this rule.

At some point you will find a common denominator in your effects and know how to use them in a wise manner in order to signify something to the player.

##### Another example : Level Design callers:

Be careful by using this kinds of effects and try to tend to use them in safe places or place them cleverly: for instance, if the player has some collectible to find in one dark corridor barely visible from the room you’re in, place some sparkles and flashes right inside the corridor. This could imply the following:

- There’s a corridor plunged in obscurity
- Maybe there’s something hidden out there.
- The sparkles and the electric surges could explain the power outage in this corridor.
- If the sparkles are slight enough, this is not a dangerous place. If power surges are quite violent, this could be a dangerous place.

The last statement will be determinant on how the player will apprehend the situation. So if you don’t want your player to avoid the place, by fear of being electrocuted (even though that won’t happen if (s)he’s going close to the power surges), you will have to balance the intensity of the flashes, electric arcs and sparkles, so it’s quiet enough to tell this is safe passage, but visible enough to call the player from a distance.

## Methods and recipes

### The 3-3-3 Rule

As a rule of designing effects you tend to sort out things in order to put them into play. Sorting out things enable you to create visual hierarchy of what’s important in your effect and what’s less important. In terms of priority I’d like to correct this statement : things that are considered less important are in fact an improvement and the sublimation of what you consider important.

There’s what you can call the 3-3-3 rule when designing effect that comes handy in terms of reminder of variety and richness of the effects : 3 sizes, 3 speeds, 3 rhythms

| **Size** | **Speed** | **Rhythm**    |
| -------- | --------- | ------------- |
| Small    | Fast      | Jerky, Scarce |
| Medium   | Moderate  | Syncopated    |
| Large    | Slow      | Continuous    |

When designing effects, these three categories (Size,Speed,Rhythm) will help you in order to provide richness into your effect.

<u>Additional 3-rules :</u>

- Color (based on saturation, hue, …)
- Brightness (dark, dim, bright )
- Life Time (Short, Medium, Long)

### A Matter of rhythm

When designing effects you will require good animation skills in order to provide good motion richness to the effect. Motion in VFX can be described in same terms as music : as a rhythm, velocity, attenuation, decay, attack, and so on.

#### Emission Rhythm: Anticipation, Attack and Decay

The best example to describe this in action is to think about how to make an explosion and what are its components. First we will have the initial blow of the explosion that have to be really quick & intense. For this case you have two choices : either spawning all of your explosion blobs at once [REF] (preferably for a dry explosion such as C4), spawning them as a upping stream [REF] (for instance a fuel tank explosion, the explosion emits a upping fireball), or a succession of a few random explosions [REF] (for instance, a building containing various explosives).

These three cases comes with their own rhythm : the first one is a really quick blow that needs to be proceeded really quick, let’s say a “BAH’!”. The second will be set as a more mellow explosion with an intense flash at startup and rolling fire for about one second, let’s say a “BHA’RRRHHHGHFRHR”. The third example will be a more complex rhythm, let’s say a “BA’DA’...BRLAH...DABA’DAH”

If you try to practice saying these kinds of rhythms you can easily picture some common patterns in explosions, but not only in effects in general. A good exercise is, when doing reference hunting sessions, is to try to figure out the rhythm behind the effect. By whispering onomatopes while playing over and over the reference, you acquire the rhytmic essence of the effect.

It’s basically the same thing you want to do when designing effects. Once you start emitting particles, find a way to replay your effect easily (Unreal Engine has this useful space key that you can use to replay over and over your effect). This way you can adjust timing for your effect and improve rhythm to make it richer.

Making the rhythm richer will require you to polish the delays, the durations and the discontinuities of the emission to match the intent. Here is a table containing common properties and adjectives that can describe your intent.

| **Intent**                    | **Rhythm**                                                   |
| ----------------------------- | ------------------------------------------------------------ |
| Erratic, broken, heterogenous | Syncopated, random bursts. Random Delays between sets of bursts. |
| Quick, intense, swift         | Single burst, instantaneous                                  |
| Imprevisible, broken          | Syncopated bursts on top a continous flow with variation along time. |

#### Similarities with ADSR Sound Envelopes

As these notions come mainly from music, there is some vocabulary that has been taken from this domain. Most of these refer to how a [Sound Envelope](https://en.wikipedia.org/wiki/Synthesizer#ADSR_envelope) can be described.

![Sound Envelope + anticipation]()

Here are some elements that we commonly use when making effects:

* **Anticipation** : The premises of your effect. When you can, it is advised to anticipate an effect to draw the attention of the viewer before it happens. Especially in critical gameplay phases that require player perception. The anticipation can happen at least 0.25s before the effect happens if you need to make it noticeable for the player. If you need a reaction from him, you can add 0.4s (set it to at least 0.65s) 

  These are timings way shorter than - for example - a driver's perception-reaction time (which is 1.5s). So the timings are the least ones. If the player has to react with more complexity you will need to increase these timings.

  Anticipation is not possible at all times because of gameplay constraints, so It is often difficult to provide a good anticipation, or the timeframe we have to make an anticipation is really short.

  

* **Attack (and Decay)** : The rhythmic shape of the start of your effect. By combining rhythmed bursts and softer (but quick) emission you can create an attack pattern rich and delightful. Like in music, an attack can be swift, hard, or in the contrary smooth. If the attack is powerful, the following phase that will calm things down is called the **Decay** : most of the time it is the transition between the attack and the **Sustain** phase

* **Sustain** : How your effect will sustain itself over time once activated. It can be constant, vary at different rates, use a constant or a variable rhythmic. This is quite important to have a rich sustain, especially if your effects are present multiple times in the scene, so not every instance look like its neighbors too much. 

* **Release** : The dying state of an effect. When the effect has to be turned off, sometimes it is a good idea to add extra dynamics to emphasize on the termination of the effect. For instance : for a flamethrower it is good to have a "valve shut" extra effect when the effect terminates, by adding smoke or a small burst of fire when the effect stops, it helps enrich the sensation of mechanical device that shuts down the valve.

#### Duration & Motion

Alongside rhythm and emission, particles have to behave accordingly to their intent. For instance, the fire resulting of a gasoline explosion with plume smoke will be emitting a stream of fireball smoke particles that will rise upwards at a quite constant speed.

For a C4 explosions, debris will be ejected around with a very fast initial momentum and will tend to be subject to drag, and gravity, depending on the size and the type of the debris. This very fast initial motion that tends to slow down is really important in order to emphasize on the initial blow that needs to be really strong but lasts not long at all. You can mix & match with bigger debris that will tend to have slower initial momentum

#### Genericity

It is really important in order for your effects to have an identity throughout their life. Using these kind of codes your effect will be identifiable even if the player did not notice the start, and playable using multiples instances in a short time to avoid the impression of déja-vu.

Déja-vu happens often when something odd is happening in the effect or that stands out too much : "once seen, you can't un-see it" and it can ruin the experience.

Through pushing effort into genericity for repeating effects you make sure your effect will work without stealing all the attention because of unnecessary repeating visual itches.

### Foreground and Background

Readability of an image is something that can prove extremely useful when your art direction relies intensively on detail and high-frequency elements. Visual Effects are part of this equation and serve a role in terms of space boundaries. 

