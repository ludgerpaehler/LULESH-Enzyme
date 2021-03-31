NVCC		= /home/lpaehler/Work/Repositories/llvm-project/build/bin/clang++
# FLAGS		= -arch=sm_86
# DFLAGS	= -lineinfo
RFLAGS 	= -O3 -DNDEBUG 
CFLAGS  = -v -fno-exceptions -mllvm -enzyme-print -Rpass=enzyme -fno-experimental-new-pass-manager -std=c++17 --cuda-path=/usr/local/cuda-11.2 -L/usr/local/cuda-11.2/lib64 --cuda-gpu-arch=sm_80 --no-cuda-version-check -Xclang -load -Xclang /home/lpaehler/Work/Repositories/Enzyme/enzyme/build13/Enzyme/ClangEnzyme-13.so
LDFLAGS = -lcudart_static -ldl -lrt -lpthread -lm 


#SILO_INCLUDES := /usr/local/silo-4.8/include
#SILO_LIBS := /usr/local/silo-4.8/lib

#LINKFLAGS = -lmpich -L$(MPICH_DIR)/lib 
#LINKFLAGS += -L$(SILO_LIBS) -lsilo

#INC_SILO:= -I$(SILO_INCLUDES)

all: release 

debug: LINKFLAGS += 

release: 	FLAGS += $(RFLAGS)
debug: 		FLAGS += $(DFLAGS)

release: lulesh
debug: lulesh

lulesh: allocator.o lulesh.o lulesh-comms.o lulesh-comms-gpu.o
	$(NVCC) $(CFLAGS) allocator.o lulesh.o lulesh-comms.o lulesh-comms-gpu.o -o lulesh $(LDFLAGS)

allocator.o: allocator.cu vector.h
	$(NVCC) $(CFLAGS) allocator.cu -I ./ -c -o allocator.o

lulesh.o: lulesh.cu util.h vector.h allocator.h
	$(NVCC) $(CFLAGS) lulesh.cu -I ./  $(INC_SILO) -c -o lulesh.o

lulesh-comms.o: lulesh-comms.cu
	$(NVCC) $(CFLAGS) lulesh-comms.cu -I ./ -c -o lulesh-comms.o

lulesh-comms-gpu.o: lulesh-comms-gpu.cu
	$(NVCC) $(CFLAGS) lulesh-comms-gpu.cu -I ./ -c -o lulesh-comms-gpu.o

clean: 
	rm -rf allocator.o lulesh-comms.o lulesh-comms-gpu.o lulesh.o lulesh xyz.asc regNumList.txt
