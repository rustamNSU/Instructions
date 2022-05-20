
# Connect to remote server via ssh connection
1. install openssh (or putty for windows)

2. add (or create) sscc configuration into ~/.ssh/config
```bash
Host sscc_abdullin
HostName nks-1p.sscc.ru
IdentityFile /Users/rustam/.ssh/sscc_abdullin
User abdullin_r_f
StrictHostKeyChecking no
AddKeysToAgent yes
ServerAliveInterval 120
```
where Host -- title for convenience, IdentityFile -- path to private key (in openssh format, rsa 3072)

3. Run session via command
```bash
ssh sscc_abdullin
```

4. If you can't connect to remote server with the error ending as "Please make sure you have the correct access rights and the repository exists." (when coping private key from telegram or from pc to server) change access mode of private key
```bash
chmod 400 ~/.ssh/<private key>
```


# Clone private git repository via ssh key (eg. from gitlab self-hosted repository)
1. Add ssh key-pair in gitlab account ```Profile > Preferences > SSH Keys```. Add existing pair or generate new like
```bash
ssh-keygen  -t rsa -b 3072 -f gpn-gitlab-host-key
```
2. add gitlab ssh configuration into ~/.ssh/config (and change access mod, see 4)

3. clone repository using ssh ```git clone <ssh url>``` eg
```bash
git clone git@gitlab-gpn-noc.nsu.ru:hydrofracteam/fracturing_simulators/poroelasticity/poroelastic_frac_simulator_2d.git
```

# FreeFEM 4.11 installing from source **FAILED**
1. Install latest version of cmake
```bash
wget https://cmake.org/download/#:~:text=cmake%2D3.23.1%2Dlinux%2Dx86_64.tar.gz
tar -xvzf cmake-3.23.1-linux-x86_64.tar.gz -C /home/fano.hydro/abdullin_r_f/builds
```

2. add cmake to PATH
```bash
export PATH="/home/fano.hydro/abdullin_r_f/builds/cmake-3.23.1-linux-x86_64/bin:$PATH"
```

3. create alias to new version of cmake
```bash
alias cmake="/home/fano.hydro/abdullin_r_f/builds/cmake-3.23.1-linux-x86_64/bin/cmake"
```

4. clone FreeFem-sources
```bash
git clone https://github.com/FreeFem/FreeFem-sources
cd FreeFem-sources
git show
``` 
```git show``` for check latest commit

## **GCC + OpenMPI**
5. load **GCC** and **OpenMPI** modules
```bash
module purge
module load mpi/openmpi3-x86_64
scl enable devtoolset-9 bash
```
(not necessary) ```export CC=gcc $$ export CXX=g++ $$ export FC=gfortran $$ export F77=gfortran $$ export MPICXX=mpic++ $$ export MPIFC=mpifort $$ export MPICC=mpicc $$ export MPIRUN=mpirun```


## **INTEL compilers**
5. load **INTEL** modules and export compilers flags
```bash
module purge
module load oneapi/2021.2/ALL 
scl enable devtoolset-9 bash
```
```bash
export CC=icc \
$$ export CXX=icpc \
$$ export FC=ifort \
$$ export F77=ifort \
$$ export MPICXX=mpiicpc \
$$ export MPIFC=mpiifort \
$$ export MPICC=mpiicc \
$$ export MPIRUN=mpiexec
```
or ```export MPIRUN=/opt/software/intel/2021.2/mpi/2021.2.0/bin/mpiexec```

## building FreeFEM 4.11
6. In FreeFem-source directory do
```bash
autoreconf -i
./configure --enable-download --enable-optim --with-mpi --with-hdf5=yes prefix=/home/fano.hydro/abdullin_r_f/builds/freefem-4.11
./3rdparty/getall -a
```
for PETSc
```bash
cd 3rdparty/ff-petsc
make petsc-slepc
cd -
./reconfigure
```

7. building source code
```bash
make
make check
make install
```