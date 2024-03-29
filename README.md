# mlcoalsim version 1.9915b (20210424)

## usage: [input file] [output file]

##### mlcoalsimX "Multilocus coalescent simulation program"

mlcoalsim v2 performs coalescent simulations under several demographic and also using a single event selective model. In this version, the parameters can be included in separated prior files. 

Also, it calculates simulations using more than one processor (defined by the user). Furthermore, the input file has been changed significantly in relation to the fist version. The Manual of use is the same than in mlcoalsim-v1 (https://github.com/CRAGENOMICA/mlcoalsim-v1) , except for a given parameters and installation indicated bellow:

## COMPILE:
	gcc *.c -lm -o mlcoalsimX -Wall -pedantic -DZNS_ACTIVE -O3
	OR (for parallel version)
	mpicc *.c -lm -o mlcoalsimXmpi -Wall -pedantic -DZNS_ACTIVE -O3 -DinMPI 

The inclusion of ZnS statistics with the option "-DZNS_ACTIVE" can reduce significantly the speed of the simulations if the number of variants is high.

RUN:
./mlcoalsimX [input.txt] [output.txt]
OR (for parallel varsion)
mpirun -np [n_process] ./mlcoalsimXmpi [input.txt] [output.txt]


Examples:
Run the examples. Go into the folder example and run: 

cd ./examples00/
sh ./run_mlcoalsim00.sh
cd ..

cd ./example01/
./mlcoalsimX ./Example1locus.txt ./Example1locus.out
cd ..

cd ./example02/
sh ./run_mlcoalsim02.sh
cd ..

cd ./example10/
mpirun -np 10 ./mlcoalsimXmpi ./Example10loci.txt ./outputs/Example10loci.out
cd ..


## New input parameters and input format (specially for pop_event) in version2:

	{"linked_fixsegsites"}, /*in case linked loci, indicate the number of segregating sites. They will be fixed in simulations (for a specific region and population)*/
	{"linked_fixrm"},	/*in case linked loci, indicate the number of min. rec. events. They will be fixed in simulations (for a specific region and population)*/
	
	{"heter_theta_alphag"}, /*alpha=p(lambda) parameter from gamma to indicate the heterogeneity in theta values across the locus, (vector) for each independent locus, or 1 value for the entire linked region*/
	{"heter_rec_alphag"},   /*alpha=p(lambda) parameter from gamma to indicate the heterogeneity in recombination values across the locus, (vector) for each independent locus, or 1 value for the entire linked region*/
	
	{"linked_fixnhapl"},	/*in case linked loci, indicate the number of haplotypes. They will be fixed in simulations (for a specific region and population)*/
	{"correct_gamma"},	/*use in case ifgamma = 1. the result of the gamma distribution is multiplied by this value*/
	{"correct_gammar"},	/*use in case ifgammar = 1. the result of the gamma distribution is multiplied by this value*/
	{"invar_mut_sites"},	/*number of positions that are considered invariable. The algorithm will calculate (as an average) the number of invariable positions for each iteration.*/
	
	{"sfix_pop"},		/*in case sfix_all_thetas and several populations. indicate the population where the S will be fixed (starting from pop 0 to n-1)*/
	{"rmfix_pop"},		/*in case rmfix and several populations. indicate the population where the S will be fixed (starting from pop 0 to n-1)*/
	
	{"mig_rate_matrix"},	/*in case several pops, indicate the migration rate (4Nm) for each popi vs popj. Indicate the entire matrix, with 0 in the diagonal. Indicate for all pops (not only the pops sampled)*/
	
	{"linked_nregion_fixsegsites"},	/*in case fixing seg sites in liked loci, indicate the region (starting from 0) where the positions will be fixed*/
	{"linked_nregion_fixrm"},		/*in case fixing Rm in liked loci, indicate the region (starting from 0) where the positions will be fixed*/
	{"linked_nhapl_fixnregion"},	/*in case fixing nhapl in liked loci, indicate the region (starting from 0) where the positions will be fixed*/
	
	{"nprior"},		/*indicate the number of the prior (starting with 1). (The same number preceeded by & has to be found in any of the evolutionary parameters included in the file)*/
	{"prior_type_var"},	/*0 integer, 1 is double*/
	{"prior_type_dist"},	/*0 uniform, 1 log10-uniform, 2 gamma, 3 beta*/
	{"prior_parameters"},	/*for 0 and 1 two values (min and max), for 2 three values (alpha, p, and factor_corr), for 3 four values (alpha, beta, min and max)*/
	{"prior_seed"},		/*pseudorandom number for generating the distribution*/
	{"prior_file"},		/*file with prior numbers*/

	{"npop_events"},	/*in order to generate events (changing mig_rates, coalesce pops, changing pop_sizes...) indicate the number of events*/
	{"pop_event"},		/*matrix that contains 'npop_events' columns separated by commas, each with 5 values separated by spaces/tabs + 2 vectors: first the number of the event (starting from 0!), */
							/*second and third, the populations implicated in the event (if only one, type the same number twice, if second is negative, split pop)*/
							/*Next, the the pop_size of the 'new' pop (relative to N). Warning: the 'new' pop must be the number of the smallest pop number when two pops are implicated*/
							/*Next, the time (in 4N gen) the event occurred. Note that this value is relative to the other events occurred before. First, the time is counted from events from 'nint', then, all the npop_events in sucesive order*/
							/*Finally, indicate the migration vector of the new pop(i) i->rest and the vector rest->i in the correct order, with n values (all n population from starting simulation, and writing 0 to the extinct pop and the diagonal)*/
							/*in case split, two events have to be defined: the first event needs pop1 from split and pop2 a negative value (the new pop), factor pop for the current pop and the time of split, and migration vectors for the current pop*/
							/*in case split (cont) second event: pop1=pop2 the new pop, factor pop for the new pop, time (not counted) and the migration vectors for the new pop*/
	
	{"ancestral_pol"},	/*First value: n>1 (n is the number of pops used, the last is the outgroup). For n=3, the values are: 
					Second: number of populations in group 0 ns0: indicate the number of the populations (ns0 values). 
					Third: number of populations in group 1 (ns1): indicate the number of the populations (ns1 values). 
					Fourth: number of populations in outgroup (nso): indicate the number of the populations (nso values).*/

	{"patcg"},		/*if included: proportion of ATCG (from 0 to 1) sum must be 1. Only used in case print_matrixpol is 1*/
	{"pop_outgroup"},	/*if >= 0 indicates the population defined as outgroup, by defect is -1*/
	{"pevent_sexratio"},	/*indicate the pop_event number where the factor_chrn changes (e.g., change in male/female ratio) and the values of factor_chrn (two values for each event, no more changes than events)*/
	{"sex_ratio"},		/*ratio homogametic/heterogametic*/
	{"fixmax_abstime_event"},/*in some events it is necessary to fix a maximum absolute time the event occurred*/
	
## New statistics in output files:
	{"_ThetaL.out"},
	{"_ZengE.out"},
	{"_EW.out"},
	{"_Fstw.out"},
	{"_fixoutg.out"},
	{"_KoutgJC.out"},
	{"_ThetaWA.out"},
	{"_ThetaWAn.out"},
	{"_ThetaTA.out"},
	{"_ThetaTAn.out"},
	{"_AchY.out"},
	{"_AchYn.out"},
	{"_msdev.out"},
	{"_mskew.out"},
	{"_mkurt.out"},
	{"_ragg.out"},
	{"_ZnS.out"},
	{"_ZZ.out"}

## INCLUDE SELECTION generation by generation, backward.

mlcoalsim can trace a single selective event by simulating this using a forward simulation and then use the tree history at this position to go backward from coalescent process.

### parameters in input file: 
	{"sendt"}, 		/*time at the selective event that selection finish (from present to past)*/
	{"sfreqend"}, 		/*frequence of the selective position at which the selection finish (start from past to present)*/
	{"sfreqinit"}, 		/*frequence of the selective position at which the selection start (finish from past to present)*/
