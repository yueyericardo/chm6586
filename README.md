Use `gau h2o` to submit a job (h2o.com)  
instead of `vim h2o.sh` , edit, then `sbatch h2o.sh`

- create a file in `~/script/gau.sh`
```bash
mkdir ~/script
cd ~/script
touch gau.sh
```
- copy the code below to gau.sh, you need to change `youremail@ufl.edu` to your own email address.
   ( you can either use vim or edit in the way you know)  
   If you want to use vim,  `vim gau.sh`, press `i`(this is insert mode), copy and paste, press `Esc`, input `:wq` , press`enter`. If you are not familiar with `vim`, google it.  
   You can use `cat gau.sh` in terminal to make sure whether the script is right.
```bash
#!/bin/bash
#SBATCH --job-name=ch2o
#SBATCH --mail-type=ALL
#SBATCH --mail-user=youremail@ufl.edu
#SBATCH --nodes 1
#SBATCH --ntasks 1
#SBATCH --cpus-per-task=1
#SBATCH --mem-per-cpu=2gb
#SBATCH -t 0:30:00

# Going to this job's submit directory
cd $SLURM_SUBMIT_DIR

module load gaussian

for i in "$@"; do
    time g09 < $i.com > $i.log
done
```
- Add the code below to the bottom of the file in ~/.bashrc. (This is to set a alias for `gau`)
```bash
alias gau='sbatch ~/script/gau.sh'
```

- After that, input `source ~/.bashrc` in your terminal  
  Now `gau h2o` should working. Note that `gau` only the filename(h2o), not including extension (h2o.com).

- if you have `first.com` `second.com` `third.com` .You can also use `gau first second third `to run all of them. So you can do a lot job in a single line, but I will recommend not run so much at once, this will use a lot memeory, 3 or 4 at once may be a good choice.

another way of alias
```
gau() {
    qsub ~/script/gau.pbs -v file=$1
}
```
