slxc
====

SLURM in Linux Containers

The set of scripts to easily deploy SLURM cluster on one machine using Linux Containers.
The goal is SLURM development mostly. Any other ideas/usages :)?

Prerequisites: screen tool.
<p>
1. Install Linux Containers (LXC)
2. Configure LXC (for Ubuntu and Mint):
2.1 Setup lxc networking (/etc/default/lxc-net):
    USE_LXC_BRIDGE="true"
    LXC_DHCP_CONFILE=/etc/lxc/dnsmasq.conf
    LXC_DOMAIN="lxc"
2.2 Change /etc/lxc/dnsmasq.conf adding following line:
    conf-file=SLXC_PATH/build/dnsmasq.conf
3. Install Munge in MUNGE_PATH (under someuser)
4. Install SLURM in SLURM_PATH (under someuser)
5. Configure SLURM and put its configuration in $SLURM_PATH/etc/slurm.conf
5. Put SLURM and Munge installation paths to $SLURM_SLXC/slxc.conf
6. Set SLURM_USER to "someuser".
7. Create cluster machines:
    # Create frontend first
    $SLXC_PATH/slxc-new-node.sh frontend
    # Create node machines
    for i in `seq 1 n`; do $SLX_PATH/slxc-new-node.sh cn$i; done
8. [Optional] Add Munge and SLURM installation paths to your PATH env variable.
    And "export SLURM_CONF=$SLURM_PATH/etc/slurm.conf" to let sinfo/sbatch
    and others know how to reach slurmctld.
9. Restart lxc-net service (in Ubuntu and Mint):
    sudo service lxc-net restart
10. Start your cluster:
    sudo ./slxc-run-cluster.sh
11. Verify that everything is OK:
    sudo screen -ls
    sudo lxc-ls --active
    Both tools should show all your machines
12. Now you can attach to any machine with
    sudo lxc-attach -n $nodename
13. To shutdown your cluster use
    ./slxc-stop-cluster.sh
    NOTE: that it may take a while. You can speedup this process by setting
	LXC_SHUTDOWN_TIMEOUT in /etc/default/lxc (for Ubuntu and Mint)
</p>
That seems to be all. Enjoy!