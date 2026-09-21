PublishedasaconferencepaperatICLR2026
| ROBOCASA365: |     |        | A   | LARGE-SCALE |     |     | SIMULATION   |     |     |     |
| ------------ | --- | ------ | --- | ----------- | --- | --- | ------------ | --- | --- | --- |
| FRAMEWORK    |     |        |     | TRAINING    |     |     | BENCHMARKING |     |     |     |
|              |     |        | FOR |             |     |     | AND          |     |     |     |
| GENERALIST   |     | ROBOTS |     |             |     |     |              |     |     |     |
SoroushNasiriany1,∗,SepehrNasiriany1,∗,AbhiramMaddukuri1,∗,YukeZhu1,2
1TheUniversityofTexasatAustin,2NVIDIAResearch;∗Equalcontribution
https://robocasa.ai
ABSTRACT
|     | Recent                                     | advances    | in robot | learning | have  | accelerated | progress                    | toward | generalist     |     |
| --- | ------------------------------------------ | ----------- | -------- | -------- | ----- | ----------- | --------------------------- | ------ | -------------- | --- |
|     | robots that                                | can perform | everyday |          | tasks | in human    | environments.               |        | Yet it remains |     |
|     | difficulttogaugehowclosewearetothisvision. |             |          |          |       |             | Thefieldlacksareproducible, |        |                |     |
large-scalebenchmarkforsystematicevaluation.Tofillthisgap,wepresentRobo-
|     | Casa365,      | acomprehensivesimulationbenchmarkforhouseholdmobilemanipu- |          |           |               |                  |            |           |              |       |
| --- | ------------- | ---------------------------------------------------------- | -------- | --------- | ------------- | ---------------- | ---------- | --------- | ------------ | ----- |
|     | lation. Built | on the                                                     | RoboCasa | platform, |               | RoboCasa365      | introduces |           | 365 everyday |       |
|     | tasks across  | 2,500                                                      | diverse  | kitchen   | environments, |                  | with over  | 600 hours | of           | human |
|     | demonstration | data                                                       | and over | 1600      | hours         | of synthetically | generated  |           | demonstra-   |       |
tiondata—makingitoneofthemostdiverseandlarge-scaleresourcesforstudy-
|     | ing generalist                         | policies. | RoboCasa365 |           | is        | designed                        | to support | systematic | evalua-       |     |
| --- | -------------------------------------- | --------- | ----------- | --------- | --------- | ------------------------------- | ---------- | ---------- | ------------- | --- |
|     | tions for                              | different | problem     | settings, | including | multi-task                      | learning,  |            | robot founda- |     |
|     | tionmodeltraining,andlifelonglearning. |           |             |           |           | Weconductextensiveexperimentson |            |            |               |     |
thisbenchmarkwithstate-of-the-artmethodsandanalyzetheimpactsoftaskdi-
|     | versity, | dataset scale, | and  | environment | variation    |          | on generalization. |                 | Our | results |
| --- | -------- | -------------- | ---- | ----------- | ------------ | -------- | ------------------ | --------------- | --- | ------- |
|     | provide  | new insights   | into | what        | factors most | strongly | affect             | the performance |     | of      |
generalistrobotsandinformstrategiesforfutureprogressinthefield.
Figure1: OverviewofRoboCasa365. RoboCasa365isalarge-scalesimulationframeworkfortrainingand
benchmarkinggeneralistrobots.RoboCasa365includes365everydaytasks,2500diversekitchenscenes,over
600hoursofhumandemonstrationdata,plus1600hoursofsyntheticallygenerateddemonstrationdata,and
systematicbenchmarksfortrainingandevaluatinggeneralistrobotmodels.
1

PublishedasaconferencepaperatICLR2026
1 INTRODUCTION
Recentadvancesinrobotlearninghavebroughtthefieldclosertogeneralistrobotscapableofper-
formingabroadrangeoftasksacrossdiverseenvironments. Agrowingbodyofworkhasfocused
on collecting large-scale real-world robot datasets and training high-capacity robotic foundation
models(Blacketal.,2024;GeminiRoboticsTeametal.,2025;NVIDIAetal.,2025;PhysicalIntel-
ligenceetal.,2025). Thesemodelshavedemonstratedmeaningfulgeneralizationtonovelobjects,
environments,andtasks,showinggreatpromisetowardsdevelopingbroadlycapablepolicies.
Despitetheseadvances,twomajorchallengesremain. First,traininggeneralistrobotsrequiresvast
amountsofrobotexperiencedata. Althoughrecentdatasetshavegrownsubstantiallyinsize, they
remain limited in diversity and task coverage, which constrains the ability to train robust, gener-
alist policies. Second, real-world evaluation and benchmarking are resource-intensive and time-
consuming. They are often affected by experimental noise, making it difficult to perform repro-
ducible,systematiccomparisonsacrossmethods.
Simulation provides a practical avenue for addressing these challenges. With simulation, we can
createlarge-scaleinteractiondatasets, coveringaneffectivelyinfinitevarietyoftasksandenviron-
ments(Mandlekaretal.,2023;Jiangetal.,2025). Simulationalsoenablesrapidexperimentation,
controlledevaluation,andreproduciblebenchmarkingthatwouldbeinfeasibleinreal-worldrobotics
(Saxenaetal.,2025). Together, thesecapabilitiesmakeitpossibletogeneratedata, trainpolicies,
andsystematicallyevaluategeneralistrobotsatscale. However,existingsimulationframeworksfall
shortofthispotential. Mostcurrenttoolssupportonlylimitedtasksandenvironments,oftenfocus-
ing on simple object manipulation or single-room scenarios (Zhu et al., 2020; James et al., 2020;
Wang et al., 2023). The datasets they generate are small relative to the diversity and complexity
of real-world robotics challenges, and benchmarking is typically confined to these narrow condi-
tions(Liuetal.,2023;Mandlekaretal.,2021). Consequently,itremainsdifficulttostudyhowtask
diversity,environmentvariation,anddatasetscaleaffectpolicygeneralization.
Toaddressthesegaps,weintroduceRoboCasa365,acomprehensivesimulationbenchmarkforev-
eryday household robotics. RoboCasa365 is built on top of the RoboCasa simulation framework
byNasirianyetal.(2024),andisstructuredaroundfourcorecomponents:
Comprehensivetasks: RoboCasa365defines365tasksspanning60distinctkitchenactivities,in-
cluding manipulation, semantic reasoning, long-horizon planning, and memory-dependent tasks.
Thistaskdiversityallowsevaluationacrossmultipledimensionsofgeneralistrobotcapability.
Diverse environments: The benchmark includes 2,500 unique kitchen scenes modeled from real
kitchensacrosstheUnitedStates. Thesescenescaptureawidespectrumoflayouts,objectconfigu-
rations,andvisualvariations,providingrealisticcontextsforavarietyofeverydaytasks.
Large-scaledata:Thebenchmarkprovidesover2,000hoursofrobotinteractiondata.Thisincludes
612 hours of human demonstration data and an additional 1615 hours of synthetic demonstration
datausingtheMimicGendatagenerationtool(Mandlekaretal.,2023)tosignificantlyexpandthe
quantityofdata.
Systematic benchmarking: RoboCasa365 supports rigorous evaluation across three learning set-
tings: massivelymulti-tasktraining, foundationmodeltraining, andlifelonglearning. Thebench-
markisdesignedtofacilitatereproducible,large-scaleexperimentsandin-depthanalysisofwhich
dataandenvironmentfactorsmoststronglyinfluencegeneralization.
Byintegratingtheseelements,RoboCasa365providesalarge,diverse,andsystematicallystructured
resourceforstudyinggeneralistrobotsinsimulation. Itenablesresearcherstoexplorealgorithms,
run reproducible evaluations, and analyze the impact of task and environment diversity on policy
generalization. UsingRoboCasa365,weconductextensiveexperimentstocomparestate-of-the-art
methods, evaluate learning strategies, and investigate the factors that most strongly drive perfor-
manceingeneralistrobotlearning.
2 RELATED WORK
Robot Simulation Frameworks. There is a long line of prior work on building robot simulation
frameworks(Zhuetal.,2020;Guetal.,2023;Mittaletal.,2023;Taoetal.,2025;Szotetal.,2021;
2

PublishedasaconferencepaperatICLR2026
Kolveetal.,2017;Lietal.,2023;2024;Liuetal.,2023;Deitkeetal.,2022). Somearefocusedon
tabletopsettings(Zhuetal.,2020;Liuetal.,2023;Lietal.,2024;Jamesetal.,2020),whilewefocus
onsimulatingentireroom-scalescenes,similartosomeotherpriorworks(Lietal.,2023;Nasiriany
et al., 2024; Szot et al., 2021; Kolve et al., 2017). Our work is unique in that it features hundreds
oftasksacrossthousandsofuniquescenes, large-scale,high-qualitydemonstrationdatasets, anda
suiteofbenchmarksfortrainingandevaluatinggeneralistrobotmodels. Toourbestknowledge,our
workisthefirstsimulationframeworktosatisfyallofthesecriteria.
Datasets and Benchmarks for Generalist Robots. There have been numerous efforts towards
collectinglargerobotdatasetsintherealworld(Brohanetal.,2022;Walkeetal.,2023;Khazatsky
etal.,2024;OpenX-EmbodimentCollaborationetal.,2023).Evaluatingandbenchmarkingpolicies
trainedonthesedatasetsinthe realworldischallengingduetotheresourcesneededto runlarge-
scalesystematicevaluations,despiteseveralrecentapproachestowardsthisgoal(Atreyaetal.,2025;
Zhou et al., 2023; Yenamandra et al., 2023; Zhou et al., 2025; Krotkov et al., 2016; Correll et al.,
2018). Simulationenablesrunninglarge-scalebenchmarks. However,mostsimulationbenchmarks
areconfined toa verynarrow distributionof tasksandenvironments (Mandlekaret al.,2021; Zhu
et al., 2020; Liu et al., 2023; TRI LBM Team et al., 2025). Li et al. (2023) bring forth some of
thelargestdiversityofenvironmentsandtaskstodate, butlackaccompanyinglarge-scaledatasets
for all of these tasks. Nasiriany et al. (2024) include 100k demonstrations spanning 30 tasks and
100scenes. Incontrast,ourdatasetscompriseover500kdemonstrationsacrossover300tasksand
2500 unique scenes. While prior work focuses on benchmarking specific methods such as multi-
tasktraining(TRILBMTeametal.,2025;Nasirianyetal.,2024)andlifelonglearning(Liuetal.,
2023),weprovideacomprehensivesuiteofbenchmarkstosystematicallystudymulti-tasktraining,
foundationmodeltraining,andlifelonglearning.
Training Generalist Robots. There is a long body of work on learning generalist robot policies
fromlarge,diverserobotdatasets(OctoModelTeametal.,2024;OpenX-EmbodimentCollabora-
tionetal.,2023;NVIDIAetal.,2025;Brohanetal.,2023;Kimetal.,2024;Shukoretal.,2025;Wen
etal.,2025). Inourwork,weaimtobeagnostictothechoiceofmodel,andinsteadcreatebench-
marks to systematically assess the capabilities of these models across distinct settings, including
multi-tasktraining,pretraining,andfine-tuningontargetdata,andlifelonglearning.
3 ROBOCASA365: LARGE-SCALE SIMULATION OF 365 EVERYDAY TASKS
WepresentRoboCasa365,alarge-scalesimulationframeworkfortrainingandbenchmarkinggen-
eralistrobots. WeusetheexistingRoboCasasimulationframework(Nasirianyetal.,2024)asthe
startinggroundforRoboCasa365andmakesignificanteffortstoscaleuptheassets,environments,
tasks,anddatasets. Wealsoestablisharigorousbenchmarktostudystate-of-the-artpolicylearning
methods, which we outline in Section 4. In the following sections, we outline the components of
thissimulationframework: assets,scenes,tasks,anddatasets.
3.1 EXPANDINGTHESCOPEOFASSETS
RoboCasafeaturesadiversearrayofobjects,interactablefixtures,andappliances,withafocuson
commontasksinkitchenenvironments. Weusetheexistinglibraryof2,509objectsfromNasiriany
etal.(2024),spanning153objectcategories.Inadditiontothese,wesourceanadditionalcollection
ofhigh-quality3Dassetsspanning57objectcategories. Thesearehigh-quality3Dassetssourced
fromartistsandeditedtopreservestrictqualitystandards. Weusethesenewobjectstosupportnew
tasksandtopopulatevariousareasofkitchenscenesgenerally. Weprovideacompleteinventoryin
AppendixC.1.
Inadditiontothe3Dassets, wesignificantlyexpandtherepertoireofinteractablefixturesandap-
pliancesinthekitchenenvironment. RoboCasa(Nasirianyetal.,2024)includesatotalof20inter-
actablefixturesandappliancesacross4categories: sinks,coffeemachines,stoves,andmicrowaves.
Wesignificantlyexpandthescopeoftheseassetsto456instancesspanning12categories. Wein-
cludenewcategoriesofappliances,suchastoasters,toasterovens,standmixers,blenders,andelec-
trickettles. Alloftheseappliancesarearticulated,includingfridges,ovens,anddishwashers,which
werenotpreviouslyarticulatedunderRoboCasa. Wemodeltheseassetsusingthesameformatas
RoboCasa,asMJCFobjectswithannotationsoftheregions. Foreachcategory,weincludebetween
3

