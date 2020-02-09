#
# PROJECT AND TARGET
#

MCU            = stm32f446re
TARGET         = exploring_$(MCU)
CPU_INFO       = -mcpu=cortex-m4 -mfpu=fpv4-sp-d16 -mfloat-abi=hard -mthumb
OPTIM          = -O0
STARTUP        = startup_$(MCU).s
LD_SCRIPT_PATH = ./ld/
LD_SCRIPT      = ldscript_$(MCU).ld

ifeq ($(REL),)
BUILD_DIR      = Debug
else
BUILD_DIR      = Release
endif

# Add here your preprocessor macros 
MACRO_DEFS =  \

# Add here your C++ source files (.cpp)
CXX_SRC = \

# Add here your C source files (.c) 
C_SRC =  \
./main.c \

# Add here your assembly source files (.s .S)
ASM_SRC =  \
./src/$(STARTUP)

# Add here the location of your header files (.h)
INCS =  \
-I./inc \

# Add here your stastic libraries (.a)
LIBS = \


#
# GNU TOOLCHAIN
#

GCC_PATH = ../toolchain/gcc-arm-none-eabi-9-2019-q4-major

CC    = $(GCC_PATH)/bin/arm-none-eabi-gcc
CXX   = $(GCC_PATH)/bin/arm-none-eabi-c++
AS    = $(GCC_PATH)/bin/arm-none-eabi-gcc -x assembler-with-cpp
LD    = $(GCC_PATH)/bin/arm-none-eabi-gcc
CP    = $(GCC_PATH)/bin/arm-none-eabi-objcopy
SZ    = $(GCC_PATH)/bin/arm-none-eabi-size
DUMP  = $(GCC_PATH)/bin/arm-none-eabi-objdump
RDELF = $(GCC_PATH)/bin/arm-none-eabi-readelf

# C Compiler Flags
CFLAGS  = -std=c99
CFLAGS += -Wall
CFLAGS += -fdata-sections -ffunction-sections
CFLAGS += $(CPU_INFO)
CFLAGS += $(OPT)
CFLAGS += $(MACRO_DEFS) 
CFLAGS += $(INCS)

# C++ Compiler Flags
CXXFLAGS  = -std=c++11
CXXFLAGS += -Wall
CXXFLAGS += -fdata-sections -ffunction-sections
CXXFLAGS += -fno-rtti -fno-exceptions
CXXFLAGS += $(CPU_INFO)
CXXFLAGS += $(OPT)
CXXFLAGS += $(MACRO_DEFS) 
CXXFLAGS += $(INCS)

ifeq ($(REL),)
CFLAGS   += -g -gdwarf-2
CXXFLAGS += -g -gdwarf-2
endif

# Linker Flags
LDFLAGS  = -L$(LD_SCRIPT_PATH) -T$(LD_SCRIPT)
LDFLAGS += -Wl,-Map=$(BUILD_DIR)/$(TARGET).map,--cref
LDFLAGS += -lc -lm -lnosys
LDFLAGS += -Wl,--gc-sections
LDFLAGS += --specs=nano.specs
LDFLAGS += $(CPU_INFO)

#
# BUILD PROCESS
#

OBJS = $(addprefix $(BUILD_DIR)/,$(notdir $(CXX_SRC:.cpp=.o)))
vpath %.cpp $(sort $(dir $(CXX_SRC))) 

OBJS += $(addprefix $(BUILD_DIR)/,$(notdir $(C_SRC:.c=.o)))
vpath %.c $(sort $(dir $(C_SRC)))

OBJS += $(addprefix $(BUILD_DIR)/,$(notdir $(ASM_SRC:.s=.o)))
vpath %.s $(sort $(dir $(ASM_SRC)))

# default target 
all: $(BUILD_DIR)/$(TARGET).elf $(BUILD_DIR)/$(TARGET).hex $(BUILD_DIR)/$(TARGET).bin

$(BUILD_DIR)/%.o: %.cpp | $(BUILD_DIR)
	$(CXX) -c $(CXXFLAGS)  $< -o $@

$(BUILD_DIR)/%.o: %.c | $(BUILD_DIR)
	$(CC) -c $(CFLAGS)  $< -o $@

$(BUILD_DIR)/%.o: %.s | $(BUILD_DIR)
	$(AS) -c $(CFLAGS) $< -o $@

$(BUILD_DIR)/$(TARGET).elf: $(OBJS)
	$(LD) $(OBJS) $(LDFLAGS) -o $@
	$(SZ) $@ 

$(BUILD_DIR)/%.hex: $(BUILD_DIR)/%.elf | $(BUILD_DIR)
	 $(CP) -O ihex $< $@
	
$(BUILD_DIR)/%.bin: $(BUILD_DIR)/%.elf | $(BUILD_DIR)
	$(CP) -O binary -S $< $@	
	
$(BUILD_DIR):
	mkdir $@		

memdump: $(BUILD_DIR)/$(TARGET).elf
	$(DUMP) -D -g $(BUILD_DIR)/$(TARGET).elf

readelf: $(BUILD_DIR)/$(TARGET).elf
	$(RDELF) -a $(BUILD_DIR)/$(TARGET).elf

.PHONY: help
help:
	@echo TARGETS:
	@echo make             - Build the project in debug configuration.
	@echo make REL=0       - Build the project in release configuration.
	@echo make memdump     - Display insformation about the .elf objet file.
	@echo make readelf     - Display complete insformation about the .elf objet file.
	@echo make clear       - Clean the output files generated in debug configuration.
	@echo make clear REL=0 - Clean the output files generated in release configuration.
	@echo make help        - Show the targets list.

.PHONY: clean
clean:
	-rm -fR Debug
	-rm -fR Release