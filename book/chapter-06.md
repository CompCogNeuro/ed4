# Chapter 6: Perception and Attention

Perception is at once obvious and mysterious. It is so effortless to us that we have little appreciation for all the amazing computation that goes on under the hood. And yet we often use terms like "vision" as a metaphor for higher-level concepts (does the President have a vision or not?) -- perhaps this actually reflects a deep truth: that much of our higher-level cognitive abilities depend upon our perceptual processing systems for doing a lot of the hard work. Perception is not the mere act of seeing, but is leveraged whenever we imagine new ideas, solutions to hard problems, etc. Many of our most innovative scientists (e.g., Einstein, Richard Feynman) used visual reasoning processes to come up with their greatest insights. Einstein tried to visualize catching up to a speeding ray of light (in addition to trains stretching and contracting in interesting ways), and one of Feynman's major contributions was a means of visually diagramming complex mathematical operations in quantum physics.

Pedagogically, perception serves as the foundation for our entry into cognitive phenomena. It is the most well-studied and biologically grounded of the cognitive domains. As a result, we will cover only a small fraction of the many fascinating phenomena of perception, focusing mostly on vision. But we do focus on a core set of issues that capture many of the general principles behind other perceptual phenomena.

We begin with a computational model of **primary visual cortex (V1)**, which shows how self-organizing learning principles can explain the origin of oriented edge detectors, which capture the dominant statistical regularities present in natural images. This model also shows how excitatory lateral connections can result in the development of **topography** in V1 -- neighboring neurons tend to encode similar features, because they have a tendency to activate each other, and learning is determined by activity.

Building on the features learned in V1, we explore how higher levels of the **ventral what** pathway can learn to **recognize objects** regardless of considerable variability in the superficial appearance of these objects as they project onto the retina. Object recognition is the paradigmatic example of how a hierarchically-organized sequence of feature category detectors can incrementally solve a very difficult overall problem. Computational models based on this principle can exhibit high levels of object recognition performance on realistic visual images, and thus provide a compelling suggestion that this is likely how the brain solves this problem as well.

Next, we consider the role of the **dorsal where** (or how) pathway in **spatial attention**. Spatial attention is important for many things, including object recognition when there are multiple objects in view -- it helps focus processing on one of the objects, while degrading the activity of features associated with the other objects, reducing potential confusion. Our computational model of this interaction between what and where processing streams can account for the effects of brain damage to the *where* pathway, giving rise to hemispatial neglect for damage to only one side of the brain, and a phenomenon called Balint's syndrome with bilateral damage. This ability to account for both neurologically intact and brain damaged behavior is a powerful advantage of using neurally-based models.

As usual, we begin with a review of the biological systems involved in perception.

## Biology of Perception

Our goal in this section is to understand just enough about the biology to get an overall sense of how information flows through the visual system, and the basic facts about how different parts of the system operate. This will serve to situate the models that come later, which provide a much more complete picture of each step of information processing.

![**Figure 6.1:** The pathway of early visual processing from the retina through lateral geniculate nucleus of the thalamus (LGN) to primary visual cortex (V1), showing how information from the different visual fields (left vs. right) are routed to the opposite hemisphere.](figures/fig_vis_optic_path.png){ width=40% }

![**Figure 6.2:** How the retina compresses information by only responding to areas of contrasting illumination, not solid uniform illumination. The response properties of retinal cells can be summarized by these Difference-of-Gaussian (DoG) filters, with a narrow central region and a wider surround (also called center-surround receptive fields). The excitatory and inhibitory components exactly cancel when both are uniformly illuminated, but when light falls more on the center vs. the surround (or vice-versa), they respond, as illustrated with an edge where illumination transitions between darker and lighter.](figures/fig_on_off_rfields_edge.png){ width=50% }

![**Figure 6.3:** A V1 simple cell that detects an oriented edge of contrast in the image, by receiving from a line of LGN on-center cells aligned along the edge. The LGN cells will fire due to the differential excitation vs. inhibition they receive (see previous figure), and then will activate the V1 neuron that receives from them.](figures/fig_v1_simple_dog_edge.png){ width=30% }

