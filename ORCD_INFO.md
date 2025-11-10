# LOGGING INTO ENGAGING
1. In the terminal:
	- `ssh orcd-login`
		- if you haven't already, you can [set up an ssh key](https://code.visualstudio.com/docs/remote/troubleshooting#_configuring-key-based-authentication) so that you don't have to enter password each time
	- duo push authentication
	- `orcd-login` is an alias for the `orcd-login001` hostname I set in my ssh config file (`C:\Users\munib\.ssh\.config)
	- in `settings.json` file, you can specify that the login node is linux so you don't have to specify it each time (`C:\Users\munib\AppData\Roaming\Code\User\settings.json)
2. allocate resources
	- check which partitions you have access to with `sinfo` and see a [summary of each node's resources here](https://orcd-docs.mit.edu/running-jobs/available-resources/)
		- `mit_normal` for cpu-only and longer batch jobs
		- `mit_quicktest` for short batch jobs and interactive jobs that only need a cpu
		- `mit_normal_gpu` for batch and interactive jobs that need a gpu
		- Flavell lab has access to `bcs` partitions. [see here for resources](https://github.mit.edu/MGHPCC/OpenMind/wiki/User-guide-for-BCS-computing-resources-on-Engaging#bcs-compute-nodes)
			- d
	- Requesting a single core for an interactive job for 1 hour
		- `salloc -t 01:00:00 -p mit_normal`
	- Check current job status with `squeue --me`
	- Stop a job with `scancel [jobid]`
		 - stop multiple with `scancel [jobid], [jobid2], ..`
		 - stop all user's jobs with `scancel [username]`
	- Retrieve job stats with `sacct`
	- See here about info for [requesting resources](https://orcd-docs.mit.edu/running-jobs/requesting-resources/)
		- tasks, nodes, tasks per node, memory, cores
		- a guide to finding how much memory your job will need
		- GPU requests
		- [another nice resource on cores and such](https://login.scg.stanford.edu/faqs/cores/)
3. ssh into compute node in VS code (terminal will automatically switch from login to compute if you set `ProxyJump` to `orcd-login` (or whatever the login node `HostName` is set to in your ssh config file).
	- `ssh orcd-compute` __OR__
	- `Connect to remote host` in VS code

# USING PYTHON
- `module load miniforge`
- create a conda environment or load one

# USING JUPYTER NOTEBOOKS

# DATA STORAGE
- we have 60TB available at `/orcd/data/flavell/001`
	- unclear if backed up
- each user has !TB available at `/orcd/home/002/munib/orcd/pool`
	- not backed up
- each user's home directory at `/orcd/home/002/munib` has 200GB
	- backed up
- each user's scratch directory at `/orcd/home/002/munib/orcd/pool` has 1TB
	- not backed up
- check how much available storage in a directory `df -h /orcd/data/flavell/001`
-  TODO: add info about flv store 1 here

# FILE TRANSFER
The most recommended method to transfer data is Globus because it is safe and efficient. You can do all operations on a web browser on your computer. Refer to [Globus user guide](https://docs.globus.org/guides/tutorials/manage-files/transfer-files/) for details. Use the endpoint mithpc#openmind for OpenMind and use the collection MIT ORCD Home Collection for Engaging. (Might also be MIT Engaging Collection)

You can also consider using `rclone` to transfer data. Refer to the `rclone` section on [this page](https://github.mit.edu/MGHPCC/OpenMind/wiki/How-to-transfer-files%3F).

It is less recommended to use `rsync` to transfer data because the transfer speed is relatively slow. If you want to use rsync for some reason, refer to [this page for details](https://github.mit.edu/MGHPCC/OpenMind/wiki/Use-rsync-to-transfer-files-between-OpenMind-and-Engaging).

Can also just use filezilla

# GIT AND GITHUB
1. [Follow these instructions to make an SSH key](https://orcd-docs.mit.edu/faqs/#how-do-i-use-git-on-the-cluster)
2. Initialize a git repo:
	- `git init`
3. Switch your remote to SSH:
	- `git remote set-url origin git@github.com:MunibH/g5ht-pipeline.git`
4. Test:
	- `ssh -T git@github.com`
	- Should say __successfully__ __authenticated__
5. Can you now stage, commit, and push

### Merge new code from a different branch
- `git checkout <branch>` (if not already on desired branch)
- `git fetch origin` (fetches all branches)
- `git merge origin/homepc`
- `git push origin <branch>`

# SHELL ALIASES

## My aliases
  - `cdout`: cd to OUPUT DIRECTORY	
  - `cdg5code`: cd to g5ht-pipeline code directory
  - `cdg5data`: cd to g5ht data directory 
  - `sqme`: check job status (alias for `squeue --me`)
  - `sa1`: allocate ou_bcs_normal for 1hr (also have `sa2`, `sa3`, `sa4`)
  - `sjout` and `sjerr`: prints tail of output and error files, respectively. usage `sjout <jobid>`
    - alias sjout='f(){ scontrol show job "$1" | awk -F= "/StdOut=/{print \$2}" | xargs tail -f; }; f'
    - alias sjerr='f(){ scontrol show job "$1" | awk -F= "/StdErr=/{print \$2}" | xargs tail -f; }; f'


### Make a new one
- open bashrc
  - `nano ~/.bashrc`
  - add alias, for example:
    - `alias cdout='cd /home/user/my_project/output'`
  - save .bashrc
    - `Ctrl+O`, `Enter`, `Ctrl+X`
  - `source ~/.bashrc`


# OTHER
- max jobs you can submit on Engaging is 500
	- in practice I found it was only 440 (I had one job open which was the original allocation on the login node)
		- `sacctmgr show user $USER withassoc`