# Snapshots and Images


## Managing Snapshots using Azure Portal:

- Step 1: Log in to the Azure Portal.
- Step 2: Go to the "Virtual Machines" section and select the VM for which you want to create a snapshot.
- Step 3: In the VM's menu, select "Disks".
- Step 4: Under the disk section, choose the disk you want to snapshot.
  
![snapshot](/img/vm/1/snap/snap1.png)

- Step 5: Click "Create Snapshot".
- Step 6: Provide a name for the snapshot and choose a resource group and storage type.
- Step 7: Click "Create" to complete the process
![snapshot](/img/vm/1/snap/snap2.png)
![snapshot](/img/vm/1/snap/snap3.png)

## Managing Snapshots using Azure CLI:

```bash
az snapshot create \
  --resource-group anass-resources \
  --name snapshotanass \
  --source /subscriptions/b04d2a75-007b-4b4b-8e8d-83f06f415f07/resourceGroups/anass-resources/providers/Microsoft.Compute/disks/UbuntuRodrigoCaro_OsDisk_1_64e2db3cadd840abb9ca75ffea31f783 \
  --location uksouth
```

- To get the disk Id we do execute this command :

```bash
az disk show --resource-group anass-resources --name UbuntuRodrigoCaro_OsDisk_1_64e2db3cadd840abb9ca75ffea31f783 --query id --output tsv
```

- To see all snapshots we have :

```bash
az snapshot list --output table
```

- To delete a snapshot :

```bash
az snapshot delete --resource-group anass-resources --name snapshotanass
```
