##----------------------------------------------------------------------------##
##
##   CARLsim3 Project Makefile
##   -------------------------
##
##   Authors:   Michael Beyeler <mbeyeler@uci.edu>
##              Kristofor Carlson <kdcarlso@uci.edu>
##
##   Institute: Cognitive Anteater Robotics Lab (CARL)
##              Department of Cognitive Sciences
##              University of California, Irvine
##              Irvine, CA, 92697-5100, USA
##
##   Version:   03/04/2017
##
##----------------------------------------------------------------------------##

################################################################################
# Start of user-modifiable section
################################################################################

# In this section, specify all files that are part of the project.

# Name of the binary file to be created.
# NOTE: There must be a corresponding .cpp file named main_$(proj_target).cpp!
proj_target    := random_spnet

# Directory where all include files reside. The Makefile will automatically
# detect and include all .h files within that directory.
proj_inc_dir   := .

# Directory where all source files reside. The Makefile will automatically
# detect and include all .cpp and .cu files within that directory.
proj_src_dir   := .

################################################################################
# End of user-modifiable section
################################################################################


#------------------------------------------------------------------------------
# Include configuration file
#------------------------------------------------------------------------------

# NOTE: If your CARLsim3 installation does not reside in the default path, make
# sure the environment variable CARLSIM3_INSTALL_DIR is set.
ifneq ($(CARLSIM3_INSTALL_DIR),)
	CARLSIM3_INC_DIR  := $(CARLSIM3_INSTALL_DIR)/inc
else
	CARLSIM3_INC_DIR  := /usr/local/include/carlsim
endif

# include compile flags etc.
include $(CARLSIM3_INC_DIR)/configure.mk


#------------------------------------------------------------------------------
# Build local variables
#------------------------------------------------------------------------------

main_src_file := $(proj_src_dir)/main_$(proj_target).cpp

# build list of all .cpp, .cu, and .h files (but don't include main_src_file)
cpp_files  := $(wildcard $(proj_src_dir)/*.cpp)
cpp_files  := $(filter-out $(main_src_file),$(cpp_files))
cu_files   := $(wildcard $(proj_src_dir)/src/*.cu)
inc_files  := $(wildcard $(proj_inc_dir)/*.h)

# compile .cpp files to -cpp.o, and .cu files to -cu.o
obj_cpp    := $(patsubst %.cpp, %-cpp.o, $(cpp_files))
obj_cu     := $(patsubst %.cu, %-cu.o, $(cu_files))
ifeq ($(CARLSIM3_NO_CUDA),1)
obj_files  := $(obj_cpp)
else
obj_files  := $(obj_cpp) $(obj_cu)
endif

# handled by clean and distclean
clean_files := $(obj_files) $(proj_target)
distclean_files := $(clean_files) results/* *.dot *.dat *.csv *.log


#------------------------------------------------------------------------------
# Project targets and rules
#------------------------------------------------------------------------------

.PHONY: $(proj_target) clean distclean help
default: $(proj_target)


$(proj_target): $(main_src_file) $(inc_files) $(obj_files)
	$(NVCC) $(CARLSIM3_FLG) $(obj_files) $< -o $@ $(CARLSIM3_LIB)

$(proj_src_dir)/%-cpp.o: $(proj_src_dir)/%.cpp $(inc_files)
	$(CXX) -c $(CXXINCFL) $(CXXFL) $< -o $@

$(proj_src_dir)/%-cu.o: $(proj_src_dir)/%.cu $(inc_files)
	$(NVCC) -c $(NVCCINCFL) $(SIMINCFL) $(NVCCFL) $< -o $@

clean:
	$(RM) $(clean_files)

distclean:
	$(RM) $(distclean_files)

help:
	$(info CARLsim3 tutorial options:)
	$(info )
	$(info make               Compiles tutorial code
	$(info make clean         Cleans out all object files)
	$(info make distclean     Cleans out all object and output files)
	$(info make help          Brings up this message)
