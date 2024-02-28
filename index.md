---
layout: entry
title: Documentation for Relation Annotation for the RegulaTome corpus
---


## Relation type hierarchy for relationships annotated in RegulaTome

* Complex_formation    
* Regulation
  * Positive_regulation  
  * Negative_regulation
* Regulation_of_gene_expression
  * Regulation_of_transcription
  * Regulation_of_translation
* Regulation_of_degradation
* Catalysis_of_posttranslational_modification
      * Catalysis_of_small_protein_conjugation_or_removal
          * Catalysis_of_small_protein_conjugation
              * Catalysis_of_Ubiquitination
              * Catalysis_of_SUMOylation
              * Catalysis_of_Neddylation
              * Other_catalysis_of_small_protein_conjugation
          * Catalysis_of_small_protein_removal
              * Catalysis_of_Deubiquitination
              * Catalysis_of_DeSUMOylation
              * Catalysis_of_Deneddylation
              * Other_catalysis_of_small_protein_removal
      * Catalysis_of_phosphoryl_group_conjugation_or_removal
          * Catalysis_of_Phosphorylation
          * Catalysis_of_Dephosphorylation
      * Catalysis_of_other_small_molecule_conjugation_or_removal
          * Catalysis_of_small_molecule_conjugation
              * Catalysis_of_Methylation
              * Catalysis_of_Acylation
                  * Catalysis_of_Acetylation
                  * Catalysis_of_Palmitoylation
                  * Catalysis_of_Myristoylation
              * Catalysis_of_lipidation
                  * Catalysis_of_prenylation
                      * Catalysis_of_farnesylation
                      * Catalysis_of_geranylgeranylation
              * Catalysis_of_ADP-ribosylation
              * Catalysis_of_Glycosylation
              * Other_catalysis_of_small_molecule_conjugation
          * Catalysis_of_small_molecule_removal
              * Catalysis_of_Demethylation
              * Catalysis_of_Deacylation
                  * Catalysis_of_Deacetylation
                  * Catalysis_of_Depalmitoylation
              * Catalysis_of_Deglycosylation
              * Other_catalysis_of_small_molecule_removal
* Out-of-scope


## General guidelines

* Annotations should be made according to the annotator’s best understanding of the __author’s intended meaning in context__. For example, relations expressed using ambiguous verbs such as __"associate"__ that express complex formation in some contexts but not others should be annotated if and only if the annotator interprets the authors as intending to describe complex formation. The annotators should only use the text excerpt they have available to make this judgement.
* Annotators should treat Gene or Gene Product, Protein-containing Complex and Chemical named entities as being __masked__, i.e. they shouldn't annotate relationships between entities just based on their names, when they would be unable to make the same annotations for two other entities. Protein Family should be treated as __NOT__ being masked and it will be a question of experimental setting later as to whether the information that can be extracted from the names is important or not. 


## Detailed guidelines

1. Complex formation relations can be annotated between two different protein mentions, but also between the same mentions, when the masked entities could be viewed as two different entities. However, statements such as “homodimerization of A” __are not annotated__ as _Complex formation_.
2. Complexes of more than two proteins are annotated by creating __all binary relations__ between the components.
3. Nominalized expressions ("interaction of A and B", "A/B interaction", "A:B complex") and noun phrases with __any surface word__ that can be understood as implying the existence of a complex ("A/B complex", "A/B heterodimer") are __annotated__ as expressing complex formation relations. However, __in the absence of any such word__, text such as "A/B" is not annotated. The text A-B will be annotated based on the understanding of the annotator from the entire context (abstract or paragraph) and not based on former biological knowledge. 
~~~ ann
direct inhibition of NFATp/AP-1 complex formation by a nuclear hormone receptor
T1	GGP 21 26	NFATp
T2	Complex 27 31	AP-1
R1	Complex_formation Arg1:T1 Arg2:T2	 
~~~
Exception: Sentences like _"A is phosphorylated in vitro using B/C (or B-C)"_ where B is a kinase and C is a cyclin have been annotated as _"B complex formation C"_. These can be reverted or we can check the error rate in this specific subproblem.
4. Relations __should not be interpreted as combinations__, on the contrary each annotated relation should be __valid on each own__ (e.g. _"A positively regulates the proteolytic degradation of B and that leads to the rapid depletion of B"_, should be annotated as _"A regulation of degradation  B"_ and _"A negative regulation B"_ and not the combination of relations _"A positive regulation B"_ and _"A regulation of degradation B"_).
5. __Co-immunoprecipitation__ can be used as an indicator of complex formation between two named entity mentions
6. _"A regulation of degradation B"_ does not necessarily imply _"A negative regulation of B"_, so this should be annotated with care.
7. Post-translational modifications should __not__ receive a binding annotation unless binding is clearly mentioned in context. Post-translational modifications (PTMs) imply transient interactions which will not be present in physical interaction databases, so they shouldn't be annotated as such. For an example of a corner case see [Specific examples](#specific-examples-discussed)
8. The following are generally understood as implying _Complex formation_:
  * consitutive association
  * stable association
