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

This part of the lesson will be done from within either a Docker container or VM. 
All commands will be typed inside that environment. 

## Setting up your CMSSW area

If you completed the lesson on [Docker](https://cms-opendata-workshop.github.io/workshop2022-lesson-docker) you should already have a working CMSSW area.  

Start the container with:
~~~
docker start -i <theNameOfyourContainer>
~~~
{: .language-bash}

Make sure you change directories to the `CMSSW_5_3_32/src` area; for instance, in Docker:

~~~
cd /home/cmsusr/CMSSW_5_3_32/src
~~~
{: .language-bash}

Note that we are not really "installing" CMSSW but setting up an environment for it.  CMSSW was already installed. This is why **every time** you open a new shell you will have to issue the `cmsenv` command, which is just a script that runs to set some environmental variables for your working area:

~~~
cmsenv
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
edmDumpEventContent root://eospublic.cern.ch//eos/opendata/cms/MonteCarlo2012/Summer12_DR53X/TTJets_SemiLeptMGDecays_8TeV-madgraph/AODSIM/PU_RD1_START53_V7N-v1/10000/EA978C41-27D1-E211-9424-003048D46016.root > test_edm_output.log

less test_edm_output.log
~~~
{: .language-bash}
~~~
Type                                  Module                      Label             Process
----------------------------------------------------------------------------------------------
LHEEventProduct                       "source"                    ""                "LHE"
GenEventInfoProduct                   "generator"                 ""                "SIM"
edm::TriggerResults                   "TriggerResults"            ""                "SIM"
vector<int>                           "genParticles"              ""                "SIM"
vector<reco::GenJet>                  "ak5GenJets"                ""                "SIM"
vector<reco::GenJet>                  "ak7GenJets"                ""                "SIM"
vector<reco::GenJet>                  "kt4GenJets"                ""                "SIM"
vector<reco::GenJet>                  "kt6GenJets"                ""                "SIM"
vector<reco::GenMET>                  "genMetCalo"                ""                "SIM"
vector<reco::GenMET>                  "genMetCaloAndNonPrompt"    ""                "SIM"
.
.
.
vector<reco::TrackExtra>              "tevMuons"                  "firstHit"        "RECO"
vector<reco::TrackExtra>              "tevMuons"                  "picky"           "RECO"
vector<reco::TrackExtrapolation>      "trackExtrapolator"         ""                "RECO"
vector<reco::TrackJet>                "ak5TrackJets"              ""                "RECO"
vector<reco::Vertex>                  "offlinePrimaryVertices"    ""                "RECO"
vector<reco::Vertex>                  "offlinePrimaryVerticesWithBS"   ""                "RECO"
vector<reco::VertexCompositeCandidate>    "generalV0Candidates"       "Kshort"          "RECO"
vector<reco::VertexCompositeCandidate>    "generalV0Candidates"       "Lambda"          "RECO"
~~~
{: .output}

You can get from this information the names of physics objects you may be interested in (e.g. `ak5TrackJets`)
as well as what stage of processing they were produced at (**SIM** is for simulations and **RECO** is for reconstruction). 

This information can be useful when writing your analysis code, which will be discussed in a later lesson. 

Some of the other command-line options can be useful as well to filter the information. 

> ## Challenge!
> Try the following options (with the same file) and see what it gives you. Can you see why this might be useful?
>
> ~~~
> edmDumpEventContent --regex=Muon root://eospublic.cern.ch//eos/opendata/cms/MonteCarlo2012/Summer12_DR53X/TTJets_SemiLeptMGDecays_8TeV-madgraph/AODSIM/PU_RD1_START53_V7N-v1/10000/EA978C41-27D1-E211-9424-003048D46016.root
>
> edmDumpEventContent --name root://eospublic.cern.ch//eos/opendata/cms/MonteCarlo2012/Summer12_DR53X/TTJets_SemiLeptMGDecays_8TeV-madgraph/AODSIM/PU_RD1_START53_V7N-v1/10000/EA978C41-27D1-E211-9424-003048D46016.root
> ~~~
> {: .language-bash}
{: .callout}


{% include links.md %}

