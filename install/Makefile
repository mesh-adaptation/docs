# Makefile for installing Firedrake based on a PETSc installation including Mmg
# and ParMmg.

# Distribution-specific configuration
ifneq (,$(findstring Debian,$(shell cat /etc/os-release)))
DISTRO = debian
FIREDRAKE_CONFIGURE_OPTIONS :=
else ifneq (,$(findstring Red Hat,$(shell cat /etc/os-release)))
DISTRO = rhel
FIREDRAKE_CONFIGURE_OPTIONS = --no-package-manager
else ifneq (,$(findstring Ubuntu,$(shell cat /etc/os-release)))
DISTRO = ubuntu
FIREDRAKE_CONFIGURE_OPTIONS :=
else
DISTRO = other
FIREDRAKE_CONFIGURE_OPTIONS :=
endif

# Command line options
MPICC					?=
MPICXX					?=
MPIF90					?=
MPIEXEC					?=
FIREDRAKE_ENV			?= firedrake-$(shell date +%b%y | tr A-Z a-z)
BUILD_DIR 				?= $(shell pwd)
PETSC_CONFIGURE_FILE	?= petsc_configure_options.txt

# Environment variables
PETSC_CONFIGURE_OPTIONS="$(shell echo -n $(shell cat $(PETSC_CONFIGURE_FILE)))"

# Handle command line inputs
FIREDRAKE_CONFIGURE_OPTIONS += "--venv-name=$(FIREDRAKE_ENV)"
ifneq ($(MPICC),)
	ifeq ($(shell test -e $(MPICC) && echo exists),)
		$(error MPICC is defined but the path $(MPICC) does not exist)
	endif
	FIREDRAKE_CONFIGURE_OPTIONS += " --mpicc=$(MPICC)"
	PETSC_CONFIGURE_OPTIONS += " --CC=$(MPICC)"
endif
ifneq ($(MPICXX),)
	ifeq ($(shell test -e $(MPICXX) && echo exists),)
		$(error MPICXX is defined but the path $(MPICXX) does not exist)
	endif
	FIREDRAKE_CONFIGURE_OPTIONS += " --mpicxx=$(MPICXX)"
	PETSC_CONFIGURE_OPTIONS += " --CXX=$(MPICXX)"
endif
ifneq ($(MPIF90),)
	ifeq ($(shell test -e $(MPIF90) && echo exists),)
		$(error MPIF90 is defined but the path $(MPIF90) does not exist)
	endif
	FIREDRAKE_CONFIGURE_OPTIONS += " --mpif90=$(MPIF90)"
	PETSC_CONFIGURE_OPTIONS += " --FC=$(MPIF90)"
endif
ifneq ($(MPIEXEC),)
	ifeq ($(shell test -e $(MPIEXEC) && echo exists),)
		$(error MPIEXEC is defined but the path $(MPIEXEC) does not exist)
	endif
	FIREDRAKE_CONFIGURE_OPTIONS += " --mpiexec=$(MPIEXEC)"
	PETSC_CONFIGURE_OPTIONS += " --with-mpiexec=$(MPIEXEC)"
endif

.PHONY: all check_env install install_no_chk clean

all: clean install

check_env:
	@echo "Distribution=$(DISTRO)"
	@echo "MPICC=$(MPICC)"
	@echo "MPICXX=$(MPICXX)"
	@echo "MPIF90=$(MPIF90)"
	@echo "MPIEXEC=$(MPIEXEC)"
	@echo "BUILD_DIR=$(BUILD_DIR)"
	@echo "FIREDRAKE_ENV=$(FIREDRAKE_ENV)"
	@echo "FIREDRAKE_CONFIGURE_OPTIONS=$(FIREDRAKE_CONFIGURE_OPTIONS)"
	@echo "PETSC_CONFIGURE_OPTIONS=$(PETSC_CONFIGURE_OPTIONS)"
	@echo "python3=$(shell which python3)"
	@echo "Are these settings okay? Press any key to continue or Ctrl+C to exit."
	@read chk

# Install approach that doesn't require user input
install_no_chk:
	@cd $(BUILD_DIR); \
		export PETSC_CONFIGURE_OPTIONS=$(PETSC_CONFIGURE_OPTIONS); \
		curl -O https://raw.githubusercontent.com/firedrakeproject/firedrake/master/scripts/firedrake-install; \
		python3 firedrake-install $(FIREDRAKE_CONFIGURE_OPTIONS)

# Standard install approach that asks user to check environment setup
install: check_env

clean:
	@rm -f $(BUILD_DIR)/firedrake-install
	@rm -f $(BUILD_DIR)/firedrake-install.log
	@rm -rf $(BUILD_DIR)/$(FIREDRAKE_ENV)