9. The following are generally understood as __NOT__ implying _Complex formation_:
  * synergize
  * stabilize
10. If __part of a protein/complex__ has the ability to __form a complex__, then the ability of the entire protein/complex to do the same can be extrapolated from that. 
11. Subcellular localization is not annotated for _Complex formation_ even if the structure is made of proteins.
12. When an entity is a substrate of another entity then the relation connecting them is __Catalysis of protein modification__. For example:
~~~ann
the most plausible candidates as SCFFbh1 substrates are HR proteins
T1	Complex 33 40	SCFFbh1
T2	Family 56 58	HR
R1	Catalysis_of_protein_modification Arg1:T1 Arg2:T2	 
~~~
13. If a cyclin-kinase complex phosphorylates protein A then __Catalysis of phosphorylation__ should be added both for the _kinase_ __AND__ the _cyclin_. For example:
~~~ann
pRB is phosphorylated by multiple G(1) cyclin-Cdk complexes
T1	GGP 0 3	pRB
T2	Family 39 45	cyclin
T3	Family 46 49	Cdk
R1	Catalysis_of_phosphorylation Arg1:T2 Arg2:T1
R2	Catalysis_of_phosphorylation Arg1:T3 Arg2:T1
~~~
14. Synthetic lethal interactions are genetic and thus are __NOT__ annotated as Complex formation. 
15. If protein A acts as an __effector__ for protein B then _B regulation A_ is annotated. 
16. If __transfection__ with a protein leads to it obtaining a relationship with another protein entity, then that relationship should be annotated. 
17. __Chemicals__ COVALENTLY bound to other entities are not annotated as __Complex formation__, since complex formation is non-covalent.
18. Orientation of Protein A relatively to Protein B is not enough cue to annotate __Complex formation__. For example:
~~~ann
Orientation of palmitoylated CaVbeta2a relative to CaV2.2
T1	Protein 29 38	CaVbeta2a
T2	Complex 51 57	CaV2.2
~~~
19. Incorporation of a small molecule/protein congugate to a Protein (i.e. a PTM) is __Out-of-scope__ and should not be annotated as __Complex formation__
20. Proteoforms (e.g. proteins with PTMs, or isoforms), should receive annotations as if they were the main isoform/unmodified protein. For example:
~~~ann
CDK7 binds preferentially to the SUMOylation-deficient form of SF-1
T1	Protein 0 4	CDK7
T2	Protein 63 67	SF-1
R1	Complex_formation Arg1:T1 Arg2:T2
~~~
21. __Complex formation__ should be annotated when a __Chemical__ binds to any other entity (__Protein__, __Family__ or __Complex__) unless it is clearly stated that the bond is covalent (either by the fact that it is a PTM or covalently bound is mentioned in the text). 
22. __Entity A is post-translationally modified by Entity B__ should be interpreted as _B Catalysis of Protein Modification A_, UNLESS __B is a protein conjugate__, where B should receive an attribute _Small mol PTM_ and the relationship between A and B is __Out-of-scope__
23. The interactions between members of __transient intermediate complexes__ as part of catalytic reactions should __NOT__ be annotated neither between Protein-Protein (e.g. kinase-substrate), nor between Protein-Chemical.
24. __Chemical A modulates, inhibits, acts as an agonist/antagonist for Protein B__: On top of any regulatory relationships, a __Complex formation__ relationship between A and B should be annotated. This rule applies mostly to drugs.


