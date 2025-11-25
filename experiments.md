## DVC Dataset Versioning Experiments

### Going one commit back to original data
```bash
rm -rf data/ames_data_2006_2008.parquet
(mlops-course-agh-lab-05) julia@julia-zenbook:~/Documents/MLOps/mlops-lab5$ dvc checkout
Building workspace index                                                                                         |2.00 [00:00, 85.5entry/s]
Comparing indexes                                                                                                |4.00 [00:00,  998entry/s]
Applying changes                                                                                                 |1.00 [00:00,   330file/s]
A       data/ames_data_2006_2008.parquet
(mlops-course-agh-lab-05) julia@julia-zenbook:~/Documents/MLOps/mlops-lab5$ git checkout HEAD~1
Note: switching to 'HEAD~1'.

You are in 'detached HEAD' state. You can look around, make experimental
changes and commit them, and you can discard any commits you make in this
state without impacting any branches by switching back to a branch.

If you want to create a new branch to retain commits you create, you may
do so (now or later) by using -c with the switch command. Example:

  git switch -c <new-branch-name>

Or undo this operation with:

  git switch -

Turn off this advice by setting config variable advice.detachedHead to false

HEAD is now at 9979b1b track ames_description.txt with DVC
(mlops-course-agh-lab-05) julia@julia-zenbook:~/Documents/MLOps/mlops-lab5$ dvc checkout
Building workspace index                                                                                         |3.00 [00:00,  139entry/s]
Comparing indexes                                                                                                |4.00 [00:00,  983entry/s]
Applying changes                                                                                                 |1.00 [00:00,   301file/s]
M       data/ames_data_2006_2008.parquet 
(mlops-course-agh-lab-05) julia@julia-zenbook:~/Documents/MLOps/mlops-lab5$ uv run ames_inspect_data.py --file-path "data/ames_data_2006_2008.parquet"
     Order        PID  MS.SubClass MS.Zoning  Lot.Frontage  Lot.Area  ... Misc.Val Mo.Sold Yr.Sold Sale.Type Sale.Condition SalePrice
989    990  526351030           20        RL          87.0     11029  ...        0       5    2008       WD          Normal    176500
990    991  526353050           20        RL           NaN     12925  ...        0       5    2008       WD          Normal    237500
991    992  526354070           60        RL          85.0     11075  ...        0       6    2008       WD          Normal    206900
992    993  527105050           60        RL          72.0      8702  ...        0       4    2008       WD          Normal    187500
993    994  527106050           60        RL          65.0      8139  ...        0      10    2008       WD          Normal    165000

[5 rows x 82 columns]
(mlops-course-agh-lab-05) julia@julia-zenbook:~/Documents/MLOps/mlops-lab5$ 
```

### Going back to the latest git (and dvc) version - cleaned data

```bash
(mlops-course-agh-lab-05) julia@julia-zenbook:~/Documents/MLOps/mlops-lab5$ git status
HEAD detached at 9979b1b
Untracked files:
  (use "git add <file>..." to include in what will be committed)
        experiments.md

nothing added to commit but untracked files present (use "git add" to track)
(mlops-course-agh-lab-05) julia@julia-zenbook:~/Documents/MLOps/mlops-lab5$ git chekout main
git: 'chekout' is not a git command. See 'git --help'.

The most similar command is
        checkout
(mlops-course-agh-lab-05) julia@julia-zenbook:~/Documents/MLOps/mlops-lab5$ git checkout main
Previous HEAD position was 9979b1b track ames_description.txt with DVC
Switched to branch 'main'
Your branch is up to date with 'origin/main'.
(mlops-course-agh-lab-05) julia@julia-zenbook:~/Documents/MLOps/mlops-lab5$ uv run ames_inspect_data.py --file-path "data/ames_data_2006_2008.parquet"
     Order        PID  MS.SubClass MS.Zoning  Lot.Frontage  Lot.Area  ... Misc.Val Mo.Sold Yr.Sold Sale.Type Sale.Condition SalePrice
989    990  526351030           20        RL          87.0     11029  ...        0       5    2008       WD          Normal    176500
990    991  526353050           20        RL           NaN     12925  ...        0       5    2008       WD          Normal    237500
991    992  526354070           60        RL          85.0     11075  ...        0       6    2008       WD          Normal    206900
992    993  527105050           60        RL          72.0      8702  ...        0       4    2008       WD          Normal    187500
993    994  527106050           60        RL          65.0      8139  ...        0      10    2008       WD          Normal    165000

[5 rows x 82 columns]
(mlops-course-agh-lab-05) julia@julia-zenbook:~/Documents/MLOps/mlops-lab5$ dvc checkout
Building workspace index                                                                                         |3.00 [00:00,  218entry/s]
Comparing indexes                                                                                               |4.00 [00:00, 1.07kentry/s]
Applying changes                                                                                                 |1.00 [00:00,   254file/s]
M       data/ames_data_2006_2008.parquet
(mlops-course-agh-lab-05) julia@julia-zenbook:~/Documents/MLOps/mlops-lab5$ uv run ames_inspect_data.py --file-path "data/ames_data_2006_2008.parquet"
    MSSubClass MSZoning  LotFrontage  LotArea  Street  Alley  ...  MiscVal MoSold  YrSold SaleType  SaleCondition SalePrice
989       SC20       RL         87.0    11029       2      0  ...        0    May    2008      WD          Normal    176500
990       SC20       RL          0.0    12925       2      0  ...        0    May    2008      WD          Normal    237500
991       SC60       RL         85.0    11075       2      0  ...        0    Jun    2008      WD          Normal    206900
992       SC60       RL         72.0     8702       2      0  ...        0    Apr    2008      WD          Normal    187500
993       SC60       RL         65.0     8139       2      0  ...        0    Oct    2008      WD          Normal    165000

[5 rows x 80 columns]
(mlops-course-agh-lab-05) julia@julia-zenbook:~/Documents/MLOps/mlops-lab5$ git status
```

**Note:** Important to run `dvc checkout` to sync the dataset with the .dvc pointer in git