---
title: Resources & Support
excerpt: >-
  The page provides a series of resources that can help you learn more about
  GridLAB-D™.
---
# Source

The source content for GridLAB-D™ is maintained on the GridLAB-D™ [GitHub](https://github.com/gridlab-d/).

# Publications

To see what projects use GridLAB-D™, and what tools complement it, check out our **[Projects](doc:projects)** page.

# Discussion Forums

The troubleshooting guide may not always point you to an obvious solution. If it doesn't, we have help forums that are monitored by our developers and power users. Please visit our **[Forum](https://gridlab-d.readme.io/discuss#/)**.

If you are unable to answer your question on the forum or feel an issue is more appropriate, feel free to post a new issue on our [GitHub issue tracker](https://github.com/gridlab-d/gridlab-d/issues). We will try to respond as soon as possible.

An archive of our original forum is preserved at [SourceForge](https://sourceforge.net/p/gridlab-d/discussion/) for \~8 years of questions and answers to see if someone has asked the same question before.

# Models

[Prototypical Circuits](https://github.com/gridlab-d/Taxonomy_Feeders): A repository for the taxonomy of prototypical feeders, which were developed in order to give researches in academia, as well as industry, access to a set of detailed distribution feeder models that could be used to study and evaluate smart grid technologies. These feeders are representative of those found in the Continental United States and are designed to work with GridLAB-D™ version 2.0 and higher.

[PG\&E Prototypical Circuits](https://github.com/gridlab-d/Taxonomy_Feeders/tree/master/PGE_Models): PG\&E and CEC have volunteered to provide the GridLAB-D™ community with a set of 12 prototypical feeder models obtained from a k-means cluster analysis of PG\&E's 2,700 primary distribution feeders. These are statistically representative of PG\&E's system and have been converted from the native CYMDIST formats to GridLAB-D™ formats. More information about those models can be found at the [Shoutwiki](http://gridlab-d.shoutwiki.com/wiki/PGE_Prototypical_Models) site.

[IEEE Models](https://github.com/gridlab-d/tools/tree/master/IEEE%20Test%20Models): As part of the [IEEE Distribution System Analysis Subcommittee's](http://sites.ieee.org/pes-dsacom/) Test Feeder Working Group, we strive to validate our tools against others. We keep a library of these models in GridLAB-D™ format for others to use. For more information about the models, please see the [Test Feeder Working Group](http://sites.ieee.org/pes-testfeeders/) website.

# Other great tools

We try to push as many of our tricks, tools, scripts, etc. out to the open-source repository, too. Here's a brief list of some of the key ones:

[Equipment Database](https://github.com/gridlab-d/tools/tree/master/equipment_database): Model files with a list of standard overhead and underground line conductors, generators, and transformers. Nice if you just want some standard configurations or equipment types.

[Conversion Scripts](https://github.com/gridlab-d/tools/tree/master/conversion_scripts): Converts models from CYME and Synergee into GridLAB-D™ format. These are getting a little long in the tooth and are not highly reliable, as they were created a number of years ago, but they can be a good starting point. New efforts are updating these, typically as Python scripts or other processes. See NRECA's [Open Modeling Framework](https://github.com/dpinney/omf/tree/master/omf) or PNNL's [GridAPPS-D](https://github.com/GRIDAPPSD/Powergrid-Models) projects for other examples, although there are other efforts out there, too.

[MATLAB scripts](https://github.com/gridlab-d/tools/tree/master/matlab_scripts): A couple handy scripts that some of the developers use when debugging.

[Autotest](https://github.com/gridlab-d/gridlab-d/): If you are looking for a specific example of an object, the "autotest" folders might be one of the best places to do this. As part of our [unit testing](http://gridlab-d.shoutwiki.com/wiki/Validate), we create relatively simple sample models with defined solutions. If the solution changes, developers know they may have made a mistake. Over time, the team has created 500+ of these tests. The objects in these models are great for getting a skeleton of an object or seeing a variety of ways an object might be used. You will typically need to remove the [assert object](http://gridlab-d.shoutwiki.com/wiki/Assert_User_Guide) and module, but otherwise the GLM should be a great starting point for understanding what you can and should set in a specific object. To find these, navigate to the appropriate module (e.g., powerflow) in the source code, then select the autotest folder.

# Download

Please visit the GridLAB-D™GitHub [repository](🔗) to obtain the latest releases for Windows, Linux, and Mac.

# Questions

If you have questions or need support please [email](🔗) the GridLAB-D™ team.