### Negation and speculation

1. Statements explicitly __denying__ a relationship, for example the formation of a complex (_"A does not bind B"_) are __not annotated__ in any way. However, if the negated statement is qualified with conditions in a way that implies that the proteins would normally e.g. form a complex, the statement is annotated as if the negation were absent. For example in the sentence _“When A is phosphorylated, it fails to form a complex with B”_, it is implied that under other circumstances A and B would form a comples and _A Complex Formation B_ is annotated.
2. Statements expressed __speculatively__ or with __hedging__ expressions (e.g. _“may form a complex”_) are __annotated__ identically to affirmative statements (in effect, __speculation and hedging are ignored__).


### Complex formation relationships

Undirected binary relation associating two proteins that form a complex. Annotated for any statement implying the existence of a complex, including statements explicitly discussing the dissociation of a complex.
Relevant ontology terms:
* [GO:0065003](http://amigo.geneontology.org/amigo/term/GO:0065003) (__protein-containing complex assembly__): The aggregation, arrangement and bonding together of a set of macromolecules to form a protein-containing complex.
* [GO:0032984](http://amigo.geneontology.org/amigo/term/GO:0032984) (__protein-containing complex disassembly__): The disaggregation of a protein-containing macromolecular complex into its constituent components.
* [GO:0032991](http://amigo.geneontology.org/amigo/term/GO:0032991) (__protein-containing complex__): A stable assembly of two or more macromolecules, i.e. proteins, nucleic acids, carbohydrates or lipids, in which at least one component is a protein and the constituent parts function together.

Note that by contrast to the scope of [GO:0032991](http://amigo.geneontology.org/amigo/term/GO:0032991) (protein-containing complex) and related terms, the annotated complex formation relation is restricted to cases where both of the associated constituents are _proteins_, _protein complexes_, _protein families_ or _chemicals_.

### Regulation relationships

_Regulation_ relationships are generally annotated in cases where we know that entity A has an effect on entity B, even in cases where we don’t know the type of effect A has on B, but we know it is upstream. The relevant GO term upon which are annotations are based is [Regulation of biological process](http://amigo.geneontology.org/amigo/term/GO:0050789). Some more specific rules had to be added for this class for annotation consistency:

1. __Protein X can do sth to Protein Y, in a Chemical Z dependent manner__: Z>regulates>X
2. __Protein X responds to Chemical Y__: Y>regulates>X
3. __Changes in protein level of protein A lead to changes in the molecular function of protein B__: A>regulates>B
4. __Protein A levels are regulated in a protein-B dependent manner__: B>Regulates>A (As they can be regulated through expression, i.e. positively or through degradation, i.e. negatively)
5. __Protein A participates in the assembly of Complex B__: A>Regulates>B
6. __Protein A structure is regulated by Protein B__: B>Regulates>A (not sure if the structure is regulated positively or negatively)
7. __Protein A and protein B gain a function upon forming a complex and this function affects another entity C__: A>regulates C AND B>regulates>C
8. __Targeting of protein A to a specific subcellular location is mediated/regulated/blocked/allowed (including secretion) by protein B__: B>regulates>A
9. __Protein A mediates the activation/inhibition of protein B__: A>Regulates>B
10. __Activity of entity A is sensitive to entity B__: B>Regulates>A 
11. __PTM of protein A controls protein B__: A>Regulates>B
12. __Protein A phosphorylates Protein B and the phosphorylation leads to regulation of Protein C__: A>Regulates>C, B>Regulates>C, A>Catalysis of phosphorylation>B
13. __Protein A acts upstream of Protein B__: A>Regulates>B
14. __Chemical A-responsive Protein B__: A>Regulates>B
15. __Protein A cleaves Protein B__: A>Regulates>B, because the cleavage is not necessarily leading to up or down regulation of the protein. We choose to annotate it because most of the times the cleavage will have an effect on the protein. The only reason to annotate __Negative Regulation__ and __Regulation of degradation__ in these cases is if it is clearly mentioned that the cleavage from Protein A leads to degradation of Protein B.

#### Special cases for Regulation

1. __Protein A induces a PTM of protein B__: This is not annotated, unless changes in the levels/structure of protein B related to the PTM are described in the sentence, where _Positive/Negative regulation_ should be annotated. 
2. The same rule applies if the __PTM__ is a __small protein conjugation/removal__. So there shouldn't be any extra annotations regarding regulations of protein levels for _Catalysis of (de)ubiquitination_, _Catalysis of (de)SUMOylation_ or _Catalysis of (de)NEDDylation_, unless clearly stated in a sentence.
3. __Entity A-mediated PTM of Entity B__: _A>Catalysis of PTM>B_. But __A-promoted PTM of B__ is not such a strong hint, thus no relationship will be annotated in such cases.
4. __Regulation of a PTM of Protein A by Protein B__ should __NOT__ be annotated as B>Regulation>A. This should also be applied when a single ubiquitin molecule is added (a single ubiquitin does not necessarily imply a change in function). But __regulation/induction of polyubiquitination__ implies __regulation of degradation__, as the polyubiquitin is a signal for the proteasome SHOULD BE annotated as __Regulation of degradation__.
5. __Proteasome__ is the only complex that performs protein degradation, so __Regulation of degradation__ is __NOT__ annotated in that case (as we are annotating Proteins who regulate, not perform, the degradation process). Same applies if a protein is hydrolyzing another protein. 
6. __Protein X degrades Protein Y__ will be annotated as __X>Negative Regulation>Y__ even if the complex is __proteasome__.
7. Regulation of degradation of __misfolded__ proteins should __NOT__ be annotated as regulation of degradation.
8. __Chemical A reduces Protein B-mediated process__: _OOS_ since we don’t know if this is regulating the actual protein mentioned or something downstream in the process.
~~~ ann
atorvastatin significantly reduced IP-mediated crossdesensitization of signalling by TP alpha
T1	Chemical 0 12	atorvastatin
T2	Protein 35 37	IP
T3  Protein 86 93 TP alpha
R1	Out-of-scope Arg1:T1 Arg2:T2	 
~~~
9. __Protein A function was increased/reduced in Protein Y mutants/cells__: __OOS__ because one cannot necessarily extrapolate that it’s the protein the cell line is named after that is responsible for the effect.
10. __Protein A acts as a counterpart of Protein B__: __OOS__, since "acts as a counterpart" means that they have a similar function or position in a different place.
11. __Protein A is reduced by Protein B__: __OOS__. The word "reduced" standalone without mention to levels could refer to the chemical reaction of reduction. This should __NOT__ be annotated as _Negative regulation_.
12. If __Protein X up/down regulates responses induced by Protein Y stimulation__: __OOS__ since we don’t know if it is protein Y that is regulated.

### Positive/Negative regulation relationships

1. __Protein A is essential/required/needed in the assembly of Complex B__: A> Positively regulates>B
2. The word __chaperone__ can be used as a hint to annotate _Positive regulation_
3. __Protein A is necessary for Protein B to perform a function__: A> Positive Regulation>B
4. __Protein A function is restored in the presence of Protein B__: B>Positively Regulates>A
5. __Protein A structure is maintained by Protein B__: B>Positively Regulates>A
6. __Protein A directly activates/inhibits protein B__: A>Positively/Negatively Regulates>B
7. __Protein A levels increase/decrease because of protein B__: B>Positively/Negatively Regulates>A
8. __Cell lines__: when we have cell lines (including mutant cell lines), but it’s not the gene the cell line is named after that is affected annotate as __Positive regulation__
~~~ ann
treatment with MNU results in an enhanced p53-mediated response in parp-1-null cells
T1	Chemical 15 18	MNU
T2	Protein 42 45	p53
T3  Protein 67 73 parp-1
R1	Positive_Regulation Arg1:T1 Arg2:T2	 
~~~
9. __Protein A catalyzes the modification of Chemical B__: A>Negatively Regulates>B. The idea is that if a Chemical is modified, then we end up with another chemical, which means that the actual levels of the original chemical mentioned are lower. The same is not true for Proteins because having a different proteoform (e.g. with a PTM), doesn’t change the Protein in the same sense. __Catalysis of protein modification__ should __NOT__ be annotated though, because PTMs are by definition for Proteins (including Protein Families and Complexes) so those are not annotated for chemical entities.
10. __Protein A transactivates Protein B__: A>Positive Regulation>B, A>Regulation of transcription>B (Transactivation refers to the increased rate of transcription)
11. __Protein A targets Protein B for ubiquitin-mediated degradation__: A Negative Regulation B
12. __If Protein X causes the aggregation of Protein Y__: X Negative Regulation Y
13. __If Protein X activity is attenuated by Protein Y__: Y Negative Regulation X
14. __Suppressed/Repressed activity of a Protein/Protein Family A from Protein B__: B Negative Regulation A
15. __A restrains expression of B__: A Negative Regulation B, A Regulation of gene expression B
16. __A opposes the activity of B__: A Negative Regulation B
17. __A is a biosynthesis inhibitor of B__: A Negative Regulation B

### Named Entity annotation rules

1. Entity name mentions like _ubiquitin_ or reporter genes (e.g. _GFP_) which are _GGPs_ but are blacklisted by tagger, will be assigned the __blacklisted__ attribute
2. Histones: 
  * Tag _H2_, _H3_ etc. when they appear standalone
  * Include _histone_ in the span when it appears with one of the names (e.g. _histone H3_)
  * Tag _histone_ as __Protein family or group__ when it appears standalone
  * Methylated histones are also tagged as __GGP__
3. _Amino acid residues_ should not be annotated as __chemical__ when they are part of a polypeptide chain
4. _Glycosylphosphatidylinosiol_ (GPI) should not be annotated as __chemical__ as it cannot be a standalone chemical
5. Determiners like _the_ should not be included in the entity span of __GGP__, __Protein-containing complex__ and __Protein family or group__
6. Mutants of specific proteins will receive __GGP__ annotations and an _Entity Attribute_: __Mutant__
7. In order for the annotated text to be as close as possible to the ideal NE annotation produced by the NER system, cases where only part-of __mutant names__ are standalone entities, only these mentions should be annotated, e.g. in the following example from [18039934](https://pubmed.ncbi.nlm.nih.gov/18039934/) _sam35_ and __NOT__ _sam35-2_ is annotated as a ggp
~~~ann
The essential protein Sam35 was addressed through use of the temperature-sensitive yeast mutant sam35-2.
T1	GGP 22 27	Sam35
T2	GGP 96 101	sam35
~~~
An exception is when __mutant names are a single word__, and then they are annotated as one mutant entity e.g. __rex1Delta__ in the following sentence from [16100378](https://pubmed.ncbi.nlm.nih.gov/16100378)
~~~ann
However, both the rex1Delta strain and the rex1-1 strain are indistinguishable from wild type.
T1	GGP 18 27	rex1Delta
T2	GGP 43 47	rex1
~~~
8. Named entities that are part of antibodies should be annotated as the corresponding NE type and should receive a _Note: antibody_
9. rRNAs and tRNAs are annotated as __GGP__ with the __noncoding__ attribute
10. __Fusion proteins__ should be treated as two entities for the purposes of annotation and during the creation of the training dataset. These should get an _Entity Attribute_: __Fusion__. The reporter protein in fusion should get a note: __not tagged by tagger__ if it is not detect by tagger. E.g. in this document __NRIF3__ will receive an _Entity Attribute_: __Fusion__ and __Gal4__ will receive an _Entity Attribute_: __Fusion__ and a _Note: not tagged by tagger_ [11713274](https://pubmed.ncbi.nlm.nih.gov/11713274)
~~~ ann
full-length NRIF3 fused to the DNA-binding domain of Gal4
T1	GGP 12 17	NRIF3
T2	GGP 53 57	Gal4
~~~
11. _Domains_ and other _protein regions_ should __NOT__ be annotated as _GGP_
12. _FLAG_ and _6xHis_ are polypeptide protein tags and should receive an _OOS_ annotation, or should not be annotated at all.
13. _ATP_ and _ADP_ are annotated as __OOS__
14. _GTP_ and _GDP_ are annotated as __Chemicals__ due to their signalling function
15. [__Palmitate__](https://pubchem.ncbi.nlm.nih.gov/compound/985), [__Myristate__](https://pubchem.ncbi.nlm.nih.gov/compound/11005), [__Acetate__](https://pubchem.ncbi.nlm.nih.gov/compound/175) and [__ADP-ribose__](https://pubchem.ncbi.nlm.nih.gov/compound/445794) should receive a __Chemical__ annotation. When they are mentioned as [3H]chemical, only the chemical is annotated (e.g. __myristate__ in \[3H\]myristate)
16. __prenyl__ is a functional group and thus __NOT__ annotated as chemical. Similarly __geranylgeranyl__ and __farnesyl__ are also protein groups and are **NOT annotated as Chemical**.
17. __HMR__ and __HML__ loci will be annotated as GGP, but no annotation will be added to their chromosomal location (e.g. 17p13.1)
18. __Cyclic AMP__ is annotated as Chemical and __AMP__ annotated as OOS
19. __Zinc__ as part of __zinc finger__ domain won’t be annotated as Chemical
20. __Leucine__ as part of __leucine zipper/latch__ won’t be  annotated as Chemical
21. __POU__ will be annotated as Family when part-of __POU transcription factor__, annotated as OOS when part of __POU domain__
22. __ankyrin__ will be annotated as OOS when part-of __ankyrin repeats__'
23. __RING__ standalone will NOT be annotated as Family
24. __phospholipid__ will be annotated as __Chemical__ with __blacklist__ attribute


#### Specific rules for complexes/families and plural form annotations

1. If a term is in Gene Ontology and is assigned a [__Protein-containing complex__](http://amigo.geneontology.org/amigo/term/GO:0032991) annotation then it is considered a Complex in this annotation effort, 
2. If a term is found in Gene ontology but it is NOT a __protein-containing complex__, then it will __NOT__ be considered a _Complex_ in this effort
3. If a term is not at all present in Gene Ontology then other resources in the field will be used to decide whether it should be considered a _Complex_ or not (e.g. [Complex Portal](https://www.ebi.ac.uk/complexportal/home), [Reactome](https://reactome.org/)).
4. For cases where it is difficult to distinguish [_family_](pfam.xfam.org/family/PF00110#tabview=tab6) from [_domain_](pfam.xfam.org/family/PF05207#tabview=tab6) mentions, the field type in Pfam could be used to aid in making a decision (if available).
5. When a _protein-containing complex_ name coincides with the name of the GGPs comprising it, seperated by ***ANY*** punctuation, annotation of the __GGP__ NEs is preferred over annotation of the protein-containing complex. Two notable exceptions are _Arp2/3_ and _SWI/SNF_ where a single __Protein-containing complex__ NE is annotated instead
6. __Complexes__ (__Protein-containing complex__) and protein __Families__ (__Protein family or group__) are annotated if there is data verifying the existence of them. As a general rule for __Complexes__ [Gene Ontology - Cellular Component](http://geneontology.org/) or another database of Complexes is enough to annotate an entity as Complex. For protein __Families__ information is gathered from [InterPro](https://www.ebi.ac.uk/interpro/) or [Wikipedia](https://www.wikipedia.org/) or specific publications for very rare cases.
7. The words _"complex"_ and _"family"_ _ should __not__ be part of the entity annotations.
8. Annotations should be applied to all variants of a name: e.g. __NF kappaB__, __NF-kappaB__, __NFkappaB__ should all be marked as __Protein-containing complex__


### Specific Examples discussed

1. Instances of binding and phosphorylation should be seperately annotated as two events when binding is clearly mentioned in text.
~~~ ann
PPP1R12A is phosphorylated at Ser-473 by CDK1 during mitosis, creating docking sites for the POLO box domains of PLK1. Subsequently, PLK1 binds and phosphorylates PPP1R12A.
T1	GGP 0 8	PPP1R12A
T2	GGP 41 45	CDK1
T3	GGP 113 117	PLK1
T4	GGP 133 137	PLK1
T5	GGP 163 171	PPP1R12A
R1	Catalysis_of_phosphorylation Arg1:T2 Arg2:T1
R2	Catalysis_of_phosphorylation Arg1:T4 Arg2:T5
R3	Complex_formation Arg1:T4 Arg2:T5
~~~
2. When _"regulation of expression"_ is mentioned in text, if the annotator suspects that the intended meaning of the authors is __Regulation of Transcription__ based on the context of the document they have available, then the annotator should annotate __Regulation of Gene Expression__, with a __Note: "Potentially Regulation of Transcription"__.
~~~ ann
HSF1 can function as both an activator of heat shock genes and a repressor of non-heat shock genes such as IL1B and c-fos.
T1	GGP 0 4	HSF1
T2	GGP 107 111	IL1B
T3	GGP 116 121	c-fos
R1 Negative_Regulation Arg1:T1 Arg2:T2
R2 Negative_Regulation Arg1:T1 Arg2:T3
R3 Regulation_of_Gene_Expression Arg1:T1 Arg2:T2
R4 Regulation_of_Gene_Expression Arg1:T1 Arg2:T3
~~~
3.	When the level at which the protein product is regulated at is not clear, then the general term _Regulation of Gene Expression_ should be used.
~~~ ann
We demonstrated that IL-12 directly up-regulates IRF-1 to the same extent as IFN-alpha in normal human T cells and in NK cells.
T1	GGP 21 26	IL-12
T2	GGP 49 54	IRF-1
T3	GGP 77 86	IFN-alpha
R1 Regulation_of_Gene_Expression Arg1:T1 Arg2:T2
R2 Regulation_of_Gene_Expression Arg1:T3 Arg2:T2
R3 Positive_Regulation Arg1:T1 Arg2:T2
R4 Positive_Regulation Arg1:T3 Arg2:T2
~~~
4.  In the current scheme we can annotate the semantics of e.g. _"A negatively regulates the expression of B"_ by assigning __two relations__: _A  negatively regulates B_ AND _A Regulation of Gene Expression B_ 
5. The following is a __part-of relationship__ and not complex formation, so it should be annotated as __Out-of-scope__ [16380507](https://pubmed.ncbi.nlm.nih.gov/16380507)
~~~ann
However, GR was not recruited to the p65-NF-kappaB complex after HDAC2 KD.
T1	GGP 9 11	GR
T2	GGP 37 40	p65
T3	Complex 41 50	NF-kappaB
T4	GGP 65 70	HDAC2
R1 Out-of-scope Arg1:T2 Arg2:T3
~~~
6. In document [19328066](https://pubmed.ncbi.nlm.nih.gov/19328066), _Mex67:Mtr2_, _NXF1:NXT1_ and _TAP:P15_ represent protein heterodimers and have been annotated as such, since the first sentence mentions __Mex67:Mtr2 heterodimer__ denoting that in the document _:_ can be used to represent _heterodimers_.
7. In this sentence, it looks like quite clear mentions of __Complex formation__, but _"selectively interact … in a ligand-dependent manner"_ probably denotes the correct pairs, which are otherwise not clear. 
~~~ann
Here we report that PPARgamma and RXR selectively interact with DRIP205 and p160 protein in a ligand-dependent manner
T1	GGP 20 29	PPARgamma
T2	GGP 34 37	RXR
T3	GGP 64 71	DRIP205
T4	Family 76 80	p160
R1 Complex_formation Arg1:T1 Arg2:T3
R2 Complex_formation Arg1:T1 Arg2:T4
R3 Complex_formation Arg1:T2 Arg2:T3
R4 Complex_formation Arg1:T2 Arg2:T4
~~~
In the [very next sentence](https://pubmed.ncbi.nlm.nih.gov/11027271), the authors explain in detail, that only _PPARgamma-DRIP205_ and _RXR-p160_ are interacting.
8. [In this document](https://pubmed.ncbi.nlm.nih.gov/16177062), the _Complex formation_ relationship occurs only for _human CD244_. Complex formation and negated Complex formation are in a single sentence with coordination.
~~~ann
3BP2 interacts with human but not murine CD244
T1	GGP 0 4	3BP2
T2	GGP 41 46	CD244
R1 Out-of-scope Arg1:T1 Arg2:T2
R2 Complex_formation Arg1:T1 Arg2:T2
~~~

For information on Annodoc, see <http://spyysalo.github.io/annodoc/>.
