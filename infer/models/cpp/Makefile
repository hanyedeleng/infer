# Copyright (c) 2015 - present Facebook, Inc.
# All rights reserved.
#
# This source code is licensed under the BSD style license found in the
# LICENSE file in the root directory of this source tree. An additional grant
# of patent rights can be found in the PATENTS file in the same directory.

ROOT_DIR = ../../..
include $(ROOT_DIR)/Makefile.config

CPP_MODELS_FILE = $(SPECS_LIB_DIR)/cpp_models
CPP_MODELS_SOURCES = $(shell find src/ -name "*.cpp")
C_MODELS_SOURCES = $(shell find src/c_src/ -name "*.c")
INFER_RESULTS = out/report.json

all: install

$(INFER_RESULTS): $(CPP_MODELS_SOURCES) $(C_MODELS_SOURCES) $(CLANG_DEPS)
# make clean in src/ in case $(CLANG_DEPS) have changed
	@$(MAKE) -C src clean
	$(call silent_on_success,\
	  $(INFER_BIN) -o $(@D) --models-mode --no-failures-allowed -- $(MAKE) -C src)

$(CPP_MODELS_FILE): $(INFER_RESULTS)
	@$(INSTALL_DATA) $(dir $(INFER_RESULTS))/specs/*.specs $(SPECS_LIB_DIR)
	@touch $@

install: $(CPP_MODELS_FILE)

clean:
	$(REMOVE) $(CPP_MODELS_FILE)
	$(REMOVE_DIR) $(dir $(INFER_RESULTS))
	@$(MAKE) -C src clean

.PHONY: all clean install
