---
title: "What is in the datafiles?"
teaching: 5
exercises: 5
questions:
- "How do I inspect these files to see what is in them?"
objectives:
- "To be able to see what objects are in the data files"
- "To be able to see how big these files are and how much space these object take up."
keypoints:
- "It's useful to sometimes inspect the files before diving into the full analysis"
- "Some files may not have the information you're looking for"
---

This part of the lesson will be done from within the CMSSW docker container. 
All commands will be typed inside that environment. 

## Go to your CMSSW area

If you completed the lesson on [Docker](https://cms-opendata-workshop.github.io/workshop2022-lesson-docker) you should already have a working CMSSW area.  

Restart your existing `my_od` container with:
~~~
docker start -i my_od
~~~
{: .language-bash}

Make sure you are in the `CMSSW_7_6_7/src` area; if needed change the directory:

~~~
cd /code/CMSSW_7_6_7/src
~~~
{: .language-bash}


## edmXXX tools 

CMS uses a set of homegrown tools to interact with the **AOD** format, all of which are prefixed by **edm**, which
stands for *Event Data Model*. We will not show you all of them, but introduce a few to give you an idea of what
can be done. 

### edmDumpEventContent

The **edmXXX** tools take as an argument the full path to a file. Following a similar approach
to the previous module, we've chosen one of the Monte Carlo files to test, but these commands would equally well with
a data file. 

Let's start by using `edmDumpEventContent` and looking at the options

~~~
edmDumpEventContent --help
~~~
{: .language-bash}
~~~
Usage: edmDumpEventContent [options] templates.root
Prints out info on edm file.

Options:
  -h, --help      show this help message and exit
  --name          print out only branch names
  --all           Print out everything: type, module, label, process, and
                  branch name
  --lfn           Force LFN2PFN translation (usually not necessary)
  --lumi          Look at 'lumi' tree
  --run           Look at 'run' tree
  --regex=REGEX   Filter results based on regex
  --skipping      Print out branches being skipped
  --forceColumns  Forces printouts to be in nice columns
~~~
{: .output}

We will first use `edmDumpEventContent` to see what is in one of these files with no other options. It may take 15-60 seconds to run and
there will be *a lot* of output. You may find it useful to redirect the output to a file and then look at it there
using `less` or a similar command (you can exit `less` by typing `q`). 

~~~
edmDumpEventContent root://eospublic.cern.ch//eos/opendata/cms/mc/RunIIFall15MiniAODv2/DYJetsToLL_M-5to50_TuneCUETP8M1_13TeV-madgraphMLM-pythia8/MINIAODSIM/PU25nsData2015v1_76X_mcRun2_asymptotic_v12-v1/00000/029FD4E8-26DE-E511-92E9-0CC47A78A45A.root  > test_edm_output.log

less test_edm_output.log
~~~
{: .language-bash}
~~~
Type                                  Module                      Label             Process
----------------------------------------------------------------------------------------------
LHEEventProduct                       "externalLHEProducer"       ""                "LHE"
GenEventInfoProduct                   "generator"                 ""                "SIM"
edm::TriggerResults                   "TriggerResults"            ""                "SIM"
edm::TriggerResults                   "TriggerResults"            ""                "HLT"
HcalNoiseSummary                      "hcalnoise"                 ""                "RECO"
L1GlobalTriggerReadoutRecord          "gtDigis"                   ""                "RECO"
double                                "fixedGridRhoAll"           ""                "RECO"
double                                "fixedGridRhoFastjetAll"    ""                "RECO"
.
.
.
vector<reco::GenJet>                  "slimmedGenJets"            ""                "PAT"
vector<reco::GenJet>                  "slimmedGenJetsAK8"         ""                "PAT"
vector<reco::GenParticle>             "prunedGenParticles"        ""                "PAT"
vector<reco::GsfElectronCore>         "reducedEgamma"             "reducedGedGsfElectronCores"   "PAT"
vector<reco::PhotonCore>              "reducedEgamma"             "reducedGedPhotonCores"   "PAT"
vector<reco::SuperCluster>            "reducedEgamma"             "reducedSuperClusters"   "PAT"
vector<reco::Vertex>                  "offlineSlimmedPrimaryVertices"   ""                "PAT"
vector<reco::VertexCompositePtrCandidate>    "slimmedSecondaryVertices"   ""                "PAT"
unsigned int                          "bunchSpacingProducer"      ""                "PAT"
~~~
{: .output}

You can get from this information the names of physics objects you may be interested in (e.g. `slimmedGenJets`)
as well as what stage of processing they were produced at (**SIM** is for simulations, **RECO** is for reconstruction and **PAT** is for MINIAOD level). 

This information can be useful when writing your analysis code, which will be discussed in a later lesson. 

Some of the other command-line options can be useful as well to filter the information. 

> ## Challenge!
> Try the following options (with the same file) and see what it gives you. Can you see why this might be useful?
>
> ~~~
> edmDumpEventContent --regex=Muon root://eospublic.cern.ch//eos/opendata/cms/mc/RunIIFall15MiniAODv2/DYJetsToLL_M-5to50_TuneCUETP8M1_13TeV-madgraphMLM-pythia8/MINIAODSIM/PU25nsData2015v1_76X_mcRun2_asymptotic_v12-v1/00000/029FD4E8-26DE-E511-92E9-0CC47A78A45A.root
>
> edmDumpEventContent --name root://eospublic.cern.ch//eos/opendata/cms/mc/RunIIFall15MiniAODv2/DYJetsToLL_M-5to50_TuneCUETP8M1_13TeV-madgraphMLM-pythia8/MINIAODSIM/PU25nsData2015v1_76X_mcRun2_asymptotic_v12-v1/00000/029FD4E8-26DE-E511-92E9-0CC47A78A45A.root
> ~~~
> {: .language-bash}
{: .callout}


{% include links.md %}

