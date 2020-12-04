# Ligand Preparation using openbabel:

## Recommendation: Use PyRx to miinimize ligands first to find out the errors and then remove those ligands from the ligand main file.


##### Step 1: Type following  in commandline to install openbabel

```sudo snap install openbabel```

##### Step 2: Minimize the ligands using openbabel.obminimize command as follows

```openbabel.obminimize -ff MMFF94 -n 2000 -sd -c 1e-7 -o sdf fda_short.sdf > fda_min.sdf```


##### Step 3: Split the minimized file to a pdbqt directory as follows. Remember to add -m to perform splitting.


``` openbabel.obabel -i sdf -o pdbqt -m -O ligand_.pdbqt fda_min.sdf ```


# Protein preparation:

##### Step 1: Use Discovery Studio to remove ligands from crystal structure and hydrogen.
##### Step 2: If possible add the missing residues add and do the loop modelling to get the compelete structure candidate for docking.
##### Step 3: Minimize the structure using Swiss PDB Viewer to optimize the structure.
##### Step 4: Use PyRx to convert the structure to pdbqt format for docking and find grid box. 

## Recommendation: Perfrom docking for one molecule. This would generate the conf.txt file for the use of vitual screening.
## Also, the same ligand can be used in linux as a control for the process in linux.



# Docking in Autodock vina

### Use the script 'vina_screen_local.sh' for docking. Use --cpu if necessary. Normally the program finds that on it own.

``` #! /bin/bash

for f in ligand_*.pdbqt; do
    b=`basename $f .pdbqt`
    echo Processing ligand $b
    mkdir -p $b
    vina --config conf.txt --ligand $f --out ${b}/out.pdbqt --log ${b}/log.txt
done
```
