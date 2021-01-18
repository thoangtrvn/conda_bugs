
# When you try to update python on base environment

```
conda install python=3.7    
```
you may get an error that there are conflicting packages. You may be at the state in that there are so many of them, and 
it takes forever for conda to resolve.

Try to revert back to an older revision, and then run 
``` 
conda update --all
```
on a smaller size of conflicting packages.


# SOLUTION

Backup 

```
  conda env export > base_environment.yml 
  cp ~/anaconda3/conda-meta/history{,_backup}
  conda list -n base -r > conda_base_revisions.txt
```

Revert to the earliest revisions

```
  conda install --revision [num]
```
 
You can choose a version that works, for me I just choose '2' (don't choose 0 as it will destroy your conda)

```console
  CondaUpgradeError: This environment has previously been operated on by a conda version that's newer
than the conda currently being used. A newer version of conda is required.
  target environment location: /Users/tmhoangtus.ibm.com/anaconda3
  current conda version: 4.5.11
  minimum conda version: 4.8
```
Solution: remove any references to 4.8 in ~/anaconda3/conda-meta/history file

After you revert, you may have problems with 
1. pip versions
2. conda versions
3. boto3

We probably still have some packages with conflict. Then run
```
conda update --all                                                                                                      
```

## Fix pip

```
conda install pip 
```
ERROR:
```
CustomValidationError: Parameter channel_priority = 'strict' declared in <<merged>> is invalid.
The value 'strict' cannot be boolified.
```
Solution: remove the 'strict' from ~/.condarc file

## Fix boto3

```console 
boto3.client('s3')  # https://github.com/conda/conda/issues/8993
AttributeError: module 'boto3' has no attribute 'client'
```
Solution:
```
pip uninstall boto3
pip uninstall botocore
conda uninstall awscli
pip uninstall s3transfer
pip uninstall urllib3
```
# Fix corrupted package(s)

You may have to remove, before reinstall certain packages, e.g. itermocil

```
importlib_metadata.PackageNotFoundError: No package metadata was found for itermocil
```

Solution:
```
brew remove itermocil 
rp ~/anaconda3/bin/itermocil
brew install install TomAnthony/brews/itermocil 
```
