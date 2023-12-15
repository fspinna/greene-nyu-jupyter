# greene-nyu-jupyter

## Launching a Jupyter Notebook on the Greene Cluster compute nodes

### Preliminaries
Be sure to have a conda environment (`<EnvironmentID>`) with Jupyter Notebook installed.

### 1. Create a Shell Script
First, create a shell script (`sh` file) to launch the Jupyter Notebook on the control node. Use the following template, replacing `<NetID>` and `<EnvironmentID>` (conda environment) with your specific details:

```bash
#!/bin/bash
#SBATCH --job-name=jupyter
#SBATCH --gres=gpu:1
#SBATCH --time=00:20:00
#SBATCH --mem=8GB
#SBATCH --output=/home/<NetID>/jupyter.log

source /home/${USER}/.bashrc
source activate <EnvironmentID>

cat /etc/hosts
jupyter notebook --port=8080 --no-browser
```

### 2. Run the Script
Launch the script on the control node and wait for it to execute. 
```bash
sbatch <ScriptName>.sh
```

Monitor its status using:
```bash
squeue -u $USER
```

Write down the NodeID from the NODELIST(REASON) column.

### 3. Access the Log File
Open the log file located at `/home/<NetID>/jupyter.log` to retrieve the Jupyter Notebook access token. If you've set up a password, this step is not necessary.

### 4. Retrieve the Access Token
The log file will contain a URL with a token similar to:

```
http://localhost:8080/?token=your_access_token
```

Copy the token (`your_access_token`).

### 5. Locate the Node Name
Identify the node name either from the web interface, the log file, or by checking your queue.

### 6. Establish SSH Tunneling
From your local terminal, establish an SSH tunnel to the node using:

```bash
ssh -o UserKnownHostsFile=/dev/null -t -t <NetID>@greene.hpc.nyu.edu -L 8080:localhost:8080 ssh <NodeID>.hpc.nyu.edu -L 8080:localhost:8080
```

Replace `<NetID>` and `<NodeID>` with your specific details.

### 7. Access Jupyter Notebook
In your browser, navigate to `http://localhost:8080` and enter the previously copied token to access the Jupyter Notebook.