PublishedasaconferencepaperatICLR2026
Pretraining
Scenes
Target
Scenes
Figure2: KitchenScenes. Oursimulationframeworkfeatures2,500distinctkitchenscenesforpretraining
(top,representativesamplesshown),and10distincttargetkitchenscenes(bottom,allscenesshown).
20and50 instancesinordertoensurethat thereissufficientdiversitytosupport generalization to
novelinstances. WeprovideacompleteinventoryofourfixturesandappliancesinAppendixC.2.
3.2 DIVERSEKITCHENSCENES
Achieving generalization in robot learning requires exposure to a wide range of training environ-
ments; we address this need by providing thousands of diverse kitchen scenes spanning a broad
spectrumofhouseholdsettings. Wecategorizethesescenesintopretrainingandtarget splits. Our
goalistousethepretrainingkitchenscenesforlarge-scaledatacollectionandsyntheticdatagen-
erationpipelines;weusethetargetkitchenscenesfortargeteddatacollectionandforrunningmost
ofourexperimentevaluations. UsingtheterminologyfromNasirianyetal.(2024),wedefineeach
kitchensceneasacombinationoflayoutandstyle,wherethelayoutdefinesthefloorplan,andthe
styledefinesthespecificselectionoffixtures,appliances,andtexturesusedinthekitchen. Wecan
configureeachkitchenscenetouseanycombinationoflayoutandscene.
For our target kitchens, we use the 10 layouts and 10 styles defined by Nasiriany et al. (2024) in
RoboCasa,whereeachlayoutismatchedwithaspecificstyle,foratotalof10kitchenscenes. For
our pretraining kitchen scenes, we create 50 distinct new layouts. In order to capture the distri-
butionofdiversescenes, wesourceourkitchensfrom50real-worldhomeswithactivelistingson
Zillow.com, a real estate marketplace. These homes span diverse geographic locations across the
United States. We build digital cousin (Dai et al., 2024) replicas for each of these environments,
making sure to match the floor plan as closely as possible. In addition to these layouts, we create
50distinctstyles. Weensurethatthepretrainingandtargetstylesdonotoverlapintheselectionof
thefixtures,appliances,orenvironmenttexturesused. Together,wehaveatotalcombinationof50
layouts×50styles,foratotalof2,500pretrainingkitchenscenes. Weprovideanoverviewofthe
pretrainingandtargetkitchenscenesinFigure2.
3.3 SUITEOF365EVERYDAYTASKS
Weaimtoprovideadiversesetoftaskstosupportsharingknowledgeacrosstasksandgeneralizing
to new tasks. Nasiriany et al. (2024) define two broad categories of tasks: atomic tasks, which
representtheexecutionofasingleskill,andcompositetasks,whichinvolveexecutingasequenceof
skills. Nasirianyetal.(2024)defineeightfoundationalskills: (1)pick-and-place,(2)openingand
4

Activity list redesign - v0 by Vercel https://v0.app/chat/activity-list-redesign-op1uOLS0I4I?b=b_VWxjMdrKlyf&f=1
PublishedasaconferencepaperatICLR2026/
/ Latest(edited)
Chat
Design
Git
Connect
Vars
| Template Task:BlendIngredients | Activity Families       |                         | Task:LoadDishwasher |
| ------------------------------ | ----------------------- | ----------------------- | ------------------- |
| Se!ings                        | Beverage Preparation    | Cleaning and Sanitizing |                     |
|                                | Adding Ice to Beverages | Cleaning Appliances     |                     |
|                                | Brewing                 | Cleaning Sink           |                     |
|                                | Making Juice            | Loading Dishwasher      |                     |
|                                | Making Smoothies        | Organizing Recycling    |                     |
InAsskt ra ufocllotwi-ounpÉ:Open the blender lid, put the pear in the blender, close Making Tea Sanitizing Surface Instruction:Pick the cup and bowl from the counter, place them in
the lid, and  s t a rt the blender. Preparing Hot Chocolate Sanitizing Cu!ing BoUanrsdaved Changes the  d i s hwasher, and close the dishwasher door.
| v0  M a x |     |     | Reset S a v e |
| --------- | --- | --- | ------------- |
Washing Dishes
| Task:GetToastedBread |               |                     | Task:GatherProduceWashing |
| -------------------- | ------------- | ------------------- | ------------------------- |
|                      | Cooking       | Food Preparation    |                           |
|                      | Baking        | Chopping Food       |                           |
|                      | Boiling       | Chopping Vegetables |                           |
|                      | Broiling Fish | Clearing Table      |                           |
|                      | Frying        | Defrosting Food     |                           |
|                      | Making Toast  | Making Salads       |                           |
Instruction:Start the toaster. Once the lever pops up, take the Sauteing Vegetables Measuring Ingredients Instruction:Pick up the pear and cucumber from the fridge and
bread to the plate on the dining counter. Toasting Bread Washing Fruits & Vegetables place them in the bowl next to the sink.
|                    | ááá                            | ááá                    |                      |
| ------------------ | ------------------------------ | ---------------------- | -------------------- |
| Task:EmptyDishRack | Organizing and Storage         | Serving                | Task:AlignSilverware |
|                    | Arranging Cabinets             | Arranging Bu"et        |                      |
|                    | Loading Fridge                 | Arranging Condiments   |                      |
|                    | Managing Freezer Space         | Filling Serving Dishes |                      |
|                    | Organizing Dishes & Containers | Garnishing Dishes      |                      |
|                    | Organizing Utensils            | Packing Lunches        |                      |
Instruction:Pick up the mug from the dish rack and place it inside Restocking Supplies Plating Food Instruction:Take the fork and place it on the left side of the plate
|     | Sorting Ingredients | Se!ing the Table |     |
| --- | ------------------- | ---------------- | --- |
the open cabinet where all mugs go. Then close the cabinet door. Storing Leftovers ááá and place the spoon on the right side of the plate.
Figure3:CompositeTasks. RoboCasa365features300compositetasksthatinvolveasequenceofskills. We
uselargelanguagemodelstogenerateasetofhigh-levelactivities,andforeachactivity,asetoftaskblueprints.
There are 6 activity families (high-level categories) spanning 60 activities, which organize composite tasks
basedonsharedfunctionalandsemanticstructure.Representativetasksareshownforselectedactivities.
1 of 1 2/8/26, 2:30 PM
closing doors, (3) opening and closing drawers, (4) turning levers, (5) turning knobs, (6) pressing
buttons,(7)insertion,and(8)navigation. Weadopttheseskillsasthebasisforouratomictasks. In
additiontothe25atomictasksinRoboCasa,wecreateanadditionalsetof40newatomictasksto
supportvariousnewappliancesandnewbehaviorsaffordedbyoursimulator. Weprovidetheentire
listof65atomictasksinAppendixE.1.
For our composite tasks, we follow the framework established by Nasiriany et al. (2024), where
weuselargelanguagemodelstosolicittaskblueprints. Theprocessfollowstwostages. First, we
promptLLMstogivealistofactivitiesrepresentinghigh-levelgroupsoftasksinkitchenenviron-
ments. We retrieve a list of the top 60 activities, such as boiling water, toasting bread, brewing
coffee,washingdishes,andstoringleftovers,tonameafew. Foreachactivity,wethenpromptthe
LLMtoprovidetaskblueprints,whichconsistofthenameofthetask,ahigh-leveldescriptionofthe
task,theobjectsandfixturesinvolved,andthesequencesofskillsneededtosolvethetask. Wethen
proceedtowritecodeforthetasksbasedontheseblueprints. Weuse83oftheexistingcomposite
tasksfromRoboCasaandgenerateanadditionalsetof217newcompositetasks,foratotalof300
compositetasks. WeoutlinethefulllistofactivitiesandrepresentativecompositetasksinFigure3.
Intotal,ourbenchmarkincludes365everydaytasks: 65atomictasksand300compositetasks. Out
ofthese,220requiremobilemanipulation,while145canbeperformedwithoutmobility.
3.4 DATASETS
Weprovidealargecollectionofrobotdatasetscoveringallofourtasks. Broadly, ourdatasetsare
divided into two categories: pretraining datasets for data from the pretraining scenes, and target
datasetsfromthetargetscenes.
3.4.1 PRETRAININGDATASETS
Out of the 365 total tasks outlined in Section 3.3, our pretraining data covers 300 tasks, with 65
atomictasksand235compositetasks. Foreachofthese300tasks,wecollect100humandemon-
strations per task via robot teleoperation. This results in 30k human demonstrations total for pre-
training. Forourdatacollection, weusetheFrankaPandaEmikarobot, equippedwithanOmron
mobile base (Haviland et al., 2022), and in principle, our simulation framework can support data
collectionwithothermobilemanipulatorsandhumanoidplatforms.
5

PublishedasaconferencepaperatICLR2026
WealsousetheMimicGengenerationsystem(Mandlekaretal.,2023)togeneratelarge-scalesyn-
theticdataacross60atomictasks. Foreachtask,weusethe100humandemonstrationspreviously
collectedasseeddemonstrations,andgenerate10kdemonstrations,effectivelyscalingdata100×.
3.4.2 TARGETDATASETS
Forourtargetdata,wechoose50representativeonesoutofthe365tasks,groupedintothreesplits:
• Atomic (18 tasks): We include 18 representative tasks out 65 total atomic tasks in the
benchmark.
• Composite-Seen (16 tasks): We choose 16 representative composite tasks spanning
16activities. Theseincludeamixofshortandlong-horizontasks,withsomeinvolving2
subtasksandthelongesttaskinvolving15subtasks.
• Composite-Unseen(16tasks):Totesttheeffectofourpretrainingdata,wealsochoose
16compositetasksthatareunseeninthepretrainingdata. Thesetasksareofsimilardiffi-
cultytothecompositeseentasks,butfocusonanotherdistinctsetof16activities.
We list the entire set of 50 target tasks in Appendix E.2. For each of these tasks, we collect 500
humandemonstrationsviarobotteleportation,foratotalof25kdemonstrations.
3.4.3 DATASETSTATISTICS
Weprovideahigh-leveloverviewofourdatasetsinAppendixF.Ourpretrainingsyntheticdemon-
strationdatasetspansthehighestamountofdata,with1615totalhours,followedbyhumanpretrain-
ingdata(404hours),andthenhumantargetdata(208hours). InFigure4awereportthedistribution
overthenumberofsubtasksrequiredforeachofour365tasks. Mosttasksrequireoneortwosub-
tasks,butthereareafewtasksthatrequire15ormoresubtaskstocomplete. InFigure4b,wereport
thedistributionofepisodelengthsacrossallpretrainingandtargethumandata(55kepisodes). The
majorityofepisodesrangefrom10to60seconds,withalongtailendforlongerhorizonepisodes,
somegoingbeyond3minutes.
100
80
60
40
20
0
1 2 3 4 5 6 7 8 9 11 12 15 16
Number of Subtasks
tnuoC
ksaT
Distribution of Required Subtasks per Task
(a)Distributionofrequiredsubtaskspertask.
01-0 02-01 03-02 04-03 05-04 06-05 07-06 08-07 09-08 001-09 011-001 021-011 031-021 041-031 051-041 +051
10000
8000
6000
4000
2000
0
Demo length (seconds)
someD
fo
rebmuN
Distribution of Human Demonstration Episode Lengths
(b)Distributionofdemonstrationlengths.
Figure 4: Distribution of task lengths (by number of subtasks) and dataset episode lengths (by number of
seconds).Weobservealongtailoftasksanddatarepresentinglong-horizonbehaviors.
4 EXPERIMENTS
In our experiments, we conduct a systematic study to understand the key factors that influence
training generalist robot policies. To this end, we design a comprehensive suite of benchmarks
aimedatansweringthefollowingquestions:
1. Howwelldogeneralistrobotmodelsperformwhentrainedonlargemulti-taskdatasets?
2. Whatroledoespretrainingdataplay,andtowhatextentcanitimprovelearningofdown-
streamtasks?
6

