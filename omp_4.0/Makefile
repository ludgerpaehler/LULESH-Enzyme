# Default build suggestion of MPI + OPENMP with Clang on IBM (Power 8) + NVIDIA GPU machines.
# You might have to change the compiler name and flags.

SHELL = /bin/sh
.SUFFIXES: .cc .o

LULESH_EXEC = lulesh2.0

# Use the LLVM Compiler-toolchain
CC=clang
CXX = clang

SOURCES2.0 = \
	lulesh.cc \
	lulesh-comm.cc \
	lulesh-viz.cc \
	lulesh-util.cc \
	lulesh-init.cc
OBJECTS2.0 = $(SOURCES2.0:.cc=.o)

teams = 
ifdef TEAMS
teams = -DTEAMS=$(TEAMS)
endif

threads = 
ifdef THREADS
threads = -DTHREADS=$(THREADS)
endif

gpu =
ifdef USE_GPU
gpu = -DUSE_GPU=$(USE_GPU)
endif

mpi = -DUSE_MPI=0
ifdef USE_MPI
mpi = -DUSE_MPI=$(USE_MPI)
endif

# Tuning flags
CXXFLAGS = -fopenmp=libomp -O3 $(shared) $(teams) $(threads) $(gpu) -DUSE_MPI=0 -I/home/lpaehler/Work/Repositories/llvm-project-11.0.0/build/projects/openmp/runtime/src/ -Xclang -load -Xclang /home/lpaehler/Work/Repositories/Enzyme/enzyme/build/Enzyme/ClangEnzyme-11.so

LDFLAGS = -L/usr/local/cuda/nvvm/libdevice -L/home/lpaehler/Work/Repositories/Enzyme/build/lib

.cc.o: lulesh.h
	@echo "Building $<"
	$(CXX) -c $(CXXFLAGS) -o $@  $<

all: $(LULESH_EXEC)

lulesh2.0: $(OBJECTS2.0)
	@echo "Linking"
	$(CXX) -fopenmp=libomp $(OBJECTS2.0) $(LDFLAGS) -lomp -lomptarget -lstdc++ -lm -o $@

clean:
	/bin/rm -f *.o *~ *.tgt* $(OBJECTS) $(LULESH_EXEC)
	/bin/rm -rf *.dSYM

tar: clean
	cd .. ; tar cvf lulesh-2.0.tar LULESH-2.0 ; mv lulesh-2.0.tar LULESH-2.0