Figure 6.1 shows the basic optics and transmission pathways of visual signals, which come in through the retina, and progress to the lateral geniculate nucleus of the thalamus (LGN), and then to primary visual cortex (V1). The primary organizing principles at work here, and in other perceptual modalities and perceptual areas more generally, are:

* **Transduction of different information** -- in the retina, photoreceptors are sensitive to different **wavelengths of light** (red = long wavelengths, green = medium wavelengths, and blue = short wavelengths), giving us color vision, but the retinal signals also differ in their **spatial frequency** (how coarse or fine of a feature they detect -- photoreceptors in the central *fovea* region can have high spatial frequency = fine resolution, while those in the periphery are lower resolution), and in their **temporal response** (fast vs. slow responding, including differential sensitivity to motion).
    
* **Organization of information in a topographic fashion** -- for example, the left vs. right visual fields are organized into the contralateral hemispheres of cortex -- as the figure shows, signals from the left part of visual space are routed to the right hemisphere, and vice-versa. Information within LGN and V1 is also organized topographically in various ways. This organization generally allows similar information to be contrasted, producing an enhanced signal, and also grouped together to simplify processing at higher levels.
    
* **Extracting relevant signals, while filtering irrelevant ones** -- Figure 6.2 shows how retinal cells respond only to **contrast**, not uniform illumination, by using **center-surround** receptive fields (e.g., on-center, off-surround, or vice-versa). Only when one part of this receptive field gets different amounts of light compared to the others do these neurons respond. Typically this arises with edges of contrast, where illumination transitions between light and dark, as shown in the figure -- these transitions are the most informative aspects of an image, while regions of constant illumination can be safely ignored. Figure 6.3 shows how these center-surround signals (which are present in the LGN as well) can be integrated together in V1 simple cells to detect the orientation of these edges -- these edge detectors form the basic vocabulary for describing images in V1. It should be easy to see how more complex shapes can then be constructed from these basic line/edge elements. V1 also contains complex cells that build upon the simple cell responses (Figure 6.4), providing a somewhat richer basic vocabulary.

The following videos show how we know what these receptive fields look like:

* Classic Hubel & Wiesel V1 receptive field mapping using old school projector stimuli: [YouTube Video](https://www.youtube.com/watch?v=KE952yueVLA)
    
* Newer reverse correlation V1 receptive field mapping: [YouTube Video](https://www.youtube.com/watch?v=n31XBMSSSpI)

![**Figure 6.4:** Simple and complex cell types within V1 -- the complex cells integrate over the simple cell properties, including abstracting across the polarity (positions of the on vs. off coding regions), and creating larger receptive fields by integrating over multiple locations as well (the V1-Simple-Max cells are only doing this spatial integration). The end stop cells are the most complex, detecting any form of contrasting orientation adjacent to a given simple cell. In the simulator, the V1 simple cells are encoded more directly using gabor filters, which mathematically describe their oriented edge sensitivity.](figures/fig_v1_visual_filters_hypercols_basic.png){ width=50% }

In the auditory pathway, the cochlear membrane plays an analogous role to the retina, and it also has a topographic organization according to the frequency of sounds, producing the rough equivalent of a fourier transformation of sound into a spectrogram. This basic sound signal is then processed in auditory pathways to extract relevant patterns of sound over time, in much the same way as occurs in vision.

![**Figure 6.5:** Updated and simplified version of Felleman & Van Essen's (1991) diagram of the anatomical connectivity of visual processing pathways, starting with primary visual cortex (V1) and on up.  The blue-shaded areas comprise the ventral *What* pathway, and green-shaded are the dorsal *Where*.  Reproduced from Markov et al., (2014).](figures/fig_markov_et_al_cort_hier.png){ width=50% }

Moving up beyond the primary visual cortex, the perceptual system provides an excellent example of the power of hierarchically organized layers of neural detectors, as we discussed in the *Networks* Chapter. Figure 6.5 shows the anatomical connectivity patterns of all of the major visual areas, from V1 and on up [@MarkovVezoliChameauEtAl14; @FellemanVanEssen91]. The specific patterns of connectivity allow a hierarchical structure to be extracted, as shown, even though there are many interconnections outside of a strict hierarchy as well.

![**Figure 6.6:** Division of *What* vs *Where* (ventral vs. dorsal) pathways in visual processing, based on the classic Ungerlieder and Mishkin (1982) framework.](figures/fig_vis_system_bio.png){ width=50% }

Figure 6.6 puts these areas into their anatomical locations, showing more clearly a **what vs where** (**ventral vs dorsal**) split in visual processing [@UngerleiderMishkin82]. The projections going in a ventral direction from V1 to V4 to areas of **inferotemporal cortex (IT)** (TE, TEO, labeled as PIT for posterior IT in the previous figure) are important for recognizing the identity ("what") of objects in the visual input, while those going up through parietal cortex extract spatial ("where") information, including motion signals in area MT and MST. We will see later in this chapter how each of these visual streams of processing can function independently, and also interact together to solve important computational problems in perception.

Here is a quick summary of the flow of information up the **what** side of the visual pathway (pictured on the left side of Figure 6.5):

* **V1** -- primary visual cortex, which encodes the image in terms of oriented edge detectors that respond to edges (transitions in illumination) along different angles of orientation. We will see in the first simulation in this chapter how these edge detectors develop through self-organizing learning, driven by the reliable statistics of natural images.
    
* **V2** -- secondary visual cortex, which encodes combinations of edge detectors to develop a vocabulary of intersections and junctions, along with many other basic visual features (e.g., 3D depth selectivity, basic textures, etc), that provide the foundation for detecting more complex shapes. These V2 neurons also encode these features in a broader range of locations, starting a process that ends up with IT neurons being able to recognize an object regardless of where it appears in the visual field (i.e., *invariant* object recognition).
    
* **V4** -- detects more complex shape features, over an even larger range of locations (and sizes, angles, etc).

* **IT-posterior** (PIT) -- detects entire object shapes, over a wide range of locations, sizes, and angles. For example, there is an area near the fusiform gyrus on the bottom surface of the temporal lobe, called the **fusiform face area (FFA)**, that appears especially responsive to faces. As we saw in the *Networks* Chapter, however, objects are encoded in distributed representations over a broad range of areas in IT.
    
* **IT-anterior** (AIT) -- this is where visual information becomes extremely abstract and **semantic** in nature -- it can encode all manner of important information about different people, places and things.

In contrast, the **where** aspect of visual processing going up in a dorsal directly through the parietal cortex (areas MT, VIP, LIP, MST) contains areas that are important for processing motion, depth, and other spatial features.

## Oriented Edge Detectors in Primary Visual Cortex

![**Figure 6.7:** Orientation tuning of an individual V1 neuron in response to bar stimuli at different orientations -- this neuron shows a preference for vertically oriented stimuli.](figures/fig_v1_orientation_tuning_data.jpg){ width=50% }

![**Figure 6.8:** Topographic organization of oriented edge detectors in V1 -- neighboring regions of neurons have similar orientation tuning, as shown in this colorized map where different colors indicate orientation preference as shown in panel C. Panel B shows how a full 360 degree loop of orientations nucleate around a central point -- these are known as pinwheel structures.](figures/fig_v1_orientation_cols_data.jpg){ width=30% }

Neurons in primary visual cortex (V1) detect the orientation of edges or bars of light within their receptive field (RF -- the region of the visual field that a given neuron receives input from). Figure 6.7 shows characteristic data from electrophysiological recordings of an individual V1 neuron in response to oriented bars. This neuron responds maximally to the vertical orientation, with a graded fall off on either side of that. This is a very typical form of **tuning curve**. Figure 6.8 shows that these orientation tuned neurons are organized **topographically**, such that neighbors tend to encode similar orientations, and the orientation tuning varies fairly continuously over the surface of the cortex.

The question we attempt to address in this section is why such a topographical organization of oriented edge detectors would exist in primary visual cortex? There are multiple levels of answer to this question. At the most abstract level, these oriented edges are the basic constituents of the kinds of images that typically fall on our retinas. These are the most obvious **statistical regularities** of natural images [@OlshausenField96]. If this is indeed the case, then we would expect that the self-organizing aspect of the XCAL learning algorithm used in our models (as discussed in the *Learning* Chapter) would naturally extract these statistical regularities, providing another level of explanation: V1 represents oriented edge detectors because this is what the learning mechanisms will naturally develop.

The situation here is essentially equivalent to the self organizing learning model explored in the *Learning Chapter, which was exposed to horizontal and vertical lines, and learned to represent these strong statistical regularities in the environment.

However, that earlier simulation did nothing to address the topography of the V1 neurons -- why do neighbors tend to encode similar information? The answer we explore in the following simulation is that neighborhood-level connectivity can cause nearby neurons to tend to activate together, and because activity drives learning, this then causes them to tend to learn similar things.

### Simulation Exploration

![**Figure 6.9:** Topographic organization of oriented edge detectors in simulation of V1 neurons exposed to small windows of natural images (mountains, trees, etc). The neighborhood connectivity of neurons causes a topographic organization to develop.](figures/fig_v1rf_map.png){ width=50% }

Open `v1rf` in [CCN Sims](https://github.com/CompCogNeuro/sims) to explore the development of oriented edge detectors in V1. This model gets exposed to a set of natural images, and learns to encode oriented edges, because they are the statistical regularity present in these images. Figure 6.9 shows the resulting map of orientations that develops.

## Invariant Object Recognition in the *What* Pathway

Object recognition is the defining function of the ventral "what" pathway of visual processing: identifying what you are looking at. Neurons in the inferotemporal (IT) cortex can detect whole objects, such as faces, cars, etc, over a large region of visual space. This **spatial invariance** (where the neural response remains the same or invariant over spatial locations) is critical for effective behavior in the world -- objects can show up in all different locations, and we need to recognize them regardless of where they appear. Achieving this outcome is a very challenging process, one which has stumped artificial intelligence (AI) researchers for a long time -- in the early days of AI, the 1960's, it was optimistically thought that object recognition could be solved as a summer research project, and 50 years later we are making a lot of progress, but it remains unsolved in the sense that people are still much better than our models. Because our brains do object recognition effortlessly all the time, we do not really appreciate how hard of a problem it is.

![**Figure 6.10:** Why object recognition is hard: things that should be categorized as the same (i.e., have the same output label) often have no overlap in their retinal input features when they show up in different locations, sizes, etc, but things that should be categorized as different often have high levels of overlap when they show up in the same location. Thus, the bottom-up similarity structure is directly opposed to the desired output similarity structure, making the problem very difficult.](figures/fig_objrec_difficulty.png){ width=40% }

The reason object recognition is so hard is that there can often be no overlap at all among visual inputs of the same object in different locations (sizes, rotations, colors, etc), while there can be high levels of overlap among different objects in the same location (Figure 6.10). Therefore, you cannot rely on the bottom-up visual similarity structure -- instead it often works directly against the desired output categorization of these stimuli. As we saw in the *Learning* Chapter, successful learning in this situation requires error-driven learning, because self-organizing learning tends to be strongly driven by the input similarity structure.

![**Figure 6.11:** Schematic for how multiple levels of processing can result in invariant object recognition, where an object can be recognized at any location across the input. Each level of processing incrementally increases the featural complexity and spatial invariance of what it detects. Doing this incrementally allows the system to appropriately bind together features and their relationships, while also gradually building up overall spatial invariance.](figures/fig_invar_trans.png){ width=50% }

![**Figure 6.12:** Another way of representing the hierarchy of increasing featural complexity that arises over the areas of the ventral visual pathways. V1 has elementary feature detectors (oriented edges). Next, these are combined into junctions of lines in V2, followed by more complex visual features in V4. Individual faces are recognized at the next level in IT (even here multiple face units are active in graded proportion to how similar people look). Finally, at the highest level are important functional "semantic" categories that serve as a good basis for actions that one might take -- being able to develop such high level categories is critical for intelligent behavior -- this level corresponds to more anterior areas of IT.](figures/fig_category_hierarch_dist_reps.png){ width=100% }

The most successful approach to the object recognition problem, which was advocated initially in a model by [@Fukushima80], is to incrementally solve two problems over a hierarchically organized sequence of layers (Figures 6.11, 6.12):

* The invariance problem, by having each layer *integrate over a range of locations* (and sizes, rotations, etc) for the features in the previous layer, such that neurons become increasingly invariant as one moves up the hierarchy.
    
* The pattern discrimination problem (distinguishing an A from an F, for example), by having each layer build up more complex combinations of feature detectors, as a result of detecting *combinations of the features* present in the previous layer, such that neurons are better able to discriminate even similar input patterns as one moves up the hierarchy.

The critical insight from these models is that breaking these two problems down into incremental, hierarchical steps enables the system to solve both problems without one causing trouble for the other. For example, if you had a simple fully invariant vertical line detector that responded to a vertical line in any location, it would be impossible to know what spatial relationship this line has with other input features, and this relationship information is critical for distinguishing different objects (e.g., a T and L differ only in the relationship of the two line elements). So you cannot solve the invariance problem in one initial pass, and then try to solve the pattern discrimination problem on top of that. They must be interleaved, in an incremental fashion. Similarly, it would be completely impractical to attempt to recognize highly complex object patterns at each possible location in the visual input, and then just do spatial invariance integration over locations after that. There are way too many different objects to discriminate, and you'd have to learn about them anew in each different visual location. It is much more practical to incrementally build up a "part library" of visual features that are increasingly invariant, so that you can learn about complex objects only toward the top of the hierarchy, in a way that is already spatially invariant and thus only needs to be learned once.

![**Figure 6.13:** Summary of neural response properties in V2, V4, and IT for the macaque monkey, according to both the extent to which the areas respond to complex vs. simple visual features (Smax / MAX column, showing how the response to simple visual inputs (Smax) compares to the maximum response to any visual input image tested (MAX), and the overall size of the visual receptive field, over which the neurons exhibit relatively invariant responding to visual features. For V2, nearly all neurons responded maximally to simple stimuli, and the receptive field sizes were the smallest. For V4, only 50% of neurons had simple responses as their maximal response, and the receptive field sizes increase over V2. Posterior IT increases (slightly) on both dimensions, while anterior IT exhibits almost entirely complex featural responding and significantly larger receptive fields. These incremental increases in complexity and invariance (receptive field size) are exactly as predicted by the incremental computational solution to invariant object recognition as shown in the previous figure. Reproduced from Kobatake & Tanaka (1994).](figures/fig_kobatake_tanaka_94_invariance.png){ width=50% }

![**Figure 6.14:** Complex stimuli that evoked a maximal response from neurons in V2, V4, and IT, providing some suggestion for what kinds of complex features these neurons can detect. Most V2 neurons responded maximally to simple stimuli (oriented edges, not shown). Reproduced from Kobatake & Tanaka (1994).](figures/fig_kobatake_tanaka_94_v2_v4_it.png){ width=50% }

In a satisfying convergence of top-down computational motivation and bottom-up neuroscience data, this incremental, hierarchical solution provides a nice fit to the known properties of the visual areas along the ventral what pathway (V1, V2, V4, IT). Figure 6.13 summarizes neural recordings from these areas in the macaque monkey [@KobatakeTanaka94], and shows that neurons increase in the complexity of the stimuli that drive their responding, and the size of the receptive field over which they exhibit an invariant response to these stimuli, as one proceeds up the hierarchy of areas. Figure 6.14 shows example complex stimuli that evoked maximal responding in each of these areas, to give a sense of what kind of complex feature conjunctions these neurons can detect.

### Exploration of Object Recognition

![**Figure 6.15:** Set of 20 objects composed from horizontal and vertical line elements used for the object recognition simulation. By using a restricted set of visual feature elements, we can more easily understand how the model works, and also test for generalization to novel objects (object 18 and 19 are not trained initially, and then subsequently trained only in a relatively few locations -- learning there generalizes well to other locations).](figures/fig_objrec_objs.png){ width=30% }

Open up the `objrec` simulation in [CCN Sims](https://github.com/CompCogNeuro/sims) for the computational model of object recognition, which demonstrates the incremental hierarchical solution to the object recognition problem. We use a simplified set of "objects" (Figure 6.15) composed from vertical and horizontal line elements. This simplified set of visual features allows us to better understand how the model works, and also enables testing generalization to novel objects composed from these same sets of features. You will see that the model learns simpler combinations of line elements in area V4, and more complex combinations of features in IT, which are also invariant over the full receptive field. These IT representations are not identical to entire objects -- instead they represent an invariant distributed code for objects in terms of their constituent features. The generalization test shows how this distributed code can support rapid learning of new objects, as long as they share this set of features. Although they are likely much more complex and less well defined, it seems that a similar such vocabulary of visual shape features are learned in primate IT representations.

## Spatial Attention and Neglect in the *Where/How* Pathway

The dorsal visual pathway that goes into the parietal cortex is more heterogeneous in its functionality relative to the object recognition processing taking place in the ventral *what* pathway, which appears to be the primary function of that pathway. Originally, the dorsal pathway was described as a *where* pathway, in contrast to the ventral *what* pathway [@UngerleiderMishkin82]. However, [@GoodaleMilner92] provide a compelling broader interpretation of this pathway as performing a *how* function -- mapping from perception to action. One aspect of this *how* functionality involves spatial location information, in that this information is highly relevant for controlling motor actions in 3D space, but spatial information is too narrow of a definition for the wide range of functions supported by the parietal lobe. Parietal areas are important for numerical and mathematical processing, and representation of abstract relationship information, for example. Areas of parietal cortex also appear to be important for modulating episodic memory function in the medial temporal lobe, and various other functions. This later example may represent a broader set of functions associated with prefrontal cortical cognitive control -- areas of parietal cortex are almost always active in combination with prefrontal cortex in demanding cognitive control tasks, although there is typically little understanding of what precise role they might be playing.

![**Figure 6.16:** Where's Waldo visual search example.](figures/fig_wheres_waldo.jpg){ width=50% }

In this chapter, we focus on the established *where* aspect of parietal function, and we'll take up some of the *how* functionality in the next chapter on Motor Control. Even within the domain of spatial processing, there are many cognitive functions that can be performed using parietal spatial representations, but we focus here on their role in focusing attention to spatial locations. In relation to the previous section, one crucial function of spatial attention is to enable object recognition to function in visual scenes that have multiple different objects present. For example, consider one of those "where's Waldo" puzzles () that is packed with rich visual detail. Is it possible to perceive such a scene all in one take? No. You have to scan over the image using a "spotlight" of visual attention to focus on small areas of the image, which can then be processed effectively by the object recognition pathway. The ability to direct this spotlight of attention depends on spatial representations in the dorsal pathway, which then interact with lower levels of the object recognition pathway (V1, V2, V4) to constrain the inputs to reflect only those visual features that come from within this spotlight of attention.

### Hemispatial Neglect

![**Figure 6.17:** Drawings of given target objects by patients with hemispatial neglect, showing profound neglect of the left side of the drawings.](figures/fig_neglect_drawings.png){ width=30% }

![**Figure 6.18:** Progression of self portraits by an artist with hemispatial neglect, showing gradual remediation of the neglect over time.](figures/fig_neglect_portrait.jpg){ width=30% }

![**Figure 6.19:** Results of a line bisection task for a person with hemispatial neglect. Notice that neglect appears to operate at two different spatial scales here: for the entire set of lines, and within each individual line.](figures/fig_neglect_line_bisect.png){ width=50% }

Some of the most striking evidence that the parietal cortex is important for spatial attention comes from patients with hemispatial neglect, who tend to ignore or neglect one side of space (Figures 6.17, 6.18, 6.19). This condition typically arises from a stroke or other form of brain injury affecting the right parietal cortex, which then gives rise to a neglect of the left half of space (due to the crossing over of visual information shown in the biology section). Interestingly, the neglect applies to multiple different spatial reference frames, as shown in , where lines on the left side of the image tend to be neglected, and also each individual line is bisected more toward the right, indicating a neglect of the left portion of each line.

### The Posner Spatial Cueing Task

![**Figure 6.20:** The Posner spatial cueing task, widely used to explore spatial attention effects. The participant is shown a display with two boxes and a central fixation cross -- on some trials, one of the boxes is cued (e.g., the lines get transiently thicker), and then a target appears in one of the boxes (or not at all on catch trials). The participant just presses a key when they first detect the target. Reaction time is quicker for valid cues vs. invalid ones, suggesting that spatial attention was drawn to that side of space. Patients with hemispatial neglect exhibit slowing for targets that appear in the neglected side of space, particularly when invalidly cued.](figures/fig_posner_task.png){ width=50% }

One of the most widely used tasks to study the spotlight of spatial attention is the Posner spatial cueing task, developed by Michael Posner [@Posner80] (Figure 6.20). One side of visual space is cued, and the effects of this cue on subsequent target detection are measured. If the cue and target show up in the same side of space (*valid* cue condition), then reaction times are faster compared to when they show up on different sides of space (*invalid* cue condition) (). This difference in reaction time (RT) suggests that spatial attention is drawn to the cued side of space, and thus facilitates target detection. The invalid case is actually worse than a neutral condition with no cue at all, indicating that the process of reallocating spatial attention to the correct side of space takes some amount of time. Interestingly, this task is typically run with the time interval between cue and target sufficiently brief as to prevent eye movements to the cue -- thus, these attentional effects are described as *covert attention*.

![**Figure 6.21:** Typical data from the Posner spatial cueing task, showing a speedup for valid trials, and a slowdown for invalid trials, compared to a neutral trial with no cueing. The data for patients with hemispatial neglect is also shown, with their overall slower reaction times normalized to that of the intact case.](figures/fig_posner_graph.png){ width=30% }

As shown in Figure 6.21, patients with hemispatial neglect show a disproportionate increase in reaction times for the invalid cue case, specifically when the cue is presented to the good visual field (typically the right), while the target appears in the left. Posner took this data to suggest that these patients have difficulty disengaging attention, according to his box-and-arrow model of the spatial cueing task (Figure 6.22).

![**Figure 6.22:** Posner's box-and-arrow model of the spatial cueing task. He suggests that spatial neglect deficits are attributable to damage to the disengage mechanism. However, this fails to account for the effects of bilateral parietal damage, for example.](figures/fig_posner_disengage_mech.png){ width=20% }

![**Figure 6.23:** Interacting spatial and object recognition pathways can explain Posner spatial attention effects in terms of spatial influences on early object recognition processing, in addition to top-down influence on V1 representations.](figures/fig_spat_attn_lateral.png){ width=30% }

We explore an alternative account here, based on bidirectional interactions between spatial and object processing pathways (Figure 6.23). In this account, damage to one half of the spatial processing pathway leads to an inability of that side to compete against the intact side of the network. Thus, when there is something to compete against (e.g., the cue in the cueing paradigm), the effects of the damage are most pronounced.

Importantly, these models make distinct predictions regarding the effects of *bilateral* parietal damage. Patients with this condition are known to suffer from Balint's syndrome, which is characterized by a profound inability to recognize objects when more than one is present in the visual field [@CoslettSaffran91]. This is suggestive of the important role that spatial attention plays in facilitating object recognition in crowded visual scenes. According to Posner's disengage model, bilateral damage should result in difficulty disengaging from *both* sides of space, producing slowing in invalid trials for both sides of space. In contrast, the competition-based model makes the opposite prediction: the lesions serve to reduce competition on *both* sides of space, such that there should be reduced attentional effects on both sides. That is, the effect of the invalid cue actually decreases in magnitude. The data is consistent with the competition model, and not Posner's model.

### Exploration of Spatial Attention

Open `attn_simpl` in [CCN Sims](https://github.com/CompCogNeuro/sims) to explore a model with spatial and object pathways interacting in the context of multiple spatial attention tasks, including perceiving multiple objects, and the Posner spatial cueing task. It reproduces the behavioral data shown above, and correctly demonstrates the observed pattern of reduced attentional effects for Balint's patients.