PublishedasaconferencepaperatICLR2026
3. Howeffectivelycanwelearnnewtasksinlifelonglearningsettings?
4. Howdoesthescopeandcompositionofpretrainingdataimpactlearningdownstreamtasks?
4.1 MULTI-TASKTRAINING
Webeginbyinvestigatinghowstate-of-the-artmethodsperformwhentrainedonmassivelymulti-
taskdatasets. Thisevaluationisacriticalsteptowarddevelopinggeneralistrobotsthatcannotonly
masterawiderangeofbehaviorsbutalsoadapttoentirelynoveltasksbeyondtheirtrainingdata.
We train language-conditioned vision-based policies on the mixture of 300 pretraining human
datasets outlined in Section 3.4.1. Each task has 100 human demonstrations, for a total of 30k
demonstrations. Ourexperimentsfeaturefourstate-of-the-artmethods:Diffusionpolicy(Chietal.,
2023),π (Blacketal.,2024),π (PhysicalIntelligenceetal.,2025),andGR00TN1.5(NVIDIA
0 0.5
etal.,2025).
We train a multi-task language-conditioned policy for each method. We use the pretrained check-
points released publicly for π , π , and GR00T N1.5 as the base model for training our models.
0 0.5
WeprovidedetailsonthetrainingprotocolsforeachmethodinAppendixG.
We evaluate on the 50 tasks outlined in Section 3.4.2: Atomic, Composite-Seen, and
Composite-Unseen. Note that the Composite-Unseen tasks represent unseen tasks in the
pretrainingdata; ourevaluationforthesetasksiszero-shot, aimedatunderstandinggeneralization
tonoveltasks. Weevaluateinthepretrainingkitchenscenesforeachtaskandreportaveragetask
completionsuccessratesacrossmethods. SeeAppendixGfordetailsontheevaluationprotocol.
TaskSplit DiffusionPolicy π π GR00TN1.5
0 0.5
Atomic 15.7 36.3 39.6 43.0
Composite-Seen 0.2 5.2 7.1 9.6
Composite-Unseen 1.25 0.7 1.2 4.4
Average 6.1 15.0 16.9 20.0
Table1: Multi-taskTrainingResults. Wecomparestate-of-the-artpolicylearningapproachesonourhuman
pretrainingdataacross300tasks,andreporttasksuccessrates(%)acrossseenandunseentasks. Weseethat
learningcompositetasksismorechallenging,andthatperformancesufferswhenevaluatingonunseentasks.
WereportresultsinTable1. Overall,weseethatacrossallmethods,learningAtomictasksisthe
easiest,followedbylearningComposite-SeentasksandComposite-Unseentasks. Thisis
reasonable, as the Atomic tasks are shorter-horizon tasks that present fewer learning challenges
for imitation learning (Ross et al., 2011), and the lower performance on Composite-Unseen
tasksisduetothefactthatthemodelhasneverbeentrainedonthesetasks. Overall,GR00TN1.5
performsthebestamongallmethods,followedbyπ ,π ,andfinallyDiffusionPolicy. Theyshow
0.5 0
non-zerosuccessratesonComposite-Unseentasks,asignofstrongergeneralizationabilities.
DiffusionPolicyperformstheworst,highlightinghowhigh-capacityvision-language-actionmodels
can better fit large, diverse multi-task robot datasets. While our multi-task learning experiments
show that GR00T N1.5 outperforms other baselines, we do not claim that it is conclusively the
superiormethod. Performancecanbeinfluencedbymanyfactors,includingtheamountofcompute
used (e.g., batch size), data composition, and whether the visual or language backbones are fine-
tuned. Overall,weseeasignificantopportunityforfuturemethodstoimproveupontheseresults.
4.2 FOUNDATIONMODELTRAINING
Inournextexperiment,weareinterestedinstudyingfoundationmodeltraining, i.e.,trainingwith
ourpretrainingdatasets,followedbyfine-tuningonourtargetdatasets. Thislearningparadigmhas
been established by numerous prior works in robotics (Black et al., 2024; NVIDIA et al., 2025),
withevidencethatpretrainingcanaidlearningdownstreamtasksinamorerobustanddata-efficient
manner. In our experiments, our pretraining data includes human datasets across 300 tasks (411
hours),andsyntheticdataacross60atomictasks(1,615hours),whileourtargetdataincludeshuman
datasets across 50 tasks (208 hours). Out of the 50 target tasks, 34 are also represented in the
human pretraining data (Atomic and Composite-Seen tasks), and the target data includes an
additional16compositetasksthatarenotseeninthepretrainingdata(Composite-Unseen). We
7

