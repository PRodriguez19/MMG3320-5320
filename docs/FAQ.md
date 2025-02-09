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

