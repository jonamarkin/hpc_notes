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

`mpicxx -I/opt/intel/oneapi/mpi/2021.10.0/include -I ~/fastflow -I/home/j.markin/lib/cereal/include -std=c++20 -Wall -O3 -finline-functions -DNDEBUG -o simple_pipeline simple_pipeline.cpp  -L/opt/intel/oneapi/mpi/2021.10.0/lib/release -lmpi -lmpifort -pthread`
