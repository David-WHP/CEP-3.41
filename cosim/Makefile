#//************************************************************************
#// Copyright 2021 Massachusetts Institute of Technology
#// SPDX short identifier: BSD-2-Clause
#//
#// File Name:      Makefile
#// Program:        Common Evaluation Platform (CEP)
#// Description:    
#// Notes:          
#//
#//************************************************************************

all: runAll

# Always in regression mode
override REGRESS = 1
override NOWAVE  = 1
override BOARD_SIM = 1
#override COVERAGE  = 0
UPDATE_INFO = 1
#
#
COSIM_NAME  = $(shell basename `pwd`)
#
DUT_TOP_DIR = $(shell cd ..; pwd | ./${COSIM_NAME}/bin/strip_net.pl )
BLD_DIR     = $(shell cd ./bfmTests; pwd | ../bin/strip_net.pl )
TEST_SUITE  = $(shell basename ${BLD_DIR})
TOP_DIR     = $(shell cd .; pwd | ./bin/strip_net.pl )
SIM_DIR     = ${DUT_TOP_DIR}/${COSIM_NAME}

#
# Either run one at a time or batch the whole thing if supported (TBA)
#

TEST_GROUP  =  	bareMetalTests	\
		bfmTests	\
		isaTests	\
		vBareTest	\

runAll: 
	make cleanAll
	make buildLibs
	@for i in ${TEST_GROUP}; do				\
		rm -f $${i}/$${i}.log;				\
		(cd $${i}; make STAND_ALONE=0 runAll); 		\
	done

summary: .force
	@date > ${TOP_DIR}/regressionSummary
	@for i in ${TEST_GROUP}; do								\
		(cd $${i}; make -s STAND_ALONE=0 summary | sort | tee -a ${TOP_DIR}/regressionSummary);	\
	done

#
# override anything here before calling the common file
#
MKFILE_DIR ?= .
include $(MKFILE_DIR)/common.make