PublishedasaconferencepaperatICLR2026
|     |     | PretrainingOnly |     |     | TargetOnly | Pretraining+TargetPost-Training |     |
| --- | --- | --------------- | --- | --- | ---------- | ------------------------------- | --- |
TaskType
|                  |     |     |      | 10%  | 30%  | 100% 10% 30%   | 100% |
| ---------------- | --- | --- | ---- | ---- | ---- | -------------- | ---- |
| Atomic           |     |     | 41.9 | 38.7 | 50.6 | 60.6 56.9 59.1 | 68.5 |
| Composite-Seen   |     |     | 0.0  | 11.0 | 22.7 | 35.0 25.4 34.6 | 40.6 |
| Composite-Unseen |     |     | 0.2  | 11.2 | 27.5 | 33.3 22.7 30.8 | 42.1 |
| Average          |     |     | 15.1 | 21.0 | 34.3 | 43.7 35.9 42.2 | 51.1 |
Table2: FoundationModelTrainingResults. Comparingtheimpactoftrainingonpretrainingandtarget
datasetsonlearningdownstreamtasks.Theperformancesaremeasuredbyaveragetasksuccessrates(%).
firsttrainonallofourpretrainingdatasets(seeSection3.4.1),followedbyfine-tuningindependently
onthreeseparatetargetsplitdatasets(Atomic,Composite-Seen,Composite-Unseen;see
Section 3.4.2). We compare learning on different amounts of target data, with 50, 150, and 500
demospertask,representing10%,30%,and100%ofthetotaltargetdata.
Unlessotherwisenoted,weuseGR00TN1.5asthemodelfortheseexperimentsandallsubse-
quentexperiments. Weopensourceallmodelsforthecommunitytobenchmarkallmethods. We
comparepretrainingonly,targettasklearningonly,andpretrainingfollowedbypost-trainingontar-
getdata. Aftertraining,weevaluatethemodelacrossthe50targettasksinthetargetkitchens. See
AppendixGforadetaileddiscussionofthetrainingandevaluationprotocols. Wereportexperiment
resultsinTable2.
| We see | that with pretraining | alone, | the | model | per- |     |     |
| ------ | --------------------- | ------ | --- | ----- | ---- | --- | --- |
Foundation Model Training Results
| forms over                     | 40% on the | atomic | tasks           | but performs |     |     |     |
| ------------------------------ | ---------- | ------ | --------------- | ------------ | --- | --- | --- |
| verypoorlyonthecompositetasks. |            |        | Fortargetlearn- |              |     | 50  |     |
)%( etaR sseccuS ksaT gvA
45
| ing only,                                   | we see more | capable | policies. | However, |     |     |     |
| ------------------------------------------- | ----------- | ------- | --------- | -------- | --- | --- | --- |
| theyrequireahighamountofdatatobeperformant. |             |         |           |          |     | 40  |     |
Usingpretrainingyieldssignificantimprovementsin
35
| modelperformance. | Thesegainsareespeciallypro- |     |     |     |     |     |     |
| ----------------- | --------------------------- | --- | --- | --- | --- | --- | --- |
30
| nounced | for the Composite-Unseen |     |     | tasks | (see |     |     |
| ------- | ------------------------ | --- | --- | ----- | ---- | --- | --- |
25
| Table 2). | We visualize | the         | improvement | in      | per- |     |                  |
| --------- | ------------ | ----------- | ----------- | ------- | ---- | --- | ---------------- |
|           |              |             |             |         |      | 20  | Pretraining only |
| formance  | in Figure 5, | visualizing | the         | average | task |     |                  |
Target only
success rates from Table 2. We observe a roughly 15 Pretraining + Target Post-training
3× improvement in data efficiency, i.e., pretraining 0 50(10%) 150(30%) 500(100%)
Quantity of Target Demos per Task
helpsachieveroughlythesameperformanceastar-
get learning only with 3× higher number of target Figure5: FoundationModelTrainingResults.
task demonstrations. In Appendix H.2, we present Pre-training enables more effective learning of
arigorousrobustnessevaluationandanalyzetheef- downstreamtaskswithsignificantgainsindataef-
| fectsofdifferentfactorsonperformance. |     |     |     |     |     | ficiency. |     |
| ------------------------------------- | --- | --- | --- | --- | --- | --------- | --- |
4.3 LIFELONGLEARNING
Incontrasttotheconventionaltwo-stageparadigmofpretrainingfollowedbypost-trainingontarget
data,real-worldrobotsmustoftenacquirenewskillscontinuously. Thissetting,knownaslifelong
learning, involves learning tasks over a sequence of phases. The central challenge is leveraging
priorknowledgetolearnnewtaskswhileretainingpreviouslyacquiredskills. Wedesignalifelong
learning benchmark to assess these capabilities. In our experiments, we learn a series of tasks
overfourphases. Eachphaseinvolveslearningprogressivelylongerhorizontasks. Phase1involves
learning65atomictasks,Phase2involveslearning20newcompositetaskswith2or3stages,Phase
3involveslearning20newcompositetaskswith4or5stages,andPhase4involveslearning20new
compositetaskswith6ormorestages. Wedefine“stage”astheinvocationofoneoftherobotskills
definedbyNasirianyetal.(2024), suchaspick-and-place, turningknobs, andnavigation. Weuse
pretrainingdatasetsforthesephases;Phase1includesallhumanandMimicGendatasetsforatomic
tasks,whilePhases2,3,and4featurehumandatasets.
ForeachphaseN,wetakethemodelpreviouslytrainedfromphaseN −1,andfine-tuneitfordata
pertaining to the tasks in phase N. After training completes for phase N, we run evaluations for
tasksfromphase1throughphaseN inthepretrainingkitchensandreportresults. Wereportresults
inTable3. Wemaketwodistinctobservations. First,weseethatthesuccessratessteadilydropas
welearnprogressivelylonger-horizontasksineachnewphase(seethediagonalentriesinthetable).
8

PublishedasaconferencepaperatICLR2026
| Phase  | AtomicTasks | 2-3StageTasks |     | 4–5StageTasks | 6+StageTasks |     |
| ------ | ----------- | ------------- | --- | ------------- | ------------ | --- |
| Phase1 | 41.5        | -             |     | -             |              | -   |
| Phase2 | 13.9        | 24.5          |     | -             |              | -   |
| Phase3 | 13.9        | 4.8           |     | 11.3          |              | -   |
| Phase4 | 10.6        | 1.7           |     | 2.7           |              | 4.3 |
Table 3: Lifelong Learning Results. We train across four phases with progressively longer horizon tasks.
Aftereachphase,wereporttasksuccessrates(%)acrossalltasksseeninthecurrentandpreviousphases.
Thisisintuitivelythecase,aslearninglonger-horizontaskscandemandhigherdatarequirements.
Second,weseethattheperformanceonpreviouslylearnedtaskssteadilydropswitheachnewphase.
Thishighlightsthecatastrophicforgettingproblem,i.e.,performancedegradesonpriortasksifthe
agentdoesnotcontinuetotrainontheminsubsequentphases. Overall,thisexperimenthighlights
thecurrentchallengeswithlifelonglearningandisausefultestbedforimprovingupontheseresults.
4.4 PRETRAININGDATACOMPOSITIONSTUDY
InSection4.2,weshowedthatpretrainingbringsforthsignificantimprovementsindataefficiency
for learning downstream target tasks. In this section, we run experiments to further understand
howthecompositionofpretrainingdataaffectsdownstreamperformance. Inourfoundationmodel
trainingexperiments,weusedalloftheavailablepretrainingdata,comprisinghumandatafrom300
tasksandMimicGendataacross60tasks(Human300+MG60). Wecomparetoavariantthatdoes
notincludeMimicGendataandonlyincludesthehumandata(Human300).Tobetterunderstandthe
roleoftaskdiversityinthepretrainingdata,wecomparetwovariantsthatincludehumandatafrom
50tasks(Human50). These50tasksincludetheAtomicandComposite-Seentasks,aswellas
anadditionalrandomlyselectedsetoftasks. Finally,wecomparewiththecasewithnopretraining
data. OurpretrainingandtargetprotocolareidenticaltotheprocessinSection4.2. Wespecifically
runtwoseparatesetsofexperiments,oneforthelow-dataregimewith10%ofthetargetdata,and
oneforthehigh-dataregimewith100%ofthetargetdata.
PretrainingData
TargetData
|                  |       | NoPretraining | Human50 | Human300  | Human300+MG60 |      |
| ---------------- | ----- | ------------- | ------- | --------- | ------------- | ---- |
| Atomic (10%)     |       | 38.7          |         | 52.0 57.0 |               | 56.9 |
| Composite-Seen   | (10%) | 11.0          |         | 26.2 28.7 |               | 25.4 |
| Composite-Unseen | (10%) | 11.2          |         | 23.8 32.3 |               | 22.7 |
40.0
| Average(10%)     |        | 21.0 |     | 34.7      |     | 35.9 |
| ---------------- | ------ | ---- | --- | --------- | --- | ---- |
| Atomic (100%)    |        | 60.6 |     | 68.1 70.0 |     | 68.5 |
| Composite-Seen   | (100%) |      |     |           |     |      |
|                  |        | 35.0 |     | 41.0 41.2 |     | 40.6 |
| Composite-Unseen | (100%) | 33.3 |     | 38.5 44.0 |     | 42.1 |
| Average(100%)    |        | 43.7 |     | 50.0 52.5 |     | 51.1 |
Table4: PretrainingTaskDiversityResults. Wereporttasksuccessrates(%)andcomparethedownstream
effectsoftrainingondifferentmixturesofpretrainingdata.
We report evaluations in target kitchens in Table 4. Compared to training on all pretraining data
(Human300+MG60),wefindthattrainingonjustthehumandata(Human300)yieldsbetterdown-
streamlearningresults. AlthoughMimicGenenablesthelarge-scalegenerationofsynthetictrajec-
tories,wefindthattheresultingdemonstrationsvaryinquality. Developingmethodsthatcanmore
effectively leverage such large, mixed-quality datasets is an important direction for future work.
ComparingtheHuman50andHuman300settings,weseethatincreasingthenumberofpretraining
taskscanenableasignificantimprovementindownstreamtargettasks, especiallyforthelow-data
regimetargetdatasetting. Notably,thebiggestgainsareseenfortheComposite-Unseentasks,
suggestingthatincreasingthescopeoftaskdiversityisespeciallybeneficialforlearningnoveltasks.
In addition to task diversity, we study the effects of scene diversity in pretraining on downstream
performance. WereporttheseresultsinAppendixH.1.
4.5 REAL-WORLDEXPERIMENTS
Weconductanadditionalsetofexperimentstoexaminetheutilityofourbenchmarkfordownstream
real-worldapplications. Ourreal-worldsetupusestheDROIDPandaarm(Khazatskyetal.,2024)
withthreecameras.
9

PublishedasaconferencepaperatICLR2026
Weexaminefourtasksinarealkitchen:
|     | •   | CloseElectricKettleLid: |     |     | closetheelectrickettlelid |     |     |     |     |     |
| --- | --- | ----------------------- | --- | --- | ------------------------- | --- | --- | --- | --- | --- |
PickPlaceToasterOvenToCounter:
|     | •   |     |     |     |     |     | place the item | from | the toaster | oven to the |
| --- | --- | --- | --- | --- | --- | --- | -------------- | ---- | ----------- | ----------- |
counter
PickPlaceCounterToCabinet:
|     | •   |     |     |     |     | placetheobjectfromthecountertothecabinet |     |     |     |     |
| --- | --- | --- | --- | --- | --- | ---------------------------------------- | --- | --- | --- | --- |
• PlaceOnDishRack: a longer horizon task, involving placing two items from the sink
ontothedishrack.
Wecollect30demonstrationsforeachofthefirstthreetasks,and50
demonstrationsofthelasttask,foratotalof140real-worlddemon-
strations. Wecomparethefollowingsettings:
|     | •   | Real Only: | we  | train the GR00T | N1.5 | model | on the real- |     |     |     |
| --- | --- | ---------- | --- | --------------- | ---- | ----- | ------------ | --- | --- | --- |
worlddemonstrations(140demonstrations);
|     | •   | Sim-and-Real(Ours): |     | wefirstmid-traintheGR00TN1.5 |     |     |     |     |     |     |
| --- | --- | ------------------- | --- | ---------------------------- | --- | --- | --- | --- | --- | --- |
modelonoursimulationtasks(weusedatafromthe150
highestperformingtasksinsimulation),andthenco-fine-
tunethemodelonthereal-worlddemonstrationsandcor-
respondingdataforthefourreal-worldtasksinsimulation.
| Following |     | best | practices | for sim-and-real | alignment |     | from Mad- |     |     |     |
| --------- | --- | ---- | --------- | ---------------- | --------- | --- | --------- | --- | --- | --- |
dukurietal.(2025),were-renderoursimulationdatasetstomatch
thecameraviewsoftherealsetting,facilitatingimprovedtransfer. Figure6: Real-RobotPlatform.
Aftertraining,weevaluateeachmodelintherealworld,wherewe Our real-world setup features a
conduct20trialspertask. WereporttasksuccessratesinTable5. Pandarobotarminarealkitchen.
|     |     |                    |     | CloseElectric | PickPlaceToasterOven |     | PickPlaceCounter |     | PlaceOn  |      |
| --- | --- | ------------------ | --- | ------------- | -------------------- | --- | ---------------- | --- | -------- | ---- |
|     |     |                    |     | KettleLid     | ToCounter            |     | ToCabinet        |     | DishRack | Avg  |
|     |     | RealOnly           |     | 70            |                      | 70  |                  | 52  | 55       | 61.8 |
|     |     | Sim-and-Real(Ours) |     | 70            |                      | 100 |                  | 84  | 65       | 79.8 |
Table5: Real-worldevaluations. Acrossfourreal-worldtasks,wecomparetrainingonreal-worlddataonly
versustrainingonamixtureofoursimulationandreal-worlddata. Byadditionallyusingsimulationdata,it
outperformstrainingonreal-worlddataonlybyanaveragetasksuccessrateof18.1%.
| Overall, |     | the Real | Only | model achieves | a 61.8% | average | success |     |     |     |
| -------- | --- | -------- | ---- | -------------- | ------- | ------- | ------- | --- | --- | --- |
rate, while Sim-and-Real training reaches 79.8%, a substantial improvement. This highlights the
valueofoursimulationbenchmarkforbothalgorithmevaluationandreal-worldpolicylearning.
5 CONCLUSION
We presented RoboCasa365, a large-scale simulation framework for training and benchmarking
generalistrobotmodels. RoboCasa365provides2,500realistickitchenenvironments,365everyday
tasksspanningover50activitycategories,andover2,000hoursofrobotinteractiondata,makingit
oneofthemostdiversesimulationresourcestodate.
Using this benchmark, we conducted a systematic study along three axes: multi-task learning at
scale, foundation model learning, and lifelong learning. Our experiments show that (i) generalist
policiestrainedonlargemulti-taskdatasetscanacquirebroadcompetencebutstillfacechallenges
withlong-horizontasks,(ii)pretrainingdatasignificantlyimprovesdownstreamlearning,withboth
scaleandtaskdiversityplayingkeyroles,and(iii)lifelonglearningremainsanopenchallenge,with
substantialtrade-offsbetweenacquiringnewtasksandretainingpriorknowledge.
RoboCasa365 opens several avenues for future work. First, the benchmark is currently limited to
kitchen environments, raising the question of how well findings transfer to other household set-
tings or broader domains. Second, while the dataset is large, it does not capture the full sensory
andphysicalcomplexityoftherealworld,andbridgingthegapbetweensimulationandreal-world
deployment remains a significant challenge. Addressing these limitations will be an important di-
rectionforfutureresearch.
10

PublishedasaconferencepaperatICLR2026
ACKNOWLEDGMENTS
WethankQiWangforhisvaluableassistanceincoordinatingprojectresources,particularlyindata
collectionandassetpreparation. WethankSteveXieandtheLightWheelteamfortheirclosecol-
laborationinprovidingsimulationassetsandsupportwithdatacollection.WealsothankAjayMan-
dlekar,Zi-angCao,andKevinLinfortheirassistancewithrunningbenchmarkingexperiments.Part
ofthisworkwasdoneduringSoroushNasiriany’sinternshipatNVIDIAResearch. Thisworkwas
partiallysupportedbytheNationalScienceFoundation(FRR-2145283,EFRI-2318065),theOffice
ofNavalResearch(N00014-24-1-2550),theDARPATIAMATprogram(HR0011-24-9-0428),the
ArmyResearchLab(W911NF-25-1-0065),andtheKIST-UTcollaboration(UTAUS-FA00004578).
It was also supported by the Institute of Information & Communications Technology Planning &
Evaluation (IITP) grant funded by the Korean Government (MSIT) (No. RS2024-00457882, Na-
tionalAIResearchLabProject).
REFERENCES
PranavAtreya,KarlPertsch,TonyLee,MooJinKim,ArhanJain,ArturKuramshin,ClemensEpp-
ner,CyrusNeary,EdwardHu,FabioRamos,etal. Roboarena: Distributedreal-worldevaluation
ofgeneralistrobotpolicies. InProceedingsoftheConferenceonRobotLearning(CoRL2025),
2025.
LucasBeyer,AndreasSteiner,Andre´SusanoPinto,AlexanderKolesnikov,XiaoWang,DanielSalz,
MaximNeumann,IbrahimAlabdulmohsin,MichaelTschannen,EmanueleBugliarello,Thomas
Unterthiner, Daniel Keysers, Skanda Koppula, Fangyu Liu, Adam Grycner, Alexey Gritsenko,
Neil Houlsby, ManojKumar, Keran Rong, Julian Eisenschlos, Rishabh Kabra, Matthias Bauer,
Matko Bosˇnjak, Xi Chen, Matthias Minderer, Paul Voigtlaender, Ioana Bica, Ivana Balazˇevic´,
JoanPuigcerver,PinelopiPapalampidi,OlivierHenaff,XiXiong,RaduSoricut,JeremiahHarm-
sen,andXiaohuaZhai. Paligemma: Aversatile3bvlmfortransfer. arXivpreprint,2024. URL
https://arxiv.org/abs/2407.07726.
Kevin Black, Noah Brown, Danny Driess, Adnan Esmail, Michael Equi, Chelsea Finn, Niccolo
Fusai,LachyGroom,KarolHausman,BrianIchter,SzymonJakubczak,TimJones,LiyimingKe,
SergeyLevine,AdrianLi-Bell,MohithMothukuri,SurajNair,KarlPertsch,LucyXiaoyangShi,
James Tanner, Quan Vuong, Anna Walling, Haohuan Wang, and Ury Zhilinsky. 0: A vision-
language-actionflowmodelforgeneralrobotcontrol. arXivpreprintarXiv:2410.24164v1,2024.
URLhttps://arxiv.org/abs/2410.24164v1.
Anthony Brohan, Noah Brown, Justice Carbajal, Yevgen Chebotar, Joseph Dabis, Chelsea Finn,
Keerthana Gopalakrishnan, Karol Hausman, Alex Herzog, Jasmine Hsu, et al. RT-1: Robotics
transformerforreal-worldcontrolatscale. InarXivpreprintarXiv:2212.06817,2022.
Anthony Brohan, Noah Brown, Justice Carbajal, Yevgen Chebotar, Xi Chen, Krzysztof Choro-
manski, Tianli Ding, Danny Driess, Avinava Dubey, Chelsea Finn, Pete Florence, Chuyuan Fu,
Montse Gonzalez Arenas, Keerthana Gopalakrishnan, Kehang Han, Karol Hausman, Alexan-
der Herzog, Jasmine Hsu, Brian Ichter, Alex Irpan, Nikhil Joshi, Ryan Julian, Dmitry Kalash-
nikov, Yuheng Kuang, Isabel Leal, Lisa Lee, Tsang-Wei Edward Lee, Sergey Levine, Yao Lu,
Henryk Michalewski, Igor Mordatch, Karl Pertsch, Kanishka Rao, Krista Reymann, Michael
Ryoo, Grecia Salazar, Pannag Sanketi, Pierre Sermanet, Jaspiar Singh, Anikait Singh, Radu
Soricut, Huong Tran, Vincent Vanhoucke, Quan Vuong, Ayzaan Wahid, Stefan Welker, Paul
Wohlhart,JialinWu,FeiXia,TedXiao,PengXu,SichunXu,TianheYu,andBriannaZitkovich.
Rt-2: Vision-language-action models transfer web knowledge to robotic control, 2023. URL
https://arxiv.org/abs/2307.15818.
Cheng Chi, Siyuan Feng, Yilun Du, Zhenjia Xu, Eric Cousineau, Benjamin Burchfiel, and Shu-
ran Song. Diffusion policy: Visuomotor policy learning via action diffusion. arXiv preprint
arXiv:2303.04137,2023.
Nikolaus Correll, Kostas E. Bekris, Dmitry Berenson, Oliver Brock, Albert Causo, Kris Hauser,
Kei Okada, Alberto Rodriguez, Joseph M. Romano, and Peter R. Wurman. Analysis and ob-
servations from the first amazon picking challenge. IEEE Transactions on Automation Science
11

PublishedasaconferencepaperatICLR2026
and Engineering, 15(1):172–188, 2018. doi: 10.1109/TASE.2016.2600527. URL https:
//doi.org/10.1109/TASE.2016.2600527.
TianyuanDai,JosiahWong,YunfanJiang,ChenWang,CemGokmen,RuohanZhang,JiajunWu,
andLiFei-Fei. Automatedcreationofdigitalcousinsforrobustpolicylearning. arXivpreprint
arXiv:2410.07408,2024.
Matt Deitke, Eli VanderBilt, Alvaro Herrasti, Luca Weihs, Jordi Salvador, Kiana Ehsani, Winson
Han, Eric Kolve, Ali Farhadi, Aniruddha Kembhavi, and Roozbeh Mottaghi. Procthor: Large-
scale embodied ai using procedural generation, 2022. URL https://arxiv.org/abs/
2206.06994.
Gemini Robotics Team, Saminda Abeyruwan, Joshua Ainslie, Jean-Baptiste Alayrac, Montser-
rat Gonzalez Arenas, Travis Armstrong, Ashwin Balakrishna, Robert Baruch, Maria Bauza´,
MichielBlokzijl,StevenBohez,KonstantinosBousmalis,AnthonyBrohan,ThomasBuschmann,
Arunkumar Byravan, Serkan Cabi, Ken Caluwaerts, Federico Casarini, Oscar Chang, Jose´ En-
rique Chen, Xi Chen, Hao-Tien Lewis Chiang, Krzysztof Choromanski, Davide D’Ambrosio,
SudeepDasari,TodorDavchev,ColineDevin,NormanDiPalo,TianliDing,AdilDostmohamed,
Danny Driess, Yilun Du, Debidatta Dwibedi, Michael Elabd, Claudio Fantacci, Cody Fong,
Erik Frey, Chuyuan Fu, Marissa Giustina, Keerthana Gopalakrishnan, Laura Graesser, Leonard
Hasenclever,NicolasHeess,BrandonHernaez,AlexanderHerzog,R.Hofer,Tsang-WeiEdward
Lee,JackyLiang,YixinLin,SharathMaddineni,AnirudhaMajumdar,AssafHurwitzMichaely,
RobertMoreno,MichaelNeunert,FrancescoNori,CarolinaParada,EmilioParisotto,PeterPas-
tor,AcornPooley,KanishkaRao,KristaReymann,DorsaSadigh,StefanoSaliceti,PannagSan-
keti, Pierre Sermanet, Dhruv Shah, Mohit Sharma, Kathryn Shea, Charles Shu, Vikas Sind-
hwani, Sumeet Singh, Radu Soricut, Jost Tobias Springenberg, Rachel Sterneck, Razvan Surd-
ulescu, JieTan, JonathanTompson, VincentVanhoucke, JakeVarley, GraceVesom, GiuliaVez-
zani, Oriol Vinyals, Ayzaan Wahid, and Stefan Welker. Gemini robotics: Bringing ai into the
physical world. CoRR, abs/2503.20020, March 2025. doi: 10.48550/arXiv.2503.20020. URL
https://arxiv.org/abs/2503.20020.
Jiayuan Gu, Fanbo Xiang, Xuanlin Li, Zhan Ling, Xiqiang Liu, Tongzhou Mu, Yihe Tang, Stone
Tao,XinyueWei,YunchaoYao,etal. Maniskill2: Aunifiedbenchmarkforgeneralizablemanip-
ulationskills. arXivpreprintarXiv:2302.04659,2023.
JesseHaviland,NikoSu¨nderhauf,andPeterCorke. Aholisticapproachtoreactivemobilemanipu-
lation. IEEERoboticsandAutomationLetters,7(2):3122–3129,2022.
Stephen James, Zicong Ma, David Rovick Arrojo, and Andrew J Davison. Rlbench: The robot
learningbenchmark&learningenvironment. IEEERoboticsandAutomationLetters,5(2):3019–
3026,2020.
Zhenyu Jiang, Yuqi Xie, Kevin Lin, Zhenjia Xu, Weikang Wan, Ajay Mandlekar, Linxi Fan, and
YukeZhu. Dexmimicgen: Automateddatagenerationforbimanualdexterousmanipulationvia
imitationlearning,2025. URLhttps://arxiv.org/abs/2410.24185.
OussamaKhatib. Inertialpropertiesinroboticmanipulation: Anobject-levelframework. Interna-
tionalJournalofRoboticsResearch,1995.
Alexander Khazatsky, Karl Pertsch, Suraj Nair, Ashwin Balakrishna, Sudeep Dasari, Siddharth
Karamcheti, SoroushNasiriany, MohanKumarSrirama, LawrenceYunliangChen, KirstyEllis,
etal. Droid: Alarge-scalein-the-wildrobotmanipulationdataset,2024.
Moo Jin Kim, Karl Pertsch, Siddharth Karamcheti, Ted Xiao, Ashwin Balakrishna, Suraj Nair,
Rafael Rafailov, Ethan Foster, Grace Lam, Pannag Sanketi, Quan Vuong, Thomas Kollar, Ben-
jamin Burchfiel, Russ Tedrake, Dorsa Sadigh, Sergey Levine, Percy Liang, and Chelsea Finn.
Openvla: Anopen-sourcevision-language-actionmodel,2024. URLhttps://arxiv.org/
abs/2406.09246.
Eric Kolve, Roozbeh Mottaghi, Winson Han, Eli VanderBilt, Luca Weihs, Alvaro Herrasti, Matt
Deitke,KianaEhsani,DanielGordon,YukeZhu,etal.AI2-THOR:Aninteractive3denvironment
forvisualai. arXivpreprintarXiv:1712.05474,2017.
12

PublishedasaconferencepaperatICLR2026
EricP.Krotkov,DouglasHackett,LarryJackel,MichaelPerschbacher,JamesPippine,JesseStrauss,
Gill Pratt, and Christopher Orlowski. The darpa robotics challenge finals: Results and per-
spectives. Journal of Field Robotics, 34(2):229–240, 2016. doi: 10.1002/rob.21683. URL
https://onlinelibrary.wiley.com/doi/10.1002/rob.21683.
Chengshu Li, Ruohan Zhang, Josiah Wong, Cem Gokmen, Sanjana Srivastava, Roberto Mart´ın-
Mart´ın, Chen Wang, Gabrael Levine, Michael Lingelbach, Jiankai Sun, et al. Behavior-1k: A
benchmarkforembodiedaiwith1,000everydayactivitiesandrealisticsimulation.InConference
onRobotLearning,pp.80–93.PMLR,2023.
Xuanlin Li, Kyle Hsu, Jiayuan Gu, Karl Pertsch, Oier Mees, Homer Rich Walke, Chuyuan Fu,
Ishikaa Lunawat, Isabel Sieh, Sean Kirmani, Sergey Levine, Jiajun Wu, Chelsea Finn, Hao Su,
Quan Vuong, and Ted Xiao. Evaluating real-world robot manipulation policies in simulation.
arXivpreprintarXiv:2405.05941,2024.
ZhiqiLi, GuoChen, ShilongLiu, ShihaoWang, V.S.Vibashan, YishenJi, ShiyiLan, HaoZhang,
YilinZhao,SubhashreeRadhakrishnan,NadineChang,KaranSapra,AmalaSanjayDeshmukh,
TuomasRintamaki,MatthieuLe,IliaKarmanov,LukasVoegtle,PhilippFischer,De-AnHuang,
TimoRoman,TongLu,JoseM.Alvarez,BryanCatanzaro,JanKautz,AndrewTao,GuilinLiu,
andZhidingYu. Eagle2: Buildingpost-trainingdatastrategiesfromscratchforfrontiervision-
languagemodels. arXivpreprint,arXiv:2501.14818,2025.
YaronLipman,RickyT.Q.Chen,HeliBen-Hamu,MaximilianNickel,andMattLe. Flowmatching
forgenerativemodeling. InInternationalConferenceonLearningRepresentations(ICLR),2023.
URLhttps://openreview.net/forum?id=KZy4-0etZgZ.
Bo Liu, Yifeng Zhu, Chongkai Gao, Yihao Feng, Qiang Liu, Yuke Zhu, and Peter Stone. Libero:
Benchmarkingknowledgetransferforlifelongrobotlearning. arXivpreprintarXiv:2306.03310,
2023.
Abhiram Maddukuri, Zhenyu Jiang, Lawrence Yunliang Chen, Soroush Nasiriany, Yuqi Xie,
Yu Fang, Wenqi Huang, Zu Wang, Zhenjia Xu, Nikita Chernyadev, Scott Reed, Ken Goldberg,
AjayMandlekar,LinxiFan,andYukeZhu. Sim-and-realco-training: Asimplerecipeforvision-
basedroboticmanipulation.InProceedingsofRobotics:ScienceandSystems(RSS),LosAngeles,
CA,USA,2025.
AjayMandlekar,DanfeiXu,JosiahWong,SoroushNasiriany,ChenWang,RohunKulkarni,LiFei-
Fei,SilvioSavarese,YukeZhu,andRobertoMart´ın-Mart´ın.Whatmattersinlearningfromoffline
humandemonstrationsforrobotmanipulation. InConferenceonRobotLearning,2021.
Ajay Mandlekar, Soroush Nasiriany, Bowen Wen, Iretiayo Akinola, Yashraj Narang, Linxi Fan,
YukeZhu,andDieterFox. Mimicgen:Adatagenerationsystemforscalablerobotlearningusing
humandemonstrations. arXivpreprintarXiv:2310.17596,2023.
Mayank Mittal, Calvin Yu, Qinxi Yu, Jingzhou Liu, Nikita Rudin, David Hoeller, Jia Lin Yuan,
Ritvik Singh, Yunrong Guo, Hammad Mazhar, Ajay Mandlekar, Buck Babich, Gavriel State,
Marco Hutter, and Animesh Garg. Orbit: A unified simulation framework for interactive robot
learning environments. IEEE Robotics and Automation Letters, 8(6):3740–3747, 2023. doi:
10.1109/LRA.2023.3270034.
Soroush Nasiriany, Abhiram Maddukuri, Lance Zhang, Adeet Parikh, Aaron Lo, Abhishek Joshi,
AjayMandlekar,andYukeZhu. Robocasa: Large-scalesimulationofeverydaytasksforgener-
alistrobots. InRobotics: ScienceandSystems(RSS),2024.
NVIDIA, Nikita Cherniadev Johan Bjorck andFernando Castan˜eda, Xingye Da, Runyu Ding,
Linxi”Jim”Fan, YuFang, DieterFox, FengyuanHu, SpencerHuang, JoelJang, ZhenyuJiang,
Jan Kautz, Kaushil Kundalia, Lawrence Lao, Zhiqi Li, Zongyu Lin, Kevin Lin, Guilin Liu,
Edith Llontop, Loic Magne, Ajay Mandlekar, Avnish Narayan, Soroush Nasiriany, Scott Reed,
YouLiangTan,GuanzhiWang,ZuWang,JingWang,QiWang,JiannanXiang,YuqiXie,Yinzhen
Xu,ZhenjiaXu,SeonghyeonYe,ZhidingYu,AoZhang,HaoZhang,YizhouZhao,RuijieZheng,
andYukeZhu. GR00TN1: Anopenfoundationmodelforgeneralisthumanoidrobots. InArXiv
Preprint,March2025.
13

PublishedasaconferencepaperatICLR2026
Octo Model Team, Dibya Ghosh, Homer Walke, Karl Pertsch, Kevin Black, Oier Mees, Sudeep
Dasari,JoeyHejna,CharlesXu,JianlanLuo,TobiasKreiman,YouLiangTan,LawrenceYunliang
Chen,PannagSanketi,QuanVuong,TedXiao,DorsaSadigh,ChelseaFinn,andSergeyLevine.
Octo: Anopen-sourcegeneralistrobotpolicy. InProceedingsofRobotics: ScienceandSystems,
Delft,Netherlands,2024.
Open X-Embodiment Collaboration et al. Open X-Embodiment: Robotic learning datasets and
RT-Xmodels. https://arxiv.org/abs/2310.08864,2023.
EthanPerez,FlorianStrub,HarmdeVries,VincentDumoulin,andAaronCourville. Film: Visual
reasoningwithageneralconditioninglayer. InProceedingsoftheThirty-SecondAAAIConfer-
enceonArtificialIntelligence(AAAI),volume32,pp.3942–3951,2018.doi:10.1609/aaai.v32i1.
11671. URL https://aaai.org/ocs/index.php/AAAI/AAAI18/paper/view/
17253.
PhysicalIntelligence,KevinBlack,NoahBrown,JamesDarpinian,KaranDhabalia,DannyDriess,
Adnan Esmail, Michael Equi, Chelsea Finn, Niccolo Fusai, Manuel Y. Galliker, Dibya Ghosh,
LachyGroom,KarolHausman,BrianIchter,SzymonJakubczak,TimJones,LiyimingKe,Devin
LeBlanc, Sergey Levine, Adrian Li-Bell, Mohith Mothukuri, Suraj Nair, Karl Pertsch, Allen Z.
Ren,LucyXiaoyangShi,LauraSmith,JostTobiasSpringenberg,KyleStachowicz,JamesTanner,
Quan Vuong, Homer Walke, Anna Walling, Haohuan Wang, Lili Yu, and Ury Zhilinsky. π :
0.5
a vision-language-action model with open-world generalization. CoRR, abs/2504.16054, April
2025. doi: 10.48550/arXiv.2504.16054. URLhttps://arxiv.org/abs/2504.16054.
Alec Radford, Jong Wook Kim, Chris Hallacy, Aditya Ramesh, Gabriel Goh, Sandhini Agar-
wal, Girish Sastry, Amanda Askell, Pamela Mishkin, Jack Clark, Gretchen Krueger, and Ilya
Sutskever. Learningtransferablevisualmodelsfromnaturallanguagesupervision. InProceed-
ingsofthe38thInternationalConferenceonMachineLearning(ICML),volume139ofProceed-
ings of Machine Learning Research, pp. 8748–8763, 2021. URL https://proceedings.
mlr.press/v139/radford21a.html.
Ste´phaneRoss, GeoffreyGordon, andDrewBagnell. Areductionofimitationlearningandstruc-
turedpredictiontono-regretonlinelearning. InProceedingsofthefourteenthinternationalcon-
ferenceonartificialintelligenceandstatistics,pp.627–635,2011.
Vaibhav Saxena, Matthew Bronars, Nadun Ranawaka Arachchige, Kuancheng Wang, Woo Chul
Shin, Soroush Nasiriany, Ajay Mandlekar, and Danfei Xu. What matters in learning from
large-scaledatasetsforrobotmanipulation,2025. URLhttps://arxiv.org/abs/2506.
13536.
Mustafa Shukor, Dana Aubakirova, Francesco Capuano, Pepijn Kooijmans, Steven Palma, Adil
Zouitine, Michel Aractingi, Caroline Pascal, Martino Russi, Andres Marafioti, Simon Alibert,
MatthieuCord,ThomasWolf,andRemiCadene. Smolvla: Avision-language-actionmodelfor
affordableandefficientrobotics,2025. URLhttps://arxiv.org/abs/2506.01844.
Andrew Szot, Alexander Clegg, Eric Undersander, Erik Wijmans, Yili Zhao, John Turner, Noah
Maestre,MustafaMukadam,DevendraSinghChaplot,OleksandrMaksymets,etal. Habitat2.0:
Traininghomeassistantstorearrangetheirhabitat. AdvancesinNeuralInformationProcessing
Systems,34:251–266,2021.
Stone Tao, Fanbo Xiang, Arth Shukla, Yuzhe Qin, Xander Hinrichsen, Xiaodi Yuan, Chen Bao,
XinsongLin, YulinLiu, TsekaiChan, YuanGao, XuanlinLi, TongzhouMu, NanXiao, Arnav
Gurha,VisweshNagaswamyRajesh,YongWooChoi,Yen-RuChen,ZhiaoHuang,RobertoCa-
landra,RuiChen,ShanLuo,andHaoSu. Maniskill3: Gpuparallelizedroboticssimulationand
renderingforgeneralizableembodiedai. Robotics: ScienceandSystems,2025.
EmanuelTodorov,TomErez,andYuvalTassa. Mujoco: Aphysicsengineformodel-basedcontrol.
InIEEE/RSJInternationalConferenceonIntelligentRobotsandSystems,pp.5026–5033,2012.
TRI LBM Team, Jose Barreiros, Andrew Beaulieu, Aditya Bhat, Rick Cory, Eric Cousineau,
Hongkai Dai, Ching-Hsin Fang, Kunimatsu Hashimoto, Muhammad Zubair Irshad, Masha Itk-
ina, Naveen Kuppuswamy, Kuan-Hui Lee, Katherine Liu, Dale McConachie, Ian McMahon,
14

PublishedasaconferencepaperatICLR2026
Haruki Nishimura, Calder Phillips-Grafflin, Charles Richter, Paarth Shah, Krishnan Srinivasan,
Blake Wulfe, Chen Xu, Mengchao Zhang, Alex Alspach, Maya Angeles, Kushal Arora, Vi-
tor Campagnolo Guizilini, Alejandro Castro, Dian Chen, Ting-Sheng Chu, Sam Creasey, Sean
Curtis, Richard Denitto, Emma Dixon, Eric Dusel, Matthew Ferreira, Aimee Goncalves, Grant
Gould, Damrong Guoy, Swati Gupta, Xuchen Han, Kyle Hatch, Brendan Hathaway, Allison
Henry,HillelHochsztein,PhoebeHorgan,ShunIwase,DonovonJackson,SiddharthKaramcheti,
SedrickKeh,JosephMasterjohn,JeanMercat,PatrickMiller,PaulMitiguy,TonyNguyen,Jeremy
Nimmer, Yuki Noguchi, Reko Ong, Aykut Onol, Owen Pfannenstiehl, Richard Poyner, Leticia
PriebeMendesRocha,GordonRichardson,ChristopherRodriguez,DerickSeale,MichaelSher-
man, Mariah Smith-Jones, David Tago, Pavel Tokmakov, Matthew Tran, Basile Van Hoorick,
Igor Vasiljevic, Sergey Zakharov, Mark Zolotas, Rares Ambrus, Kerri Fetzer-Borelli, Ben-
jamin Burchfiel, Hadas Kress-Gazit, Siyuan Feng, Stacie Ford, and Russ Tedrake. A care-
ful examination of large behavior models for multitask dexterous manipulation. 2025. URL
https://arxiv.org/abs/2507.05331.
Homer Walke, Kevin Black, Abraham Lee, Moo Jin Kim, Max Du, Chongyi Zheng, Tony Zhao,
PhilippeHansen-Estruch, QuanVuong, AndreHe, VivekMyers, KuanFang, ChelseaFinn, and
Sergey Levine. Bridgedata v2: A dataset for robot learning at scale. In Conference on Robot
Learning(CoRL),2023.
Lirui Wang, Yiyang Ling, Zhecheng Yuan, Mohit Shridhar, Chen Bao, Yuzhe Qin, Bailin Wang,
HuazheXu,andXiaolongWang.Gensim:Generatingroboticsimulationtasksvialargelanguage
models. InArxiv,2023.
Junjie Wen, Yichen Zhu, Jinming Li, Minjie Zhu, Kun Wu, Zhiyuan Xu, Ning Liu, Ran Cheng,
Chaomin Shen, Yaxin Peng, Feifei Feng, and Jian Tang. Tinyvla: Towards fast, data-efficient
vision-language-actionmodelsforroboticmanipulation,2025. URLhttps://arxiv.org/
abs/2409.12514.
Sriram Yenamandra, Arun Ramachandran, Karmesh Yadav, Austin S. Wang, Mukul Khanna,
The´ophile Gervet, Tsung-Yen Yang, Vidhi Jain, Alexander William Clegg, John M. Turner,
ZsoltKira, Manolis Savva, Angel X. Chang, Devendra Singh Chaplot, Dhruv Batra, Roozbeh
Mottaghi, Yonatan Bisk, and Chris Paxton. Homerobot: Open-vocabulary mobile manipula-
tion. In Proceedings of the 7th Conference on Robot Learning (CoRL), volume 229 of Pro-
ceedings of Machine Learning Research, pp. 1975–2011. PMLR, Nov 2023. URL https:
//proceedings.mlr.press/v229/yenamandra23a.html.
Gaoyue Zhou, Victoria Dean, Mohan Kumar Srirama, Aravind Rajeswaran, Jyothish Pari, Kyle
Hatch, Aryan Jain, Tianhe Yu, Pieter Abbeel, Lerrel Pinto, Chelsea Finn, and Abhinav Gupta.
Train offline, test online: A real robot learning benchmark. In Proceedings of the IEEE In-
ternational Conference on Robotics and Automation (ICRA), pp. 9197–9203. IEEE, 2023. doi:
10.1109/ICRA48891.2023.10160594. URL https://doi.org/10.1109/ICRA48891.
2023.10160594.
Zhiyuan Zhou, Pranav Atreya, You Liang Tan, Karl Pertsch, and Sergey Levine. Autoeval: Au-
tonomousevaluationofgeneralistrobotmanipulationpoliciesintherealworld.InProceedingsof
the9thConferenceonRobotLearning(CoRL),volume305ofProceedingsofMachineLearning
Research,pp.1997–2017.PMLR,Sep2025. URLhttps://proceedings.mlr.press/
v305/zhou25a.html.
YukeZhu,JosiahWong,AjayMandlekar,andRobertoMart´ın-Mart´ın. robosuite: Amodularsimu-
lationframeworkandbenchmarkforrobotlearning. InarXivpreprintarXiv:2009.12293,2020.
A USE OF LARGE LANGUAGE MODELS
We use the aid of large language models to create activity labels and task blueprints, using the
processoutlinedbyNasirianyetal.(2024). Wealsouselargelanguagemodelsforsolicitingwriting
feedbackforpartsofthismanuscript.
15

PublishedasaconferencepaperatICLR2026
B SIMULATION INFRASTRUCTURE
B.1 PHYSICSANDRENDERINGENGINE
RoboCasa365isbuiltontopofRoboSuite(Zhuetal.,2020), whichusestheMuJoCophysicsen-
gine(Todorovetal.,2012).WhileMuJoCo’scorephysicscomputationsareCPU-based,weleverage
GPU-basedrendering.RoboCasa365simulatesat20Hz,withthesimulationrunningapproximately
in real time, slightly faster or slower depending on scene complexity and hardware specifications.
Multiple asynchronous environments can be run in parallel, allowing overall throughput to scale
withthenumberofavailableCPUcoresandGPUs.
B.2 ACTIONSPACE
WeadopttheunderlyingcontrollerfromRoboCasa(Nasirianyetal.,2024). Specifically,weusean
OperationalSpaceController(Khatib,1995)runningat20Hzthatcommandsthearmthroughseven
actiondimensions: threefortranslation,threeforrotation,andoneforgripperopeningandclosing.
Inaddition,weincludefiveactiondimensionsformobilebasetranslationandrotation,torsoheight
control,andanactionmodethatgatesmobilebasecontrol.
C SIMULATION ASSETS
C.1 3DOBJECTS
Wehaveaddednewobjectsacross57categories: aluminumfoil,basket,blenderjug,cheesegrater,
chicken drumstick, cinnamon, colander, cookie dough ball, cream cheese stick, digital scale, dish
brush,flourbag,glasscup,honeybottle,hotdogbun,icecube,icecubetray,jar,juice,kebabskewer,
lemon wedge, lettuce, marshmallow, mayonnaise, measuring cup, mustard, non electric kettle, oil
and vinegar bottle, oven tray, pancake, paprika, peeler, pickle slice, pitcher, pizza, pizza cutter,
placemat,pot,reamer,saltandpeppershaker,sandwichbread,saucepan,saucepanlid,shrimp,soap
dispenser, spray, strainer, straw, sugar cube, syrup bottle, tomato slice, tongs, tupperware, turkey
slice,turmeric,whisk,andwoodenspoon.
C.2 INTERACTIVEFIXTURESANDAPPLIANCES
WereportaninventoryofallfixturesandappliancesinTable6.
Category Uniquemodels
Blender 22
Coffeemachine 48
Dishwasher 25
Electrickettle 25
Fridge 50
Microwave 50
Oven 21
Sink 49
Standmixer 25
Stove 50
Toaster 44
Toasteroven 47
Total 456
Table6:Inventoryoffixturesandappliances
16

PublishedasaconferencepaperatICLR2026
D SCENES
We build 50 kitchen layouts modeled after 50 homes on sale on Zillow.com. These homes span
locationsintheBayArea(California),Austin(Texas),Denver(Colorado),Boston(Massachusetts),
andAtlanta(Georgia).
E TASKS
E.1 ATOMICTASKS
Wehave65atomictasks: AdjustToasterOvenTemperature,
AdjustWaterTemperature,CheesyBread,CloseBlenderLid,CloseCabinet,
CloseDishwasher,CloseDrawer,CloseElectricKettleLid,CloseFridge,
CloseFridgeDrawer,CloseMicrowave,CloseOven,CloseStandMixerHead,
CloseToasterOvenDoor,CoffeeServeMug,CoffeeSetupMug,LowerHeat,
MakeIcedCoffee,NavigateKitchen,OpenBlenderLid,OpenCabinet,
OpenDishwasher,OpenDrawer,OpenElectricKettleLid,OpenFridge,
OpenFridgeDrawer,OpenMicrowave,OpenOven,OpenStandMixerHead,
OpenToasterOvenDoor,PackDessert,PickPlaceCabinetToCounter,
PickPlaceCounterToBlender,PickPlaceCounterToCabinet,
PickPlaceCounterToDrawer,PickPlaceCounterToMicrowave,
PickPlaceCounterToOven,PickPlaceCounterToSink,
PickPlaceCounterToStandMixer,PickPlaceCounterToStove,
PickPlaceCounterToToasterOven,PickPlaceDrawerToCounter,
PickPlaceFridgeDrawerToShelf,PickPlaceFridgeShelfToDrawer,
PickPlaceMicrowaveToCounter,PickPlaceSinkToCounter,
PickPlaceStoveToCounter,PickPlaceToasterOvenToCounter,
PickPlaceToasterToCounter,PreheatOven,SlideDishwasherRack,
SlideOvenRack,SlideToasterOvenRack,StartCoffeeMachine,
TurnOffMicrowave,TurnOffSinkFaucet,TurnOffStove,TurnOnBlender,
TurnOnElectricKettle,TurnOnMicrowave,TurnOnSinkFaucet,TurnOnStove,
TurnOnToaster,TurnOnToasterOven,andTurnSinkSpout.
E.2 TARGETTASKS
WeprovideanoverviewforthetargettasksacrossTables7,8,and9.
17

PublishedasaconferencepaperatICLR2026
| Activity Task | #Sub- MoMa | Description |
| ------------- | ---------- | ----------- |
tasks req.
Closethelidblenderbysecurelyplacingthelid
| Atomic CloseBlenderLid | 1 No |     |
| ---------------------- | ---- | --- |
ontop.
| Atomic CloseFridge          | 1 No | Closethefridgedoor(s).   |
| --------------------------- | ---- | ------------------------ |
| Atomic CloseToasterOvenDoor | 1 No | Closethetoasterovendoor. |
Pickthemugfromthecounterandplaceitunder
| Atomic CoffeeSetupMug | 1 No |     |
| --------------------- | ---- | --- |
thecoffeemachinedispenser.
| Atomic NavigateKitchen | 1 Yes | Navigatetothe[kitchenlocation]. |
| ---------------------- | ----- | ------------------------------- |
| Atomic OpenCabinet     | 1 No  | Openthecabinetdoor(s).          |
OpenDrawer
| Atomic                    | 1 No | Openthe[left/right]drawer. |
| ------------------------- | ---- | -------------------------- |
| Atomic OpenStandMixerHead | 1 No | Openthestandmixerhead.     |
Picktheitemfromthecounterandplaceitinthe
| Atomic PickPlaceCounterTo | 1 No |     |
| ------------------------- | ---- | --- |
cabinet.
Cabinet
Picktheitemfromtheplateandplaceitinthe
| Atomic PickPlaceCounterTo | 1 No |     |
| ------------------------- | ---- | --- |
pan.
Stove
Picktheitemfromthedrawerandplaceitonthe
| Atomic PickPlaceDrawerTo | 1 No |     |
| ------------------------ | ---- | --- |
counter.
Counter
Picktheitemfromthesinkandplaceitonthe
| Atomic PickPlaceSinkTo | 1 No |     |
| ---------------------- | ---- | --- |
containerlocatedonthecounter.
Counter
| Atomic PickPlaceToasterTo | 1 No | Placethetoasteditemonaplate. |
| ------------------------- | ---- | ---------------------------- |
Counter
SlideDishwasherRack Fullyslidethetopdishwasherrack[in/out].
| Atomic | 1 No |     |
| ------ | ---- | --- |
Turnoffthe[burnerlocation]burnerofthe
| Atomic TurnOffStove | 1 No |     |
| ------------------- | ---- | --- |
stove.
Atomic TurnOnElectricKettle 1 No Pressdownthelevertoturnontheelectrickettle.
Atomic TurnOnMicrowave 1 No Pressthestartbuttononthemicrowave.
| Atomic TurnOnSinkFaucet | 1 No | Turnonthesinkfaucet. |
| ----------------------- | ---- | -------------------- |
Figure7:Post-trainingAtomic-SeenTasks(18)
Note:The“MoMareq.”columnindicateswhetherthetaskrequiresMobileManipulationorbasenavigation.
18

PublishedasaconferencepaperatICLR2026
| Activity | Task | # MoMa Description |
| -------- | ---- | ------------------ |
Sub- req.
tasks
DeliverStraw Takeastrawfromthedrawerinfrontandplace
| Servingbeverages |     | 4 Yes |
| ---------------- | --- | ----- |
itinsidetheglasscuponthediningcounter.
Startthetoaster.Oncetheleverpopsup,takethe
| Toastingbread | GetToastedBread | 4 Yes |
| ------------- | --------------- | ----- |
breadtotheplateonthediningcounter.
Pickthekettlefromthecounterandplaceitona
| Brewing | KettleBoiling | 2 No |
| ------- | ------------- | ---- |
stoveburner.Thenturntheburneron.
Pickuptheitemsfromthecounter,placethemin
| Loading | LoadDishwasher | 3 No |
| ------- | -------------- | ---- |
thedishwasher,andclosethedishwasherdoor.
dishwasher
Placetwoidenticalitemsofeachobjectineach
Packinglunches PackIdentical 15 Yes tupperwareonthenearbycounter,topacktwo
Lunches identicallunches.
PreSoakPan Pickthepanandspongeandplacethemintothe
| Washingdishes |     | 3 No |
| ------------- | --- | ---- |
sink.Thenturnonthewater.
Pickthemugfromthecabinet,placeitunderthe
Brewing PrepareCoffee 2 No coffeemachinedispenser,andpressthestart
button.
Turnonthesinkandmanueverthespouttowash
| Cleaningsink | RinseSinkBasin | 2 No |
| ------------ | -------------- | ---- |
alllocationsofthesinkbasin.
Pickupthespongefromthecounterandclean
thecuttingboardbybrieflyscrubbingor
| Sanitizingcutting | ScrubCutting | 2 Yes |
| ----------------- | ------------ | ----- |
pressingdownonthecuttingboard.Once
| boards | Board |     |
| ------ | ----- | --- |
finished,releasethesponge.
Grabthepanfromthecabinetandplaceitonthe
[burnerlocation]burneronthestove.Then
| Frying | SearingMeat | 3 Yes |
| ------ | ----------- | ----- |
placetheitemonthestoveandturntheburner
on.
Pickuptheknifefromthedrawerandplaceiton
Slicingmeat SetUpCutting 2 Yes thecuttingboard.Thenplacethemeatfromthe
Station platetothecuttingboard.
Pickupthebowlsonthecounterandstackthem
Organizingdishes StackBowls 2 Yes ontopofoneanotherintheopencabinet.Place
| andcontainers | Cabinet | thesmallerbowlontopofthelargerbowl. |
| ------------- | ------- | ----------------------------------- |
Picktheitemfromthesinkandplaceitinthe
bowl.Thenpickthebowlandplaceitinthe
| Steamingfood | SteamIn | 6 Yes |
| ------------ | ------- | ----- |
microwave.Thenclosethemicrowavedoorand
Microwave
pressthestartbutton.
Puttheitemsinthepot.Retrievethespatulaand
| Sauteing | StirVegetables | 4 Yes |
| -------- | -------------- | ----- |
lightlystirthevegetablesinthepot.
vegetables
Pickthechickendrumstickanditemfromtheir
Storingleftovers StoreLeftovers 5 Yes platesandplacetheminthebowl.Thenputthe
InBowl bowlinthefridge.
Washthelettuceinthesinkbyrunningwater
| Makingsalads | WashLettuce | 2 No |
| ------------ | ----------- | ---- |
overit.
Figure8:Post-trainingComposite-SeenTasks(16)
19

PublishedasaconferencepaperatICLR2026
| Activity | Task | # MoMa Description |
| -------- | ---- | ------------------ |
Sub- req.
tasks
Openthecabinet,pickuptheitemfromthecabinetand
Settingthetable ArrangeBread 5 Yes placeitinthebasket.Thenmovethebaskettothe
Basket diningcounter.
Pickthekettlefromthecounterandplaceitonthetray.
Brewing ArrangeTea 3 No Thenpickthemugfromthecabinetandplaceitonthe
tray.Thenclosethecabinetdoors.
Fromthedifferenttypesofpastriesonthecounter,
selectacroissantandplaceitonthecuttingboard.
| Makingtoast | Bread | 2 Yes |
| ----------- | ----- | ----- |
Thenretrieveajarofjamfromthecabinetandplaceit
Selection
alongsidethecroissantonthecuttingboard.
Puttheshakerandcondimentbottlefromthecounter
| Arranging | Categorize | 2 No |
| --------- | ---------- | ---- |
nexttotheircounterpartsinthecabinet.
| condiments | Condiments |     |
| ---------- | ---------- | --- |
Placetheappropriatecuttingtoolforcuttingtheitem
| Chopping | CuttingTool | 2 No |
| -------- | ----------- | ---- |
skinonthecuttingboard.
| vegetables | Selection |     |
| ---------- | --------- | --- |
Takethestrawberryfromthefridgeandplaceitontop
| Garnishing | Garnish | 4 Yes |
| ---------- | ------- | ----- |
ofthepancake,locatedonthediningcounter.
| dishes | Pancake |     |
| ------ | ------- | --- |
Gatherallobjectsintoonecabinetandsorttheglasses
| Arranging | Gather | 4 Yes |
| --------- | ------ | ----- |
andbowlstooppositesides.
| cabinets | Tableware |     |
| -------- | --------- | --- |
Pickupthekebabskewerandbaguettebread,andplace
Preparing HeatKebab 6 Yes theminsidethetoasteroven.Closethetoasteroven
| sandwiches | Sandwich | doorandstartbysettingthetimer. |
| ---------- | -------- | ------------------------------ |
Grabalemonwedgefromthefridgeandoneicecube
Addingiceto MakeIce 5 Yes fromtheicebowl,andputthemintheglassof
| beverages | Lemonade | lemonade. |
| --------- | -------- | --------- |
PanTransfer Pickupthepananddumpthevegetablesinitontothe
| Servingfood |     | 3 No |
| ----------- | --- | ---- |
plate.Thenreturnthepantothestove.
Placeonebunandonesausagefromthebowloneach
| Portioning | PortionHot | 4 Yes |
| ---------- | ---------- | ----- |
plate.
| meals | Dogs |     |
| ----- | ---- | --- |
Movetheplasticbottlesinthemiddletotheplastics
Organizing Recycle 3 Yes group,andtheglassbottlesinthemiddletotheglass
| recycling | BottlesBy | group. |
| --------- | --------- | ------ |
Type
Takethemeatcontainerthathasthemeatitem(s)and
placeitonthesecondhighestrackofthefreezer.Then
| Managing | Separate | 7 Yes |
| -------- | -------- | ----- |
takethevegetablecontainerthathasthevegetable(s)
| freezerspace | FreezerRack |     |
| ------------ | ----------- | --- |
andplaceitonthehighestrackofthefreezer.
Openthemicrowave,placethebowlwithwaffleinside
Reheatingfood WaffleReheat 4 Yes themicrowave,thenclosethemicrowavedoorandturn
iton.
Putthecolanderinthesink,puttheiteminthe
WashFruit
| Washing |     | 4 No colander,andturnonthesinkfaucetandpourwater |
| ------- | --- | ------------------------------------------------ |
Colander
produce overthecolander.
Picktheitemandplaceitonthedigitalscalefor
| Measuring | Weigh | 2 No |
| --------- | ----- | ---- |
weighing,andclosethecabinet.
| ingredients | Ingredients |     |
| ----------- | ----------- | --- |
Figure9:Post-trainingComposite-UnseenTasks(16)
20

PublishedasaconferencepaperatICLR2026
Figure10: CameraImages. Cameraimagesfromthreeviews(renderedat256×256resolutionarefedinto
themodel.)
F DATASETS
WepresentanoverviewofourdatasetsinTable7.
Setting NumTasks NumScenes DemosperTask DatasetSize(hrs)
Pretraining(Human) 300 2500 100 404
Pretraining(MimicGen) 60 2500 10,000 1615
Target(Human) 50 10 500 208
Table7:Datasetstatisticsacrosspretrainingandtargetsettings.
Foreachdemonstration,westorethelanguageinstruction,proprioceptiveinformation(robotbase
pose,robotendeffectorpose,gripperstateinformation),imagesfromthreecameras(wristcamera,
leftthird-personcamera,rightthird-personcamera),andtheactions.
G POLICY LEARNING
G.1 MODELARCHITECTURESANDTRAININGPROTOCOL
Ourexperimentsfocusontrainingvision-basedmodels. Themodeltakesasinputacombinationof
low-levelproprioceptiveinformation(basepose,endeffectorpose,gripperstate),taskinstruction
language,andcameraimages(onewristcameraimageandtwothird-personcameraimages).
Eachofthecameraimagesisat256×256resolution,andweshowexamplesofthecameraviews
inFigure10.
Weexperimentwithfourmodels:
DiffusionPolicy. DiffusionPolicymodelstrajectorygenerationasaconditionaldenoisingprocess
inactionspace,recoveringactionsfromnoisyexperttrajectoriestohandlemulti-modalrobot
behaviors. Weuseanopensourcediffusionpolicycodebaseandaddlanguageconditioningby
fusingCLIP-basedlanguageembeddings(Radfordetal.,2021)withtheResNetvisualencodervia
FiLMconditioninglayers(Perezetal.,2018). Weusethetransformerdiffusionvariant,witha
12-layertransformerwithanembeddingdimensionof512. Wetrainthemodelwithabatchsizeof
192andtrainfor250kstepsforthemulti-tasklearningexperiment.
π /π .π andπ arevision-language-actionmodelswhichusePaLIGemma(Beyeretal.,
0 0.5 0 0.5
2024)astheunderlyingVLMandfusesanactionexperttooutputrobotactionsviaflow
matching(Lipmanetal.,2023). Weusetheofficialopensourcerepository. Weusethedefaultfull
fine-tuningconfiguration,andweuseabatchsizeof64(thehighestbatchsizewecanfitona
GH200GPU).Forthemulti-tasklearningexperiment,wetrainthemodelfor75ksteps(48hours
oftrainingtimeonaGH200GPU).
GR00TN1.5. GR00TN1.5isavision-language-actionmodelwhichusesasystem1-system2
architecture,withtheEagle2VLM(Lietal.,2025)servingasthehigh-levelencoder(system2)and
21

PublishedasaconferencepaperatICLR2026
anactiondecodertoproduceactionsviaflowmatching(system1). Weusetheofficialopensource
repository. Forallexperiments,wefreezethevisionencoderandlanguageencoder(whicharethe
defaultsettingsfromtheopensourcecodebase),andweuseabatchsizeof128(thehighestbatch
sizewecanfitonaGH200GPU).Forthemulti-tasklearningexperiments,wetrainfor120ksteps.
Forthefoundationmodeltrainingexperiments,wepretrainfor80kstepsandfine-tuneontarget
datafor60ksteps. Finally,forlifelonglearningexperiments,wetrainstage1for100ksteps,
followedby60kstepsforallsubsequentstagesoftraining. Generally,wefindthesesettingstobe
sufficienttoallowformodelconvergence.
G.2 EVALUATIONPROTOCOL
Aftertraining,weevaluatethemodelataspecifiedcheckpointonasuiteofevaluationtasks. For
eachevaluationtask,werun30trialsforaspecifiedmaximumnumberoftimesteps(themaximum
durationistask-dependent). Ifduringthisdurationtheagentachievesthetasksuccesscondition
(binarycondition),theepisodeiscountedasasuccess;otherwise,afailure. Wereporttheaverage
successrateacrosstasks.
H ADDITIONAL EXPERIMENTS
H.1 PRETRAININGSCENEDIVERSITY
Ourpretrainingdataspans2,500kitchenscenes(50layouts×50styles),andwecompareto
restrictingpretrainingdatato25scenes(5layouts×5styles),and5scenes(5layouts×1style).
Torunafaircomparisonacrossthesesettings,weuseMimicGentogeneratedemonstrationsfor
eachsetting,generatingdataacross17atomictasksinpretrainingkitchens. Werunzero-shot
evaluationsonthe10fixedtargetkitchenscenes,andalsotrylearningontheatomictargetdata
with50demonstrationspertask. SeeTable8forresults. Forzero-shotevaluation,weobserve
notableperformancegainsasthenumberofpretrainingscenesincreases. Thesegainsalsoholdin
subsequenttargettaskfine-tuning,highlightingtheneedfordiversepretrainingdata.
PretrainingData
5Scenes 25Scenes 2500Scenes
Zero-shotEvaluation 29.6 39.6 44.7
+Fine-tuningonAtomicTargetData(10%) 53.3 56.7 62.4
Table8:Pretrainingscenediversityresults.Increasingthecompositionofscenesinpretrainingdataimproves
downstreamtaskperformance.
H.2 ROBUSTNESSEVALUATIONS
Inordertoexaminethegeneralizationcapabilitiesendowedbypretrainingonourdata,weperform
asetofrobustnessevaluationsontheGR00TN1.5modeltrainedonthefullpretrainingandtarget
mixture. Weperturbanaspectofthemodel’sinputandevaluateitonourComposite-Seenand
Composite-Unseentasks. Specifically,welookatthefollowingperturbations:
• NovelLanguage: WepromptanLLMfornovelbutsemanticallysimilartaskinstructions.
• NovelJointAngles: WesampleGaussiannoiseandaddittothestartingjointanglesof
therobot.
• NovelBasePose: WesampleGaussiannoiseandaddittothestartingpositionandyawof
therobotbase.
• NovelCameraPose: WesampleGaussiannoiseandaddittothedefaultthird-personand
wristcameraposes.
Wefindthatthemodelisrobusttolanguagevariations,butcansufferfromnovelcameraposes,
jointangles,andbaseposes.
22

PublishedasaconferencepaperatICLR2026
Table9:Evaluationofrobustnessunderdifferentperturbations.
TaskSplit NoPerturbation NovelLanguage CameraPerturbations InitialJointNoise InitialBasePoseNoise
| Composite-Seen   | 40.6 | 38.3 | 28.8 |     | 27.9 | 31.2 |
| ---------------- | ---- | ---- | ---- | --- | ---- | ---- |
| Composite-Unseen | 42.1 | 39.2 | 31.5 |     | 32.1 | 30.2 |
H.3 JOINTCO-TRAININGOFPRETRAININGANDTARGETDATA
Asanextensiontothefoundationmodeltrainingexperiments,weexamineaseparatevariantin
whichwetrainonallpre-trainingdataand100%ofthetargetdatajointlyinonesinglephase. We
trainedthemodelfor120kstepsandreporttheresultingtasksuccessratesinthetargetkitchensas
follows:
| • Atomic-Seen:      | 44.1% |       |     |     |     |     |
| ------------------- | ----- | ----- | --- | --- | --- | --- |
| • Composite-Seen:   | 9.0%  |       |     |     |     |     |
| • Composite-Unseen: |       | 11.7% |     |     |     |     |
| • Average: 22.5%    |       |       |     |     |     |     |
Comparedtoourtwo-stagelearningframework(pretrainingfirst,followedbyfine-tuningontarget
data),performanceunderthisco-trainingregimeissubstantiallylower. Thisresultunderscoresthe
importanceofadedicatedfine-tuningphaseforlearninghighlyperformantpoliciestailoredtothe
targettasks.
H.4 LORAFINE-TUNING
Forthemulti-tasklearningexperimentsinsection4.2,weranGR00TN1.5withLoRAfine-tuning,
trainedforthesamenumberofsteps,batchsize,etc,asthefullfine-tuningvariant. Theresultsare
asfollows:
Table10:Policysuccessrates(%)comparingfullvsLoRAfine-tuningforGR00TN1.5.
|                 | Atomic-Seen | Composite-Seen |     | Composite-Unseen |     | Average |
| --------------- | ----------- | -------------- | --- | ---------------- | --- | ------- |
| Fullfine-tuning | 43.0        |                | 9.6 |                  | 4.4 | 20.0    |
| LoRAfine-tuning | 2.4         |                | 0.2 |                  | 0.8 | 1.2     |
Fullfine-tuningiscriticaltomodelperformance.
| I ADDITIONAL | ANALYSIS |     |     |     |     |     |
| ------------ | -------- | --- | --- | --- | --- | --- |
I.1 FOUNDATIONMODELTRAININGANALYSIS
Webreakdowntheper-tasktaskperformanceforthebestperformingvariant,pretrainingfollowed
byfine-tuningontargetdata(on100%ofdata),inTable11. AmongtheAtomic-Seentasks,the
worst-performingtasksareTurnOffStoveandCloseBlenderLid,whichinvolvehigh
precisionanddexterity. However,forComposite-SeenandComposite-Unseentasks,the
worst-performingtasksspanmanydiversecharacteristics. Werunaqualitativeanalysis,outlining
commonfailuremodesforthetaskswherethemodelperformsata30%orlesssuccessrate:
• SteamInMicrowave: difficultyplacingthebowlinthemicrowave,eitherplacingon
theedgeofthemicrowaveordroppingthebowlintheairrightbeforeplacingitinthe
microwave
• SearingMeat: typicallydoesnotturnonthestoveburnercorrectly,orattemptstoturn
ontheincorrectstoveburner;ordoesnotplacethepanonavalidlocationonthestovetop
| • PackIdenticalLunches: |     | unreliablepickingfromfridge;notmovingtothe |     |     |     |     |
| ----------------------- | --- | ------------------------------------------ | --- | --- | --- | --- |
tupperwaretoplaceitems;placingitemsinwrongtupperware
• PrepareCoffee: oftendoesnotplacethecoffeemugcorrectlyunderthecoffee
machine
23

PublishedasaconferencepaperatICLR2026
• DeliverStraw: rangeoffailures: difficultyopeningdrawers,difficultytransporting
straw(droppingit),difficultyplacingstrawintocup
• GetToastedBread: oftendoesnotpressdownonleverfully;sometimespressesdown
leverbutthenactsrandomly
• PortionHotDogs: unreliablepicksfromcrowdedbowl;unreliableplacebyplacing
itemoncounterinsteadofplate;placingitemsonthewrongplate
• PanTransfer: generallypicksupthepanbutdoesnotreliablyflipthecontentsofthe
panintotheplate;thisisadynamictaskthat’squiteunique,doesnothavemuchoverlap
withothertasksinthebenchmark
• HeatKebabSandwich: oftenfailstopulloutthetoasterrack;othertimesoftenafter
placingthefirstitemontherackinadvertentlypushestherackinbyaccidentanddoesnot
placetheseconditemin
• CategorizeCondiments: pickandplaceisnotreliable,ordoesnotplacematching
condimentsnexttoeachother
• SeparateFreezerRack: oftenfailstoreliablyplacethetupperwareintothefreezer,
asthefreezerisatightspace
• GatherTableware: mustlocatetheothermugfromthekitchen,andbringitback;the
navigationabilityhereisnotreliable;alsosometimesdoesnotplacethemuginsidethe
cabinet,dropsitintheairwithoutreachingfarintothecabinet.
24

PublishedasaconferencepaperatICLR2026
| Task | SuccessRate(%) | Stages MoMaRequired |     |
| ---- | -------------- | ------------------- | --- |
Atomic-Seen
| TurnOnElectricKettle      | 93  | 1   | No  |
| ------------------------- | --- | --- | --- |
| OpenStandMixerHead        | 90  | 1   | No  |
| CloseToasterOvenDoor      | 87  | 1   | No  |
| OpenCabinet               | 87  | 1   | No  |
| SlideDishwasherRack       | 87  | 1   | No  |
| PickPlaceToasterToCounter | 73  | 1   | No  |
| TurnOnMicrowave           | 70  | 1   | No  |
| OpenDrawer                | 70  | 1   | No  |
| PickPlaceSinkToCounter    | 70  | 1   | No  |
| PickPlaceCounterToStove   | 70  | 1   | No  |
| CloseFridge               | 67  | 1   | No  |
| TurnOnSinkFaucet          | 63  | 1   | No  |
| PickPlaceCounterToCabinet | 63  | 1   | No  |
| CoffeeSetupMug            | 60  | 1   | No  |
| NavigateKitchen           | 60  | 1   | Yes |
| PickPlaceDrawerToCounter  | 50  | 1   | No  |
| TurnOffStove              | 37  | 1   | No  |
| CloseBlenderLid           | 37  | 1   | No  |
Composite-Seen
| StackBowlsCabinet    | 83  | 2   | Yes |
| -------------------- | --- | --- | --- |
| PreSoakPan           | 70  | 3   | No  |
| ScrubCuttingBoard    | 70  | 2   | Yes |
| WashLettuce          | 67  | 2   | No  |
| RinseSinkBasin       | 60  | 2   | No  |
| KettleBoiling        | 53  | 2   | No  |
| LoadDishwasher       | 47  | 3   | No  |
| StoreLeftoversInBowl | 43  | 5   | Yes |
| SetUpCuttingStation  | 33  | 2   | Yes |
| StirVegetables       | 33  | 4   | Yes |
| SteamInMicrowave     | 30  | 6   | Yes |
| SearingMeat          | 27  | 3   | Yes |
| PackIdenticalLunches | 17  | 15  | Yes |
| PrepareCoffee        | 13  | 2   | No  |
| DeliverStraw         | 3   | 4   | Yes |
| GetToastedBread      | 0   | 4   | Yes |
Composite-Unseen
| RecycleBottlesByType | 87  | 3   | Yes |
| -------------------- | --- | --- | --- |
| WaffleReheat         | 83  | 4   | Yes |
| ArrangeBreadBasket   | 77  | 5   | Yes |
| WeighIngredients     | 67  | 2   | No  |
| BreadSelection       | 60  | 2   | Yes |
| CuttingToolSelection | 47  | 2   | No  |
| GarnishPancake       | 47  | 4   | Yes |
| ArrangeTea           | 43  | 3   | No  |
| WashFruitColander    | 40  | 4   | No  |
| MakeIceLemonade      | 40  | 5   | Yes |
| PortionHotDogs       | 23  | 4   | Yes |
| PanTransfer          | 20  | 3   | No  |
| HeatKebabSandwich    | 13  | 6   | Yes |
| CategorizeCondiments | 10  | 2   | No  |
| SeparateFreezerRack  | 10  | 7   | Yes |
| GatherTableware      | 7   | 4   | Yes |
Table11:FoundationModelTrainingResults.
25