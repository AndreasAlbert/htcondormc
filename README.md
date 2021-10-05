# htcondormc
Collection of scripts to generate CMS simulation samples using the lxplus HTCondor cluster.

## General framework

For each sample, you need to provide the following:
* A GEN fragment. Check out the `fragments` folder for examples. For existing samples, you can go to MCM, navigate to the GEN step request and click on "get fragment" to see the fragment used there.
* A script that chains together all the cmsDriver commands for the different steps you want to generate. Generally, these steps will depend on the experimental conditions. For example, the `run_wmLHEGS_DRPremixNanov7_Fall17.sh` script is used for Fall17 production starting from an MG gridpack going up to NanoAOD v7. Almost the entire content of this script is taken directly from MCM. To get a new campaign going, go to the campaign you are interested in on MCM, steal the cmsDriver commands from there, and make up a new script. You will slightly have to tweak file input/output names in the cmsDriver commands. Look at existing scripts for inspiration. Note that this script can often be reused for multiple samples, so you do not always have to make a new script.
* If you want to run PU mixing, you also need to provide a list of files to use for PU mixing (see `pulist_*.txt`) in the correct campaign. The PU list might already exist in the repository, and can be reused between samples of the same campaign. If it does not exist, go to the DRPremix step of the campaign you are interested in, and look at the data set name that is given for PU input there. You can then get the file names for this PU data set using dasgoclient. 
* The final step is to write a `jdl` file that describes the condor job you are about to submit. Check out the `jdl` directory for inspiration. After you have made yourself familiar with the steps above, the jdl file should be self-explanatory. Make sure to change output paths here to write to an area you control. You can submit the job with `condor_submit my_jdl_file.jdl`.

NB: If you make up a new workflow, it is highly recommended to submit a single small test job to verify that your setup is OK. To do this, it is particularly useful to use the `condor_submit` commandline syntax, which allows you to overwrite config parameters on the fly, e.g.:
`condor_submit my_jdl_file.jdl -a njobs=1 -a nevents=10`
