# ER301 Custom Units

This repo is a collection of other developers custom units (that require compilation).
It's purpose is to allow me to update to use my er301 cross compile build change.
which in turn, is to support Percussa eurorack modules XMX and SSP.

This requires I modify / update all of thier build systems - aka Makefiles

This is unlikey to be useful to a broader dev audience.
and if you seek these custom units for broader usage eg. on er301. 
I encourage you to use the parent/origin of these forks.

as, its is unlikey I will make functional changes to these units, and they make likely become 'stale'

# Pushing changes to parent?
Whilst I could push the changes to the respective repos, most seem to no longer be in active development, 
also given the scope and focus of these changes, its unlikely the owner is going to be interested in the complication it entails.

so I will not be pushing the changes.
however, should the owner of these repos wish to adopt the changes, then the changes are open source, and they are free to use as-is, or adapt as they see fit.


# Goal
a collection of packages that users can copy to the XMX/SSP

# Status
In progress, just setting up the project.


# To Do
Everything :) 

- go thru each project (simplest first), for each
- see if they use, or are close to e301 mod.mk or tutorial.mk, if they are copy/adapt to use my newer version (best case!)
- test build, ensure build artifacts are git ignored etc... keep 'local' i.e not in external folders, adapt if necessary
- for more 'custom builds' look to each, and find best migration path
- result : ensure I can build all projects, and that thier binaries are correct architure
- create a package directory that I can release, store on github e.g. ./releases/xmx ./releases/ssp
- one single xmx/ssp zip file for all as well? would be easier for me?!?
- find faust2er301, used by ljwall


# Done
- create this repo with other repos as submodules
- create a percussa branch on each
- remove er-301 sdk if present as submodule etc, to avoid 'confusion'

i.e. not a lot ;)



# Observations
## build for darwin...
need to set ARCH, needs revisting
```
cmake . -B build -DARCH=darwin  
```

## Darwin - Build Status, not tested
ljwall/chaoticmods - ok
ljwall/oscillator - ok
ljwall/faustian - FAIL uses faust2er301
ljwall/faust-poc - FAIL uses  faust2er301
supernicd - ok 
yrn1  - ok 
tmfset - ok
solomine - ok

## Crosscompile SSP - Build Status, not tested
ljwall/chaoticmods
ljwall/oscillator 
ljwall/faustian - FAIL uses faust2er301
ljwall/faust-poc - FAIL uses  faust2er301
supernicd - 
yrn1  - 
tmfset - 
solomine - 

# notes for particular projects


## ljwall
based on tutorial.mk, works except for those using faust

## supernicd
own makefile, but quite simple as its bulding one package
converted to use tutorial.mk, remove bak/zip, used local includes rather than homedir!

## yrn1 / tmfset
basically same approach... yrn1 a litle simpler
makefile talks of docker image, but thats for remote build.
these were supporting darwin, and just needed LFLAGS updated.
BUT the mod_builder.mk, env.mk, mod.mk are all 'own' copies, so wont work with cross compile.

question is why? they still need er301 sdk, 
perhaps modded due to docker requirement?

# solomine
updated itws own env.mk to pull into my sdk/scripts/darwin.mk

