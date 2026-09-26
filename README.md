# Restriction mapping simulator prototype

Open `index.html` in Safari, Chrome, Firefox, or Edge. It is self-contained and works offline; no installation or server is needed.

1. In **Run the gel** mode, choose enzymes and click **Add digest lane** for each single or combined digest you want. The ladder and uncut DNA are included automatically.
2. Click **Load samples**. After the wells fill, click **Start power**. Bands migrate and separate; two illustrative tracking dyes move at different speeds.
3. Click **Stop gel** when separation looks useful. The dyes disappear. Move over the gel to estimate sizes; the ruler calibrates to the current run time. Click to hold/release it. **Resume power** continues the run.
4. Enter your estimate of the uncut length. Select an enzyme under **Add site**, then click the map bar. Drag each site or enter its position in kb.
5. Click **Check my map**. Both orientations are accepted. **Flip orientation** reflects your sites around your estimated fragment length.
6. **Rerun same gel** immediately restarts the same loaded lanes from zero time, including fragments lost during a long run. It preserves your map and lets you stop earlier on the next attempt. This is a simulation replay. **Prepare fresh gel** retains your chosen lanes and map but resets the samples and run time. Use this before adding more lanes after a run. Fragments that ran off the bottom return only with fresh samples.
7. **Instant results** provides the original immediate gel view. Switching modes resets the run and preserves lanes and your map.
8. Use **Reveal actual map** for the answer or **New problem** for another example.

Checking compares predicted band mobility against the current visible gel using a 4-pixel tolerance in the SVG gel coordinate system. It does not require agreement with hidden site positions or counts. At least one visible linear digest band is required. Keyboard arrows move a focused site by 0.1 kb; Delete removes it.

## Provenance and assumptions

The default 8 kb map is reconstructed from local bank item Q-230-264, under `The rest of BIS 101 material/exam_item_bank/q_in_obsidian/Questions/`. Its original source is `bis-101-002-fq-2025-export.imscc`, QTI member `g2248046544e3e567bc1e2f00d8b9ea1a/assessment_qti.xml`. The reconstructed sites are H at 0.5, X at 5.5, E at 6.5 kb, consistent with all six listed digest results. X, E, and H retain the bank's symbolic enzyme names.

The model assumes complete digestion of linear DNA, no partial digestion or star activity, and an ideal logarithmic gel scale (20–0.2 kb). Identical fragments share a band; intensity is an illustrative function of DNA mass. Migration distance grows linearly with simulated run time, with size-dependent speed based on the logarithmic scale. Twenty simulation seconds reproduces the original instant view; this is not a laboratory time estimate. The two dyes are illustrative, not calibrated chemical dyes. It does not simulate gel concentration, diffusion, voltage, or experimental noise. Generated problems have not been screened for globally unique solutions. Checking accepts maps consistent with the visible bands in the current experiments, including alternative maps. It does not certify uniqueness.

This is a practice prototype: answers are in the page, and progress is not saved across reloads. Original bank files are unchanged.

## Validation

Run `node test_logic.cjs` and `node test_gel.cjs` from this directory. Tests cover the six source digests, triple digestion, reversed maps, tolerance, incorrect length/site counts/positions, and repeated same-enzyme sites. Animation tests use a simulated document and frame clock to check loading, power, stop/resume, dye visibility, fresh samples, instant mode, ruler calibration, and run-off. These do not replace visual browser testing. JavaScript syntax was also checked. Browser visual and interaction testing could not be completed because the browser tool blocked the local file URL.

## Circular DNA mode

Choose **Circular** under **DNA shape**. Changing shape resets the problem, lanes, and entered map. The initial circular teaching example uses an 8 kb circle with three sites; it is adapted from the linear coordinates, not presented as an original bank question. **New problem** supports either one site per enzyme or four total sites in both shapes.

Uncut circular DNA appears as open circle (OC) and supercoiled (SC). Their illustrative mobilities use apparent-size factors of 1.65 and 0.55 times the true length. These fixed factors are simulation choices, not physical predictions. Hovering over a lane with uncut forms suppresses the size ruler and directs the student to a linearized digest. A no-cut digest retains both forms; one cut yields the full-length linear molecule. Multiple cuts produce the intervening segments including the segment crossing coordinate zero.

Click near the circle circumference to add sites and drag to reposition them. Positions are clockwise from zero at the top; tick intervals remain 1 kb. Numeric editing and arrow keys also work; positions wrap through zero. The revealed answer is circular too.

Circular checking uses the same visible-band comparison as linear checking, so rotation and reversal are naturally accepted. Circular forms indicate no cuts but provide no numerical size constraint.

Circular regression checks cover no-cut, single/double/triple digests, rotated/reversed maps, repeated enzymes, incorrect arrangements, editor coordinate conversion, topology switching, form labels, and ruler suppression. Browser visual testing remains unverified under the existing local-file restriction.

### Anchor a circular map at a restriction site

Click **Set this site to 0** beside any entered circular site. All entered positions rotate together, preserving fragment lengths; the selected site moves to the top and is labeled **Zero anchor**. The revealed answer uses that enzyme at zero and chooses an orientation for the closest visual alignment. If that enzyme has multiple actual sites, the display also chooses the closest candidate anchor and says so; this alignment is a display aid, not confirmation that an incomplete map identified the correct individual site. Moving or removing the zero site disables its use as the answer anchor until a site is set to zero again. Map checking remains rotation-independent.

## Evidence-based checking and non-uniqueness

The two practice levels are now **3 enzymes, 3 cuts** and **3 enzymes, 4 cuts**. Neither has been audited for uniqueness. Tests verify that all three enzymes are represented in generated four-cut problems.

**Check my map** uses only the current uncut lane and digest lanes the student ran. Each visible predicted band must have an observed counterpart and vice versa within 4 gel-coordinate pixels. Co-migrating multiplicities and illustrative brightness are not graded; lost bands provide no positional information. Consequently short runs, co-migration, and limited digests can admit multiple maps. Earlier experiments cleared or reset from the gel are not retained as grading evidence.

**My solution is not unique** runs the same check and records a separate claim for discussion in the feedback. It does not award correctness for the claim or prove ambiguity. Students are prompted to demonstrate a second non-equivalent map fitting the same experiments. The flag and feedback are session-only, not saved or submitted to an instructor. The actual-map reveal remains available for comparison.

## Unknown DNA shape

The DNA shape selector now offers **Linear**, **Circular**, and **Unknown**. A label immediately above the gel identifies the known shape, or says Unknown. Unknown mode randomly chooses either topology for each new problem and suppresses OC/SC labels. Students must select **My DNA shape** in the answer section before entering/checking a map. Switching that answer redraws the editor while retaining the samples, lanes, run time, and site coordinates; any zero-anchor designation is cleared.

In Unknown mode, all ruler readings are explicitly apparent sizes relative to the linear ladder. It does not give a topology-specific warning over the uncut lane that would disclose the answer. Checking uses the selected answer topology to predict digest evidence. **Reveal actual map** discloses the true topology and map, independent of the student's selection. The random examples and uncut mobility model remain idealized.

Uncut circular-form brightness is assigned independently of apparent size: the slower OC band has opacity 0.35 and the faster supercoiled (SC, covalently closed) band 0.90. This illustrative abundance contrast also applies in Unknown mode and no-cut digest lanes. These are display settings, not measured mass fractions. Linear digest band brightness is unchanged.
