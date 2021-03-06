GREEN = '\033[1;32m'
YELLOW = '\033[1;33m'
NOCOLOR = '\033[0m'

# used to check for requirements
CHECK_IMAGETK = python3 -c "from PIL import Image, ImageTk" 
CHECK_SKIMAGE = python3 -c "import skimage" 

# name of the directory and the base name of the main python file (minus the .py extension)
APP_NAME = classifier_gui

.PHONY: all
all: deps data

.PHONY: deps
deps: default_model
	@echo $(YELLOW)'\n'${APP_NAME}": Making dependencies..."$(NOCOLOR)


.PHONY: data
data:
	@echo $(YELLOW)'\n'${APP_NAME}": Making required data..."$(NOCOLOR)
	(cd ../../data/ilsvrc12; make)


.PHONY: compile_default_model
compile_default_model: googlenet alexnet squeezenet
	@echo $(YELLOW)'\n'${APP_NAME}": Compiling default model to IR..."$(NOCOLOR)
	

.PHONY: default_model
default_model: compile_default_model
	@echo $(YELLOW)'\n'${APP_NAME}": Making default models..."$(NOCOLOR)


.PHONY: googlenet
googlenet: 
	@echo $(YELLOW)'\n'${APP_NAME}": Making googlenet..."$(NOCOLOR)
	@echo "Checking OpenVINO environment..."
	@if [ -z "$(INTEL_OPENVINO_DIR)" ] ; \
	then \
		echo "Please initiate the Intel OpenVINO environment by going to the installation directory for openvino and running the setupvars.sh file in the bin folder." ; \
		exit 1 ; \
	else \
		echo "Intel OpenVINO environment is already set!" ; \
	fi
	(cd ../../networks/googlenet_v1; make compile_model;)

.PHONY: alexnet
alexnet: 
	@echo $(YELLOW)'\n'${APP_NAME}": Making alexnet..."$(NOCOLOR)
	@echo "Checking OpenVINO environment..."
	@if [ -z "$(INTEL_OPENVINO_DIR)" ] ; \
	then \
		echo "Please initiate the Intel OpenVINO environment by going to the installation directory for openvino and running the setupvars.sh file in the bin folder." ; \
		exit 1 ; \
	else \
		echo "Intel OpenVINO environment is already set!" ; \
	fi
	(cd ../../networks/alexnet; make compile_model;)


.PHONY: squeezenet
squeezenet: 
	@echo $(YELLOW)'\n'${APP_NAME}": Making squeezenet..."$(NOCOLOR)
	@echo "Checking OpenVINO environment..."
	@if [ -z "$(INTEL_OPENVINO_DIR)" ] ; \
	then \
		echo "Please initiate the Intel OpenVINO environment by going to the installation directory for openvino and running the setupvars.sh file in the bin folder." ; \
		exit 1 ; \
	else \
		echo "Intel OpenVINO environment is already set!" ; \
	fi
	(cd ../../networks/squeezenet_v1.0; make compile_model;)
	

.PHONY: run
run: run_py
	
	
.PHONY: run_py
run_py: data data deps googlenet alexnet squeezenet 
	@echo $(YELLOW)'\n'${APP_NAME}": Running the sample..."$(NOCOLOR)
	@echo "Checking python3-pil.imagetk..."
	@if $(shell) ${CHECK_IMAGETK} 2> /dev/null; \
	then \
		echo " - python3-pil.imagetk already installed.\n"; \
	else \
		echo $(YELLOW)"ATTENTION! "$(NOCOLOR)"python3-pil.imagetk is required to run this sample.\n";\
		echo "Please use the command: "$(YELLOW)" make install-reqs "$(NOCOLOR)" to install the requirement."; \
		echo "or use the command:" $(YELLOW)"sudo apt-get install -y python3-pil.imagetk"$(NOCOLOR)" to manually install requirement.\n"; \
		exit 1;\
	fi	
	@echo "Checking scikit-image..."
	@if ${shell} ${CHECK_SKIMAGE} 2> /dev/null; \
	then \
	    echo " - scikit-image already installed.\n" ; \
	else \
		echo $(YELLOW)"\nATTENTION! "$(NOCOLOR)"scikit-image is required to run this sample.\n"; \
		echo "Please use the command: "$(YELLOW)" make install-reqs "$(NOCOLOR)" to install the requirement."; \
		echo "or use the command: " $(YELLOW)"pip3 install scikit-image --user"$(NOCOLOR)" to manually install requirement.\n"; \
		exit 1; \
	fi
	@if [ -z "$(INTEL_OPENVINO_DIR)" ] ; \
	then \
		echo "Please initiate the Intel OpenVINO environment by going to the installation directory for openvino and running the setupvars.sh file in the bin folder." ; \
		exit 1 ; \
	else \
		echo "Intel OpenVINO environment is already set!" ; \
	fi
	python3 ${APP_NAME}.py


