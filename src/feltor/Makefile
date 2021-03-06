device=gpu

#configure machine
include ../../config/default.mk
include ../../config/*.mk
include ../../config/devices/devices.mk

INCLUDE+= -I../         # other src libraries
INCLUDE+= -I../../inc   # other project libraries

all: feltor_hpc feltor feltor_mpi manufactured feltordiag interpolate_in_3d

manufactured: manufactured.cu manufactured.h feltor.h implicit.h
	$(CC) $(OPT) $(CFLAGS) $< -o $@ $(INCLUDE) $(JSONLIB)

implicit_t: implicit_t.cu implicit.h  feltor.h implicit.h
	$(CC) $(OPT) $(CFLAGS) $< -o $@ $(INCLUDE) $(JSONLIB) -g -DDG_BENCHMARK

feltordiag: feltordiag.cu feltordiag.h
	$(CC) $(OPT) $(CFLAGS) $< -o $@ $(INCLUDE) $(LIBS) $(JSONLIB) -g
interpolate_in_3d: interpolate_in_3d.cu feltordiag.h
	$(CC) $(OPT) $(CFLAGS) $< -o $@ $(INCLUDE) $(LIBS) $(JSONLIB) -g

feltor: feltor.cu feltor.h implicit.h init.h parameters.h init_from_file.h
	$(CC) $(OPT) $(CFLAGS) $< -o $@ $(INCLUDE) $(GLFLAGS) $(JSONLIB) -g -DDG_BENCHMARK

feltor_hpc: feltor_hpc.cu feltordiag.h feltor.h implicit.h init.h parameters.h init_from_file.h
	$(CC) -g $(OPT) $(CFLAGS) $< -o $@ $(INCLUDE) $(LIBS) $(JSONLIB) -DDG_BENCHMARK

feltor_mpi: feltor_hpc.cu feltordiag.h feltor.h implicit.h init.h parameters.h init_from_file.h
	$(MPICC) $(OPT) $(MPICFLAGS) $< -o $@ $(INCLUDE) $(LIBS) $(JSONLIB) -DFELTOR_MPI -DDG_BENCHMARK

.PHONY: clean

clean:
	rm -f feltor feltor_hpc feltor_mpi manufactured feltordiag interpolate_in_3d
