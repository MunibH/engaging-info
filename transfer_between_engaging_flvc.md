
- Before committing, do:

`rsync -avh --dry-run munib@orcd-login001:/orcd/data/flavell/001/g5ht/DATA/ ~/g5ht_DATA/`

- run on local PC

```
rsync -avh --progress \
  -e "ssh" \
  munib@orcd-login001:/orcd/data/flavell/001/g5ht/DATA/ \
  munib@flv-c3:/home/munib/store1/shared/g5ht/data/
```

- run these on local PC prior to long transfers

`tmux new -s g5ht-transfer`

Detach (leave it running)

Press:

Ctrl-b then d


You’ll return to your normal shell.

Reattach later
tmux attach -t g5ht-transfer


# NOTE
- have to use rsync in MSYS terminal, not powershell


```
rsync -avh --progress \
  -e "ssh" \
  munib@orcd-login001:/orcd/data/flavell/001/g5ht/DATA/20260123 \
  munib@flv-c3:/home/munib/store1/shared/g5ht/data/
```


# actually have to do this

SSH into the destination: ssh munib@flv-c3

Run the rsync command there: 
`tmux`

`rsync -avh --progress munib@orcd-login001:/orcd/data/flavell/001/g5ht/DATA/20260123 /home/munib/store1/shared/g5ht/data/`

if ssh connection drops, can just tmux back in to see status:
`tmux attach`