.PHONY: install-reqs
install-reqs: 
	@echo $(YELLOW)'\n'${APP_NAME}": Checking application requirements...\n"$(NOCOLOR)
	@echo "\n Checking python3-pil.imagetk..."
	@if $(shell) ${CHECK_IMAGETK} 2> /dev/null; \
	then \
		echo " - python3-pil.imagetk already installed.\n"; \
	else \
		echo $(YELLOW)"Installing python3-pil.imagetk... "$(NOCOLOR); \
		sudo apt-get install python3-pil.imagetk; \
	fi; \
	
	@echo "\n Checking scikit-image..."
	@if ${shell} ${CHECK_SKIMAGE} 2> /dev/null; \
	then \
	    echo " - scikit-image already installed.\n" ; \
	else \
		echo $(YELLOW)"Installing scikit-image..."$(NOCOLOR); \
		pip3 install scikit-image --user; \
	fi
	
	
.PHONY: uninstall-reqs
uninstall-reqs: 
	@echo $(YELLOW)'\n'${APP_NAME}": Uninstalling requirements..."$(NOCOLOR)
	@echo $(YELLOW)"\n Checking python3-pil.imagetk..."$(NOCOLOR)
	@if $(shell) ${CHECK_IMAGETK} 2> /dev/null; \
	then \
		echo $(YELLOW)"\n - Uninstalling python3-pil.imagetk..."$(NOCOLOR); \
		sudo apt-get remove python3-pil.imagetk; \
		sudo apt autoremove -y; \
	else \
		echo " - Uninstall cancelled. Requirement is not installed."; \
	fi; \
	
	@echo $(YELLOW)"\n Checking scikit-image..."$(NOCOLOR)
	@if ${shell} ${CHECK_SKIMAGE} 2> /dev/null; \
	then \
		echo $(YELLOW)"\n - Uninstalling scikit-image..."$(NOCOLOR); \
		pip3 uninstall scikit-image; \
	else \
		echo " - Uninstall cancelled. Requirement is not installed.."; \
	fi
	
	
.PHONY: help
help:
	@echo "\nPossible make targets: ";
	@echo $(YELLOW)"  make run or run_py"$(NOCOLOR)"- runs the application";
	@echo $(YELLOW)"  make help "$(NOCOLOR)"- shows this message";
	@echo $(YELLOW)"  make all "$(NOCOLOR)"- makes everything needed to run but doesn't run";
	@echo $(YELLOW)"  make data "$(NOCOLOR)"- downloads data as needed";
	@echo $(YELLOW)"  make deps "$(NOCOLOR)"- makes/prepares dependencies";
	@echo $(YELLOW)"  make install-reqs "$(NOCOLOR)"- Installs requirements needed to run this sample on your system.";
	@echo $(YELLOW)"                      python3-pil.imagetk";
	@echo $(YELLOW)"                      scikit-image(aka skimage)";
	@echo $(YELLOW)"  make uninstall-reqs "$(NOCOLOR)"- Uninstalls requirements that were installed by the sample program.";
	@echo $(YELLOW)"  make default_model "$(NOCOLOR)"- compiles a default model to use when running";
	@echo $(YELLOW)"  make clean "$(NOCOLOR)"- removes all created content"
	@echo ""
	
	
.PHONY: clean_googlenet
clean_googlenet: 
	@echo $(YELLOW)'\n'${APP_NAME}": Cleaning up googlenet..."$(NOCOLOR)
	(cd ../../networks/googlenet_v1; make clean;)


.PHONY: clean_alexnet
clean_alexnet: 
	@echo $(YELLOW)'\n'${APP_NAME}": Cleaning up alexnet..."$(NOCOLOR)
	(cd ../../networks/alexnet; make clean;)


.PHONY: clean_squeezenet
clean_squeezenet: 
	@echo $(YELLOW)'\n'${APP_NAME}": Cleaning up squeezenet..."$(NOCOLOR)
	(cd ../../networks/squeezenet_v1.0; make clean;)
	
.PHONY: clean
clean: clean_alexnet clean_squeezenet clean_googlenet
	@echo $(YELLOW)'\n'${APP_NAME}": Cleaning up files..."$(NOCOLOR)
	
