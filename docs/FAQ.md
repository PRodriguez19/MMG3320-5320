## Frequently Asked Questions

On this page I will update as they come common problems experienced by students when using the VACC cluster with suggestions on how to resolve them. 

### Why do I see `$'\r':command not found`? 

It is likely that you created the job script on a Windows machine, which uses different newline characters. To overcome this, you can try the following command: 

```bash
sed -i 's/\r$//' your-script-name.sh
```

then run: 

```bash
sh your-script-name.sh
```

### Setup of SRA tools 

When running SRA Tools for the first time it is necessary to run `vdb-config --interactive` and then press 'x' to save a basic configuration. This is not necessary for further usage. 

### Why do I see `Unable to locate a modulefile for 'gcc/'`? 

It is most likely that you are logging into the "old" VACC cluster. This class has transitioned to using the "new" VACC cluster which can be accessed with: 

```
ssh uvmid@login.vacc.uvm.edu

#replace uvmid id with your netid
```

The website for the new VACC cluster on OpenOnDemand can be found [here](https://ondemand.vacc.uvm.edu/pun/sys/dashboard)



