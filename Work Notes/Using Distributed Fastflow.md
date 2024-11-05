Running DFF
- Use the Makefile
- Compile with mpicxx
- Issue dff_run specifying the protocol to use
- I can also use the json config

Experiment with these

Compiling with MakeFille Using MPI
- Set up the MPI_HOME and FF_HOME and edit the Makefile appropriately
- This command is run behind the scenes when compiling with mpicxx
	`mpicxx -I/opt/intel/oneapi/mpi/2021.10.0/include -I ~/fastflow -I ~/cereal  -I/home/j.markin/lib/cereal/include -std=c++20 -Wall -O3 -finline-functions -DNDEBUG -o test_group15 test_group15.cpp  -L/opt/intel/oneapi/mpi/2021.10.0/lib/release -lmpi -lmpifort -pthread`
- 

Running with dff_run
`dff_run -V -f test_group28.json ./test_group28`

if you want the printing of a single group (G1 for instance) 
`dff_run -v G1 -f test_group28.json ./test_group28`

Running with MPI

`mpirun -n 4 -ppn 2 dff_run -V  test_group1.json ./test_group1`

`I_MPI_OFI_PROVIDER=tcp I_MPI_DEBUG=4 LD_PRELOAD=/home/j.markin/mpiP_build/lib/libmpiP.so  mpirun -iface eth1 -n 4 -ppn 1 -f $HOME/torch_projects/mpi_hostfile_noslot_eth1 dff_run -V  test_group1.json ./test_group1`


mpirun -np 4  /home/j.markin/fastflow/tests/distributed/./test_group1  --DFF_Config=/home/j.markin/fastflow/tests/distributed/test_group1_mpi.json

`mpirun -machinefile $HOME/torch_projects/mpi_hostfile.txt -n 2 ./test_group27 --DFF_Config=test_group27.json`

{
    "protocol" : "MPI",
    "concurrency": "non-blocking",
    "groups" : [
    {   
        "endpoint" : "node01-ib0",
        "name" : "G1"
    },
    { 
      	"name" : "G2",
        "endpoint": "node02-ib0"
    }
    ]
}


Compiling the DFF Program
`mpicxx -I ~/fastflow -I ~/cereal  -I/home/j.markin/lib/cereal/include -std=c++20 -Wall -O3 -finline-functions -DNDEBUG -o simple_pipeline simple_pipeline.cpp  -pthread`

Running the DFF Program
`mpirun -np 2 --host node01,node02 simple_pipeline  --DFF_Config=simple_pipeline.json`

Content of JSON file should have the ports attached to the nodes and have no protocol and concurrency specified

`mpicxx -I/opt/intel/oneapi/mpi/2021.10.0/include -I ~/fastflow -I/home/j.markin/lib/cereal/include -std=c++20 -Wall -O3 -finline-functions -DNDEBUG -o simple_pipeline simple_pipeline.cpp  -L/opt/intel/oneapi/mpi/2021.10.0/lib/release -lmpi -lmpifort -pthread`


Possible Runnig with OPX
`mpirun --mca pml cm --mca mtl psm2 -np 2 --host node01,node02 simple_pipeline --DFF_Config=simple_pipeline.json`

### Explanation of Parameters

- **`--mca pml cm`**: This tells OpenMPI to use the **Connection Management (CM)** layer, which enables support for high-performance fabrics like Omni-Path.
- **`--mca mtl psm2`**: This specifies the **psm2** Multi-Transport Layer (MTL), which is the protocol that supports Omni-Path.


The `--mca` flag allows you to configure the network interface OpenMPI uses.



The `--mca` flag allows you to configure the network interface OpenMPI uses.

In OpenMPI, you can specify the network interface for communication using the `btl` (Byte Transfer Layer) or `mca` (Modular Component Architecture) parameters. This can be especially useful for selecting a high-performance network interface like InfiniBand or a specific Ethernet interface.

Here's how to specify the network interface in OpenMPI:

### 1. Using `--mca` Parameters
The `--mca` flag allows you to configure the network interface OpenMPI uses.

#### Ethernet Interface Example
To specify an Ethernet interface (e.g., `eth0` or `enp0s3`), use:
```bash
mpirun --mca btl_tcp_if_include eth0 -np 2 --host node01,node02 simple_pipeline --DFF_Config=simple_pipeline.json
```

Here:
- `--mca btl_tcp_if_include eth0` tells OpenMPI to use `eth0` for TCP communication.

#### InfiniBand Interface Example
If you want to use InfiniBand (typically using the `openib` protocol in OpenMPI), use:
```bash
mpirun --mca btl openib,self,vader --mca btl_openib_if_include mlx5_0 -np 2 --host node01,node02 simple_pipeline --DFF_Config=simple_pipeline.json
```

Here:
- `--mca btl openib,self,vader` specifies the Byte Transfer Layer to use InfiniBand (`openib`), shared memory (`self`), and intra-node communication (`vader`).
- `--mca btl_openib_if_include mlx5_0` restricts OpenMPI to the `mlx5_0` InfiniBand interface.

> **Note**: Replace `mlx5_0` with the appropriate InfiniBand device name for your system (e.g., `ib0` or similar).

### 2. Checking Available Interfaces
To see the network interfaces available on your system, you can use:
```bash
ifconfig
```
or
```bash
ip a
```

### 3. Alternative: Using an MCA Parameter File
If you use the same interface consistently, you can specify it in an MCA parameter file, typically located at `~/.openmpi/mca-params.conf`:
```plaintext
btl_tcp_if_include = eth0
btl_openib_if_include = mlx5_0
```

With this setup, you don’t need to specify `--mca` parameters each time you run `mpirun`.